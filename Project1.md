# WEB STACK IMPLEMENTATION (LAMP) IN AWS
A technology stack is a set of frameworks and tools used to develop a software product. This set of frameworks and tools are very specifically chosen to work together in creating a well-functioning software. some examples are…

1. LAMP (Linux, Apache, MySQL, PHP, Python, or Perl)
2. LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)
3. MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
4. MEAN (MongoDB, ExpressJS, AngularJS, NodeJS

For today’s project we will be focusing on LAMP STACK implementation in AWS
**A LAMP stack** is a bundle of four different software technologies that developers use to build websites and web applications. it delivers the best cost efficiency, flexibility, and high-performance web applications. It is an acronym that stands for:

1. **Linux**     – the Operating System
2. **Apache** -  the web server
3. **MySQL**  - the relational database management system
3. **PHP**      -  the programming language (it can also be Perl or Python).

To complete this project, you will need an [AWS account](https://aws.amazon.com/) in order to create a virtual environment. Create an IAM user which is the recommended practice for your daily use of resources and sign in as an IAM user.
![image](https://user-images.githubusercontent.com/116161693/203518500-52944cd2-0c66-49c5-8544-ca7f39d7a2c0.png)

Before proceeding with the creation of an EC2 instance, consider Cost, Latency and proximity, Security and regulatory compliance and Service Level Agreements (SLA) to select your desired region. AWS Region is at the top right hand close to your account ID
![image](https://user-images.githubusercontent.com/116161693/203466803-2c433c6d-19a3-470c-9a7a-a45f688f6a1f.png)

To spin up a virtual server click on "Services" at the upper left corner and you will see all the Services available on AWS. Click on "EC2"  
![image](https://user-images.githubusercontent.com/116161693/203466841-00ba6fba-6e5a-4270-bad8-1365c4ed1f94.png)

Next, click on "Launch Instances".
Now we are going to configure our EC2 instance! Select the Ubuntu Server 22.04 LTS (HVM) as the Amazon Machine Image (AMI).  
![image](https://user-images.githubusercontent.com/116161693/203466891-d5342bec-2f2b-4a90-9a1a-09a6c449b7dc.png)

Then, select "t2.micro" as the instance type.  

Next, create a keypair which is required for SSH access to the Server.

**IMPORTANT** - save your private key (.pem file) securely and do not share it with anyone! If you lose it, you will not be able to connect to your server ever again! 
![image](https://user-images.githubusercontent.com/116161693/203466937-28970af4-cb0d-4d7a-af9a-32c4cc48840e.png) 

**Good job!** You have successfully launched an EC2 instance! You can view your new instance by clicking the "View Instances" button at the bottom-right of your screen. this may take minutes to initialize and pass a status check. 
![image](https://user-images.githubusercontent.com/116161693/203467133-378d2605-3926-4dc5-a9ec-ff9ae31ed853.png)

Now let's connect to our instance! 
![image](https://user-images.githubusercontent.com/116161693/203492058-8373b47d-ba4c-4984-9607-05e1a3767700.png)

The next phase is to view your instance state, click on your instance to view it. At this stage, you will see if your instance is running or still pending. mine is running and status check has passed, now i can view my instance details.
![image](https://user-images.githubusercontent.com/116161693/203492102-e40cb0da-bb6f-4ebb-a144-694dd2fa5161.png)

In this project, I will implement a web solution based on LAMP stack on a Linux server by implementing the steps below:

## STEP 1 - Installing Apache and Updating the Firewall**
#### Using Mobaxterm
Remember the private key you downloaded from AWS while provisioning the server? It is a PEM file format. 
You can open it up to see the content and have a glimpse of what a PEM file looks like.

Now, we are going to use that PEM key to connect to our EC2 Instance via ssh using a third party tool called Mobaxterm to connect to our Instance. Username is Ubuntu, remote host: your instance IP address

![image](https://user-images.githubusercontent.com/116161693/203492293-47a4c83b-14f8-4f96-85af-c23673351adb.png)

We need to Install Apache using Ubuntu’s package manager, apt:
Run the below command to update a list of packages in package manager

```
sudo apt update
```
![image](https://user-images.githubusercontent.com/116161693/203493251-ffd6392f-83d8-48e3-8756-9815b6d6807d.png) 
run apache2 package installation command below

```
sudo apt install apache2
```
![image](https://user-images.githubusercontent.com/116161693/203493295-4f49f0ce-8d00-4c6c-a967-a8e81bdefd8d.png)
To verify that apache2 is running as a Service in our OS, use the below command

```
sudo systemctl status apache2** 
```
![image](https://user-images.githubusercontent.com/116161693/203493318-5b1b3694-cdc7-497a-8e77-6c055ac74882.png)

If it is green and running, then you did everything correctly – you have just launched your first Web Server in the Cloud! Congrats!

Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web browser uses to access web pages on the Internet. As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to open inbound connection through port 80:
Our server is running and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’). 
![image](https://user-images.githubusercontent.com/116161693/203493367-0173e496-fc7f-452a-b39f-3c8b8c23b8bd.png) 

Next, click on "Edit Inbound Rules", as highlighted in the image below:
 
![image](https://user-images.githubusercontent.com/116161693/203493397-047ae933-10f4-4c99-8958-1c32e6095c31.png)
![image](https://user-images.githubusercontent.com/116161693/203493451-b0b3d0d3-6964-4835-a09d-49d3890a8eaa.png) 

First, let us try to check how we can access it locally in our Ubuntu shell, run:

```
curl [http://localhost:80](http://localhost/)
```
or

```
curl [http://127.0.0.1:80](http://127.0.0.1/) 
```
As an output you can see some strangely formatted test, do not worry, we just made sure that our Apache web service responds to ‘curl’ command with some payload.
Now it is time for us to test how our Apache HTTP server can respond to requests from the Internet. Open a web browser of your choice and try to access following url
http://<Public-ip-address>:80	

Another way to retrieve your Public IP address, other than to check it in AWS Web Console, is to use the following command.

```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
**Congratulations!** Our web server is correctly installed and accessible through our firewall. 
![image](https://user-images.githubusercontent.com/116161693/203493534-dbeffddf-8f23-4ee5-954a-cee8bd8811a2.png)

## STEP 2 - Installing MySQL on the Virtual environment
In this step, I install a Database Management System (DBMS) to be able to store and manage data for the site in a relational database.
Run ‘apt’ to acquire and install this software, run: 

```
sudo apt install mysql-server**
```
Confirm installation by typing Y when prompted.
![image](https://user-images.githubusercontent.com/116161693/203494349-a21e516e-dc33-49e1-a324-fa0cefd2daba.png)

Once installation is complete, log in to the MySQL console by running: 
```
sudo mysql
```

![image](https://user-images.githubusercontent.com/116161693/203494378-cb4e46f7-6ff5-495f-9fec-4a78d29173e9.png)
 
Next, run a security script that comes pre-installed with MySQL, to remove some insecure default settings and lock down access to your database system. run: 

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1'
```
then exit MySQL shell by typing exit and enter.
![image](https://user-images.githubusercontent.com/116161693/203494516-68dc24cf-1351-4a04-9981-a1c91e528f06.png)

```
sudo mysql_secure_installation**
```
![image](https://user-images.githubusercontent.com/116161693/203494566-af9e83e8-0efa-4067-a035-c65c6cfb6ea1.png)

This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.
Note: Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.
Answer Y for yes, or anything else to continue without enabling.
VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No:
•	If you answer “yes”, you’ll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words.
If we enable password validation, we’ll be shown the password strength for the root password we just entered and our server will ask if we want to continue with that password. If we are happy with our current password, enter Y for “yes” at the prompt:
For the rest of the questions, We press Y and hit the ENTER key at each prompt. This will remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes we have made.
Next, test that login to MySQL console works. Run: 

```
sudo mysql -p
```
![image](https://user-images.githubusercontent.com/116161693/203494818-cf35dc14-68c7-44d3-9725-b1c9e3b67653.png)

when you are done, you will get this output:
To exit the MySQL Console, type: exit and hit the enter button
mysql> exit

Next, we will install PHP, the last component in the LAMP Stack.
## Step 3 — Installing PHP on the virtual environment

We have Apache installed to serve our content and MySQL installed to store and manage our data. PHP is the component of our setup that will process code to display dynamic content to the final user. In addition to the PHP package, we’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. We’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.
To install these 3 packages at once, run: 

```
sudo apt install php libapache2-mod-php php-mysql
```
![image](https://user-images.githubusercontent.com/116161693/203498458-908c2a5b-257a-4725-a9e5-866193e274fa.png) 
Once the installation is finished, you can run the below command to confirm your PHP version:

```
php -v
```
![image](https://user-images.githubusercontent.com/116161693/203498519-8fd565a7-5c51-44ff-8ab2-eff35f9a759b.png) 
At this point, your LAMP stack is completely installed and fully operational.
To test our setup with a PHP Script, it's best to setup a proper Apache Virtual Host to host our website's file and folders. Virtual Host allows you to have multiple websites located on a single machine and users of the websites will not even notice it.
 
## Step 4 — Creating a Virtual Host for your Website using Apache
In this project, you will set up a domain called projectlamp, but you can replace this with any domain of your choice.
•	Setting up a domain called projectlamp. Create the directory for projectlamp using ‘mkdir’. Run: 

```
sudo mkdir /var/www/projectlamp
```
![image](https://user-images.githubusercontent.com/116161693/203500062-211c1b32-28e6-47d2-bb52-7bd638ca150b.png)
•	assign ownership of the directory with your current system user, run: 

```
sudo chown -R $USER:$USER /var/www/projectlamp
```
![image](https://user-images.githubusercontent.com/116161693/203500482-0f598ab7-d113-4d9f-a5d0-e7197910631b.png)

•	Next, create and open a new configuration file in Apache’s sites-available directory. run: 

```
sudo vi /etc/apache2/sites-available/projectlamp.conf
```
•	This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text: 

```
<VirtualHost :80> ServerName projectlamp ServerAlias [www.projectlamp](http://www.projectlamp/) ServerAdmin webmaster@localhost DocumentRoot /var/www/projectlamp ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined
```
![image](https://user-images.githubusercontent.com/116161693/203500627-515a47f7-1451-4707-b22e-b05cd1d4a505.png) 
To save and close the file, simply follow the steps below:
•	Hit the esc button on the keyboard
•	Type :
•	Type wq. w for write and q for quit
•	Hit ENTER to save the file 
![image](https://user-images.githubusercontent.com/116161693/203501228-1120b2bc-07ee-4550-b470-38bf26985f92.png)
You can use the ls command to show the new file in the sites-available directory

```
sudo ls /etc/apache2/sites-available
```
•	this is my output  
![image](https://user-images.githubusercontent.com/116161693/203501257-ce5b39c1-3b97-4438-a447-8c86989ee9bd.png)

You can now use a2ensite command to enable the new virtual host:

```
sudo a2ensite projectlamp
```
![image](https://user-images.githubusercontent.com/116161693/203501673-2635cf70-e377-42f8-a7c3-8038bc1e5d05.png)

•	We might want to disable the default website that comes installed with Apache. This is required if we are not using a custom domain name, because in this case Apache’s default configuration would overwrite our virtual host. To disable Apache’sdefault website use a2dissite command, type:

```
sudo a2dissite 000-default
```
![image](https://user-images.githubusercontent.com/116161693/203501707-c1bff5a6-68f4-4d7d-8b7d-da8e68434608.png)

•	To make sure your configuration file doesn't contain syntax errors, run :

```
sudo apache2ctl configtest
```

![image](https://user-images.githubusercontent.com/116161693/203501745-e8838540-76c2-49f3-9b35-380003fe6052.png)
•	Finally, reload Apache so these changes take effect:

```
sudo systemctl reload apache2
```
Your new website is now active, but the web root /var/www/projectlamp is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:
•	Type and run :
```
sudo vi /var/www/projectlamp/index.html
```
see my input : 
![image](https://user-images.githubusercontent.com/116161693/203502083-e2b8a015-dae7-499e-8699-0688ac50377a.png)

Now go to your browser and try to open your website URL using IP address:
http://<public-ip-address>:80
or you can also browse using your public DNS. the result is the same
http://<Public-DNS-Name>:80
And this is my output : 

![image](https://user-images.githubusercontent.com/116161693/205486284-a73093a2-4873-4013-b1b6-c20eec9e716c.png)

To check your Public IP from the Ubuntu shell, run :
```
(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 
```
```
(curl -s http://169.254.169.254/latest/meta-data/public-ipv4)
```
## Step 5 — Enable PHP on the website
With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. To make index.php file tak precedence need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive.

```
sudo vim /etc/apache2/mods-enabled/dir.conf
```

![image](https://user-images.githubusercontent.com/116161693/203516928-89a46f3b-6b8f-40f7-8c5c-1a7cd7af9bee.png) 
Type esc key :%d and hit enter button to clear the previous html and paste the below

```
sudo vim /etc/apache2/mods-enabled/dir.conf then: #Change this: #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm #To this: DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
```

 After saving and closing the file, you will need to reload Apache so the changes can  take effect:

```
sudo systemctl reload apache2
```

Finally, we will create a PHP Script to test the PHP is correctly installed and configured on our Server.
Now that we have a custom location to host our website's files and folders, we'll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.
Create a new file named index.php inside our custom web root folder:
```
vim /var/www/projectlamp/index.php
```
This will open a blank file. Add the following text, which is valid PHP code, inside the file:

```
<?php
phpinfo();
```
![image](https://user-images.githubusercontent.com/116161693/203517145-0321dd56-1a96-46a9-9cf8-0db1a56b07bf.png) 

When you have finished, save and close the file, refresh the page and you will see a page similar to this: 

![image](https://user-images.githubusercontent.com/116161693/203504935-10a4a696-908e-4292-ae04-bf758606575c.png)

### CONGRATULATIONS! You did it YAY………..!
This page provides information about your server from the perspective of PHP. It is useful for debugging and to ensure that your settings are being applied correctly. If you can see this page in your browser, then your PHP installation is working as expected.
After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so by running:

```
sudo rm /var/www/projectlamp/index.php
```