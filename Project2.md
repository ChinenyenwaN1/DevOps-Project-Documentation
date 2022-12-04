# WEB STACK IMPLEMENTATION (LEMP STACK) ON AWS
In a LEMP stack, **L** stands for Linux, **E** is for Nginx (engine-x), **M** is for MySQL, and **P** stands for PHP, Perl or Python. The LEMP software stack can be used to serve dynamic web pages and web applications. 
Nginx helps to increase the ability of the server to scale in response to demand which is capable of handling huge traffic, this makes LEMP the first choice for hosting companies. MySQL is used for database and PHP for loading dynamic web pages on the browser.

To get started on how to install LEMP, you should be able to spin up a server as shown below from AWS account by now if you have followed me on my first project on how to install LAMP. 

![image](https://user-images.githubusercontent.com/116161693/205497448-26f5d72d-4945-4fa2-b139-63860d2f14dd.png)
 

**Using Mobaxterm**
Remember the private key you downloaded from AWS while provisioning the server? It is a PEM file format.
You can open it up to see the content and have a glimpse of what a PEM file looks like.
Now, we are going to use that PEM key to connect to our EC2 Instance via ssh using a third party tool called Mobaxterm to connect to our Instance. Username is Ubuntu, remote host: your instance IP address, click on use private key and locate the PEM file from your document and click on ok.

![image](https://user-images.githubusercontent.com/116161693/205497468-4ad83f25-1db0-4cd3-98c9-7fe190593d5c.png)

## STEP 1 – INSTALLING THE NGINX WEB SERVER
To start, update server’s package index. Afterwards, use apt install to get the Nginx installation going. To start the update run: 
```
sudo apt update
```

![image](https://user-images.githubusercontent.com/116161693/205497512-94550f75-1c71-4b6b-8a96-049c12de4fd2.png)

Next, install Nginx by running:
```
sudo apt install nginx
```

![image](https://user-images.githubusercontent.com/116161693/205497591-e3324702-5ae3-4060-aec5-371a1eb1cac0.png)

At the prompt, enter Y to confirm that you want to install Nginx. This would complete the installation process.
To confirm that nginx was successfully installed and is running as a service in Ubuntu, run: 
```
sudo systemctl status nginx
```

![image](https://user-images.githubusercontent.com/116161693/205497624-b56bb7cc-3b82-493c-9681-941cd47e41c2.png)

Green indicator shows that the server was successfully installed and running. 
The server is running and we can access it locally and from the Internet but to access it from the internet port 80 must be open to allow traffic from the internet in.
To test that the server can be accessed locally from the instance run the curl command: 
```
curl http://localhost:80
or
curl http://127.0.0.1:80
```

![image](https://user-images.githubusercontent.com/116161693/205497666-1be5c5a2-f7c0-4065-b2d9-32764ce21ddc.png)

To test that the server can be accessed from the internet, open a browser and type the following url with the public IP of your Ubuntu instance; http://Public-IP-Address:80 

![image](https://user-images.githubusercontent.com/116161693/205497684-de23ab1e-ed08-45aa-916a-cd0b7dfa8ce3.png)

# STEP 2 — INSTALLING MYSQL
To acquire and install SQL run: 
```
sudo apt install mysql-server 
```
![image](https://user-images.githubusercontent.com/116161693/205497710-4e6fe8ad-1955-437c-8ebc-5ea42ca1b3c6.png)
 
At the prompt, confirm installation by typing Y and enter to proceed.
Next, log into MySQL:
```
sudo mysql 
```

![image](https://user-images.githubusercontent.com/116161693/205497739-3601200e-5c71-4386-8703-ae240f7b55ce.png)

Run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. run the following command: 
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```

![image](https://user-images.githubusercontent.com/116161693/205497759-458480f8-54c8-4b1a-a527-e270b7415cd7.png)
 
Exit SQL shell by typing exit
Start interactive scripting to configure the validate password plugin. Run: 
```
sudo mysql_secure_installation
```
Answer Y to all the prompts. At the point you can change the password of your root user and also decide the level of password validation.
When done, test to know if login to the console is possible. type: 
```
sudo mysql -p 
```

![image](https://user-images.githubusercontent.com/116161693/205497795-76716e11-3dd6-4fdd-af5d-ae95d59f68d1.png)

This would prompt you to enter root user password. Enter the new password and enter.
type exit to exit MySQL console.
# STEP 3 – INSTALLING PHP
PHP has to be installed to process code and generate dynamic content for the web server.
Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. You’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.
To install the 2 pacakages at once, run: 
```
sudo apt install php-fpm php-mysql
```

![image](https://user-images.githubusercontent.com/116161693/205497892-bcdd9e8a-2bb6-44df-ad4d-f2d9bb0f19e9.png)

Type Y to confirm installation and enter.
# STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR
Now that we have PHP components installed. Next, I will configure Nginx to use them.
Create the root web directory for your_domain as follows: 
```
sudo mkdir /var/www/projectLEMP 
```
Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user: 
```
sudo chown -R $USER:$USER /var/www/projectLEMP
```

![image](https://user-images.githubusercontent.com/116161693/205497924-fa6cf9bf-5e54-4c3a-97b3-15947376aa66.png)

Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano: 
```
sudo nano /etc/nginx/sites-available/projectLEMP
```
This will create a new blank file. Paste in the following bare-bones configuration:
```
server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }
}
```

![image](https://user-images.githubusercontent.com/116161693/205497951-1275330c-8f18-44ce-9bff-a26bbd45ec81.png)
 
In the nano editor, enter CTRL+X to exit and Y to confirm.
Activate the configuration by linking to the config file from Nginx’s sites-enabled directory, run: 
```
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
test your configuration for syntax errors by typing: 
```
sudo nginx -t 
```

![image](https://user-images.githubusercontent.com/116161693/205497980-5809d33a-5897-4537-bd0e-0bcd5d9884c6.png)

We need to disable default Nginx host that is currently configured to listen on port 80, for this run: 
sudo unlink /etc/nginx/sites-enabled/default
Next, reload Nginx to apply the changes: 
```
sudo systemctl reload nginx 
```
The website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that the new server block works as expected: 
```
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```

![image](https://user-images.githubusercontent.com/116161693/205498001-9a3fa2c9-4d42-4996-803a-ec530798e567.png)

Open a browser and try to open the website URL using IP address: http://Public-IP-Address:80 
This is my output;

![image](https://user-images.githubusercontent.com/116161693/205498017-23b65711-4276-424a-886d-58016d8ccab5.png)

# STEP 5 – TESTING PHP WITH NGINX
Now that LAMP stack is completely installed and fully operational. We test it to validate that Nginx can correctly hand .php files off to your PHP processor.
Open a new file called info.php within your document root in your text editor: 
```
sudo nano /var/www/projectLEMP/info.php
```
Type the following lines into the new file.
```
<?php
phpinfo();
```
You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php: http://server_domain_or_IP/info.php 

![image](https://user-images.githubusercontent.com/116161693/205498042-69995dbf-0a5d-40ad-938d-83500c930b9d.png)

Remove the created file, as it contains sensitive information about your PHP environment and your Ubuntu server: 
```
sudo rm /var/www/your_domain/info.php
```
# STEP 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)
The goal in this step is to create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.
Because the native MySQL PHP library mysqlnd currently doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. We’ll need to create a new user with the mysql_native_password authentication method in order to be able to connect to the MySQL database from PHP.
I will be creating a database named example_database and a user named example_user.
Connect to the MySQL console using the root account: 
```
sudo mysql -p
```

![image](https://user-images.githubusercontent.com/116161693/205498072-c1f37d41-d227-412f-b0e9-4c7ea9db7b51.png)

To create a new database, run the following command from your MySQL console: 
```
CREATE DATABASE example_database; 
```

![image](https://user-images.githubusercontent.com/116161693/205498099-01a538be-a1a0-4041-8fe6-f7f9885a392c.png)

Next, create a new user and grant him full privileges on the database. The following command creates a new user named example_user, using mysql_native_password as default authentication method. Run: ```
CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord1@'; 
```

![image](https://user-images.githubusercontent.com/116161693/205498139-3c5449ef-51dc-4376-b999-4fbbc53ca371.png)

Next, give this user permission over the example_database database: 

```
GRANT ALL ON example_database. * TO 'example_user'@'%';*
```
 
![image](https://user-images.githubusercontent.com/116161693/205498168-0b622d91-dcc6-45ef-957c-4a8662618dba.png)

This will give the example_user user full privileges over the example_database database, while preventing this user from creating or modifying other databases on your server.
Exit the console: exit
Now test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials: 
mysql -u example_user -p 

![image](https://user-images.githubusercontent.com/116161693/205498192-69f9d21b-18ba-4476-8ba1-66c41e2ea8cc.png)

After logging in to the MySQL console, confirm that you have access to the example_database database:

```
SHOW DATABASES; 
```

![image](https://user-images.githubusercontent.com/116161693/205498209-45b1892e-f54f-480c-9aaf-8ccc92a73e0a.png)

Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:
```
CREATE TABLE example_database.todo_list (
item_id INT AUTO_INCREMENT ,
content VARCHAR(255) ,
PRIMARY KEY(item_id)
);
```

![image](https://user-images.githubusercontent.com/116161693/205498232-2f3a9e20-8ec7-4fb6-b0e2-37276edf5601.png)

Insert a few rows of content in the test table. Repeat the next command a few times, using different VALUES: 

```
INSERT INTO example_database.todo_list (content) VALUES ("Shop");
```
![image](https://user-images.githubusercontent.com/116161693/205498307-d2030713-e57c-4c99-8fb5-a920aed9f061.png)
 
To confirm that the data was successfully saved to your table, run: 
```
SELECT * FROM example_database.todo_list; 
```

![image](https://user-images.githubusercontent.com/116161693/205498332-96bcb612-f42c-437f-ada3-7f7a55cee526.png)

After confirming, exit the console: exit
Next, create a PHP script that will connect to MySQL and query for your content. Create a new PHP file in your custom web root directory using your preferred editor. We’ll use vi for that: 
```
nano /var/www/projectLEMP/todo_list.php
```
Copy this content into your todo_list.php script:
```
<?php
$user = "example_user";
$password = "PassWord1@";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
```

![image](https://user-images.githubusercontent.com/116161693/205498445-8b063340-fc62-459d-9c48-84502ec6a868.png)

Save and close the file when you are done editing.
Access this page in the web browser by visiting the domain name or public IP address configured for the website, followed by /todo_list.php: http://<Public_domain_or_IP>/todo_list.php 
You should see a page like this, showing the content you’ve inserted in your test table:

![image](https://user-images.githubusercontent.com/116161693/205498479-ab5626bf-cb57-493c-b6de-06fe21a9b2a9.png)
 
That means your PHP environment is ready to connect and interact with your MySQL server.

**Congratulations!** we have built a flexible foundation for serving PHP websites and applications to our visitors, using Nginx as web server and MySQL as database management system.

