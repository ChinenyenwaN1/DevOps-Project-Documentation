# Web-Solution-With-Wordpress

WordPress (WP, WordPress.org) is a free and open-source content management system (CMS) written in PHP[4] and paired with a MySQL or MariaDB database. Features include a plugin architecture and a template system, referred to within WordPress as Themes. WordPress was originally created as a blog-publishing system but has evolved to support other web content types including more traditional mailing lists and forums, media galleries, membership sites, learning management systems (LMS) and online stores. 

To function, WordPress has to be installed on a web server, either part of an Internet hosting service like WordPress.com or a computer running the software package WordPress.org in order to serve as a network host in its own right.


## Three-tier Architecture

Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.

![image](https://user-images.githubusercontent.com/116161693/210240670-54e9ea65-3afe-42eb-9e34-3fb1cf232bab.png)

3-Tier Setup

* A Laptop or PC to serve as a client
* An EC2 Linux Server as a web server (This is where you will install WordPress)
* An EC2 Linux server as a database (DB) server

**Three-tier Architecture** allows any one of the three tiers to be upgraded or replaced independently.

The user interface is implemented on any platform such as a desktop PC, smartphone or tablet as a native application, web app, mobile app, voice interface, etc. It uses a standard graphical user interface with different modules running on the application server.

The relational database management system on the database server contains the computer data storage logic.

The middle tiers are usually multitiered.

Since the three are not physical but logical in nature, they may run in different servers both in on-premises based solutions, as well as in software-as-a-service (SaaS).


**Benefit of a Three-Tier Architecture**

* it provides great freedom to development teams who can independently update or replace only specific parts of the application without affecting the product as a whole.

* The application can be scaled up and out rather easily by detaching the front-end application from the databases that are selected according to the individual needs of the customer.

* New hardware, such as new servers, can also be added at a later time to deal with massive amounts of data or particularly demanding services.

* A three-tier-architecture also provides a higher degree of flexibility to enterprises who may want to adopt a new technology as soon as it becomes available.

* Critical components of the application can be encapsulated and retained while the whole system keeps evolving organically.

* Development cycle or upgrade times are significantly improved ensuring minimal disruption in customer’s experience.

* Different teams can work on different sections of the application rather than on the full stack according to their areas of expertise, improving their efficiency and speed.


The Three Tiers in a Three-Tier Architecture

**Presentation Tier**

* Occupies the top level and displays information related to services commonly available on a web browser or web-based application in the form of a graphical user interface (GUI).

* It constitutes the front-end layer of the application and the interface with which end-users will interact directly.

* This tier is usually built on web development frameworks, such as CSS or JavaScript, and communicates with other tiers by sending results to the browser and other tiers in the network through API calls.

**Application Tier**

* This tier — also called the middle tier, logic tier, business logic or logic tier — is pulled from the presentation tier.

* It controls the application’s core functionality by performing detailed processing and is usually coded in programming languages, such as Python, Java, C++, .NET, etc.

**Data Tier**
* Houses database servers where information is stored and retrieved.

* Data in this tier is kept independent of application servers or business logic, and is managed and accessed with programs, such as MongoDB, Oracle, MySQL, and Microsoft SQL Server.


In this project, We will have the hands-on experience that showcases Three-tier Architecture while also ensuring that the disks used to store files on the Linux servers are adequately partitioned and managed through programs such as gdisk and LVM respectively. The following steps will be followed to achieve our aim and objectives of this project.

LAUNCH AN EC2 INSTANCE THAT WILL SERVE AS “WEB SERVER”.

**Step 1** — Prepare the Web Server

* Launch an EC2 instance that will serve as "Web Server". Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

* Create and Attach all three volumes one by one to your Web Server EC2 instance

* Open MobaXterm and connect Ec2 instances using public IP Address and begin configuration


```
lsblk
```
![image](https://user-images.githubusercontent.com/116161693/210232022-47938cf3-a743-4c71-8f2f-abad228ed42c.png)


```
sudo df -h 
```
![image](https://user-images.githubusercontent.com/116161693/210232038-c6da524f-6b92-4764-823a-7fe5e272cef0.png)


```
sudo gdisk /dev/xvdf
```
![image](https://user-images.githubusercontent.com/116161693/210232063-5373fc6e-c567-4283-851e-89a24202827d.png)


```
sudo gdisk /dev/xvdg
```
![image](https://user-images.githubusercontent.com/116161693/210232080-be2fddcb-5b4d-4e8e-88dd-c602ee92dbfd.png)


```
sudo gdisk /dev/xvdh
```

![image](https://user-images.githubusercontent.com/116161693/210232098-58e81c54-2421-4b32-abcc-512816e06163.png)


* We will use the command below to view the newly configured partition on each of the 3 disks


```
lsblk
```

![image](https://user-images.githubusercontent.com/116161693/210232123-7283129d-dce9-4f02-998b-db8bfde6175d.png)


* Install lvm2 package using sudo yum install lvm2. Run sudo lvmdiskscan command to check for available partitions.

```
sudo yum install lvm2
sudo lvmdiskscan
```

![image](https://user-images.githubusercontent.com/116161693/210232286-a0bb81bf-9204-4c53-a8a3-d7e233e8e18a.png)

![image](https://user-images.githubusercontent.com/116161693/210232309-8c141bfc-98ae-4cc8-90fc-9d87c7493c61.png)


* Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```

![image](https://user-images.githubusercontent.com/116161693/210232348-38bfdd76-1171-4e29-9158-b23d58d2cab5.png)

```
sudo pvs
```

![image](https://user-images.githubusercontent.com/116161693/210232359-bd925811-0683-4202-a5db-87827d35ec5b.png)

* Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg

```
sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
```
![image](https://user-images.githubusercontent.com/116161693/210232385-003f1c3d-89f2-4bcc-aac6-b0108f80f4e4.png)


* Verify that your VG has been created successfully by running

```
sudo vgs
```

![image](https://user-images.githubusercontent.com/116161693/210232401-f9a9da1b-304b-4fe8-9c39-5d26cc33dd12.png)

* **Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.**


```
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
```

![image](https://user-images.githubusercontent.com/116161693/210232424-193bdee8-7243-4fc1-8b44-e8cd697263a6.png)


* Verify that your Logical Volume has been created successfully


* Verify the entire setup

```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk 
```

![image](https://user-images.githubusercontent.com/116161693/210232847-04f98267-be53-4773-8a7e-ca109aa98878.png)


![image](https://user-images.githubusercontent.com/116161693/210232879-83398c7d-fada-4cbe-baf0-d369b53be1be.png)


* Use mkfs.ext4 to format the logical volumes with ext4 filesystem**

```
sudo mkfs.ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```

![image](https://user-images.githubusercontent.com/116161693/210232912-93ace67c-9728-46cd-805b-9136e2eda41d.png)


* Create /var/www/html directory to store website files

```
sudo mkdir -p /var/www/html
```

* Create /home/recovery/logs to store backup of log data

```
sudo mkdir -p /home/recovery/logs
```

* Mount /var/www/html on apps-lv logical volume


```
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```

* Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)

```
sudo rsync -av /var/log/. /home/recovery/logs/
```

* Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very important)

```
sudo mount /dev/webdata-vg/logs-lv /var/log
```

* Restore log files back into /var/log directory

```
sudo rsync -av /home/recovery/logs/. /var/log
```

![image](https://user-images.githubusercontent.com/116161693/210233303-8c990363-ab63-47c4-ad66-f9c9f66b3ee9.png)

### Update /etc/fstab file so that the mount configuration will persist after restart of the server.


* The UUID of the device will be used to update the /etc/fstab file;

```
sudo blkid
```
![image](https://user-images.githubusercontent.com/116161693/210233330-5a27f32e-753f-4090-b278-1260d22f471d.png)

```
sudo vi /etc/fstab
```

* Update /etc/fstab in this format using your own UUID and rememeber to remove the leading and ending quotes.

![image](https://user-images.githubusercontent.com/116161693/210233364-faf9800d-6a98-4ea4-b422-5db56256775b.png)


* Test the configuration and reload the daemon

```
sudo mount -a
sudo systemctl daemon-reload
```

* Verify your setup by running df -h, output must look like this:


```
df -h 
```


* Step 2 — Prepare the Database Server

1* **Launch a second RedHat EC2 instance that will have a role – ‘DB Server’**

2* **Create and Attach all three volumes one by one to your DB Server EC2 instance**

3* **Open MobaXterm and connect Ec2 instances using public IP Address and begin configuration**


![image](https://user-images.githubusercontent.com/116161693/210234185-914c46e7-3565-47c0-ba3a-9b5cb772ed32.png)

```
df -h
```

![image](https://user-images.githubusercontent.com/116161693/210234197-020f6a47-98cd-413c-81ad-ea325f6c85f8.png)


* We need to create a single partition on each of the 3 disks we added

```
sudo gdisk /dev/xvdf
sudo gdisk /dev/xvdg
sudo gdisk /dev/xvdh
```
![image](https://user-images.githubusercontent.com/116161693/210234221-34c746c5-d253-44cc-947f-b3456b3df76d.png)

![image](https://user-images.githubusercontent.com/116161693/210234246-1f503349-1921-4874-8046-ac2f5436e999.png)

![image](https://user-images.githubusercontent.com/116161693/210234257-93324668-b0cd-4ef5-9c58-2ac07760b3af.png)


* We will use the command below to view the newly configured partition on each of the 3 disks


```
sudo lsblk
```

![image](https://user-images.githubusercontent.com/116161693/210234291-237b5d95-0764-4a0b-b61a-46906c20e098.png)


* We use this command to check for available storage for Logical Volume Manager (LVM)

![image](https://user-images.githubusercontent.com/116161693/210234345-0e895be5-8f8b-4662-98bf-bf18160a3974.png)


```
sudo lvmdiskscan
```
![image](https://user-images.githubusercontent.com/116161693/210234367-a7d8256f-4d2f-46fb-98c7-35e5311ae300.png)



* Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```

![image](https://user-images.githubusercontent.com/116161693/210234761-54be8ff7-8946-429a-91d2-0191972eebcf.png)


* Verify that your Physical volume has been created successfully by running


```
sudo pvs
```
![image](https://user-images.githubusercontent.com/116161693/210234776-6f4f07fe-5c61-4bee-b04c-fe26357d29e1.png)


* Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG database-vg

```
sudo vgcreate vg-database /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
```
![image](https://user-images.githubusercontent.com/116161693/210234887-3bd7b90a-9bcf-4f29-868b-d210ab28a333.png)

* Verify that your VG has been created successfully by running

```
sudo vgs
```

![image](https://user-images.githubusercontent.com/116161693/210234903-e7fd2d83-dea2-440d-85e3-ae69759ce11c.png)


* Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.**


```
sudo lvcreate -n db-lv -L 14G vg-database
sudo lvcreate -n logs-lv -L 14G vg-database
```
![image](https://user-images.githubusercontent.com/116161693/210234923-98f017c6-a717-4c95-9ff1-eafbc0dfe98d.png)



* Verify that your Logical Volume has been created successfully by running


```
sudo lvs
```
![image](https://user-images.githubusercontent.com/116161693/210234999-41669a8a-b8cb-49ae-ae68-608776419975.png)


* Verify the entire setup


```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk 
```
![image](https://user-images.githubusercontent.com/116161693/210235033-45eeb4e2-b4ad-40fe-b17a-80f7874a7d23.png)


![image](https://user-images.githubusercontent.com/116161693/210235054-cb0b22a7-1264-4111-ae48-b742581649c9.png)

* Use mkfs.ext4 to format the logical volumes with ext4 filesystem

```
sudo mkfs.ext4 /dev/vg-database/db-lv && sudo mkfs.ext4 /dev/vg-database/logs-lv
```

![image](https://user-images.githubusercontent.com/116161693/210235072-68cefc1b-541d-44e4-b651-e87c4f36ea26.png)


* We need to create /db directory to store our database files
 
```
sudo mkdir /db
```

* **We need to create /home/recovery/logs to store backup of log data**

```
$ sudo mkdir -p /home/recovery/logs
```

* **We need to Mount /db on db-lv logical volume**

```
sudo mount /dev/vg-database/db-lv /db
sudo df -h
```
![image](https://user-images.githubusercontent.com/116161693/210235231-552642ff-05c7-4397-8929-bf8bd59aec0d.png)


* **We need to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)**


```
$ sudo rsync -r /var/log/ /home/recovery/logs/
```
![image](https://user-images.githubusercontent.com/116161693/210235295-98e72cda-8ab2-4681-a094-8eb161bb6723.png)


* **We need to Mount /var/log on logs-lv logical volume**

```
sudo mount /dev/vg-database/logs-lv /var/log
sudo rsync -av /home/recovery/logs/. /var/log
sudo blkid
```
![image](https://user-images.githubusercontent.com/116161693/210236325-cb021759-18a7-4588-9257-aeafa2edcce3.png)

Update /etc/fstab in this format using your own UUID and rememeber to remove the leading and ending quotes.

```
sudo vi /etc/fstab
```

UUID=ea114160-b65a-4249-9649-29bf3625d42b /db ext4 defaults 0 0

UUID=64e8e59e-20a9-4697-b912-1b716b7cbaa8 /var/log ext4 defaults 0 0

![image](https://user-images.githubusercontent.com/116161693/210236474-a9fb4fee-ee5f-4389-b03a-cc0bd95a9f3f.png)

Test the configuration and reload the daemon

![image](https://user-images.githubusercontent.com/116161693/210236500-8901f100-2115-4d88-a319-40631256e2be.png)

* **We need to restore log files back into /var/log directory.**


* **We need to update the /etc/fstab file so that the mount configuration will persist upon restart of the server.**



## Step 3 — Install WordPress on your Web Server EC2


1* **Update the repository**


```
sudo yum -y update
```

2* **Install wget, Apache and it’s dependencies**


```
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```
![image](https://user-images.githubusercontent.com/116161693/210236564-dc8f9802-cab3-4fca-b34a-a40e5a746bfe.png)

3* **Start Apache**


```
sudo systemctl enable httpd
sudo systemctl start httpd
sudo systemctl status httpd
```

![image](https://user-images.githubusercontent.com/116161693/210236982-4f417951-83c5-4ae8-83b2-389c74b8f9f9.png)


4* **To install PHP and it’s depemdencies**


```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
setsebool -P httpd_execmem 1
```

![image](https://user-images.githubusercontent.com/116161693/210237003-706abf56-f0d7-4a4e-9201-ec9f09eda9c3.png)


5* **Restart Apache**


```
sudo systemctl restart httpd
```

6* **Download wordpress and copy wordpress to var/www/html**


```
mkdir wordpress
cd   wordpress
sudo wget http://wordpress.org/latest.tar.gz
sudo tar xzvf latest.tar.gz
sudo rm -rf latest.tar.gz
sudo cp wordpress/wp-config-sample.php wordpress/wp-config.php
sudo cp -R wordpress /var/www/html/
```
  
![image](https://user-images.githubusercontent.com/116161693/210237066-3e71f04f-8196-4658-9e72-046f6ca5a345.png)

7* **Configure SELinux Policies**


```
sudo chown -R apache:apache /var/www/html/wordpress
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
sudo setsebool -P httpd_can_network_connect=1
```


## Step 4 — Install MySQL on your DB Server EC2


```
sudo yum update
sudo yum install mysql_server_installation
```
![image](https://user-images.githubusercontent.com/116161693/210237158-814e3521-7280-4f01-a541-0270bf58c619.png)

![image](https://user-images.githubusercontent.com/116161693/210237168-4abf36f3-084d-4b84-8f6c-434383138ce5.png)

**Verify that the service is up and running**


```
sudo systemctl restart mysqld
sudo systemctl enable mysqld
```

![image](https://user-images.githubusercontent.com/116161693/210237207-af7f5f06-7867-4410-b6f5-9631189b58c6.png)


## Step 5 — Configure DB to work with WordPressConfiguring the DB to work with WordPress
Here we need to configure the database to work with WordPress. By allowing the wordpress server be able to connect to the database, we need to configure the database to allow the wordpress server to connect to the database.

Get the ip-address of the wordpress server

```
curl http://checkip.amazonaws.com
```
![image](https://user-images.githubusercontent.com/116161693/210237376-a36fe265-0bbf-4254-9ee8-e699b04b6808.png)


```
sudo mysql
CREATE DATABASE wordpress;
CREATE USER `myuser`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'wordpres';
GRANT ALL ON wordpres.* TO 'myuser'@'<Web-Server-Private-IP-Address>';
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```

![image](https://user-images.githubusercontent.com/116161693/210237462-ab2b0e16-1559-4408-88bd-d959df3328e5.png)

## Step 6 — Configure WordPress to connect to remote database.

* Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, you shall allow access to the DB server ONLY from your Web Server’s IP address, so in the Inbound Rule configuration specify source as /32

![image](https://user-images.githubusercontent.com/116161693/210240161-85367fd0-92c8-4905-80d4-2da62a552064.png)

1* **Install MySQL client and test that you can connect from your Web Server to your DB server by using mysql-client**

```
sudo yum install mysql
sudo mysql -h <DB-Server-Private-IP-address> -u wordpres -p
```
![image](https://user-images.githubusercontent.com/116161693/210237862-cb0f4ad9-7491-41fa-a32c-eaf004963a4d.png)

2* **Verify if you can successfully execute SHOW DATABASES; command and see a list of existing databases.**

![image](https://user-images.githubusercontent.com/116161693/210237822-16b38122-40e5-47cd-86aa-e49099856c6c.png)

3* **Change permissions and configuration so Apache could use WordPress:
Here we need to create a configuration file for wordpress in order to point client requests to the wordpress directory.

```
sudo vi /etc/httpd/conf.d/wordpress.conf
```

<VirtualHost *:80>
ServerAdmin myuser@3.83.55.100
DocumentRoot /var/www/html/wordpress

<Directory "/var/www/html/wordpress">
Options Indexes FollowSymLinks
AllowOverride all
Require all granted
</Directory>

ErrorLog /var/log/httpd/wordpress_error.log
CustomLog /var/log/httpd/wordpress_access.log common
</VirtualHost>

![image](https://user-images.githubusercontent.com/116161693/210238122-f376bfbe-5d9f-479e-a16b-f37ca0d9238a.png)

To apply the changes, restart Apache

```
sudo systemctl restart httpd
```
Edit the wp-config file
```
sudo vi /var/www/html/wordpress/wp-config.php
```
![image](https://user-images.githubusercontent.com/116161693/210238327-2ea065ba-bab2-4a71-aeb2-40cfe783c159.png)

configure SELinux for wordpress

```
sudo semanage fcontext -a -t httpd_sys_rw_content_t "/var/www/html/wordpress/.*?"
```
Note: The semanage command is not available on CentOS 7.x.x. and you might need to install it using the following command:
```
sudo yum provides /usr/sbin/semanage
sudo yum install policycoreutils-python-utils
```

4* **Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2 (enable from everywhere 0.0.0.0/0 or from your workstation’s IP)**

5* **Try to access from your browser the link to your WordPress http://<Web-Server-Public-IP-Address>/wordpress/
  ![image](https://user-images.githubusercontent.com/116161693/210238526-4f2dfcf2-4b8a-4f62-8f67-45bd78e7f29d.png)

  ![image](https://user-images.githubusercontent.com/116161693/210238556-dc75dba2-8d2c-4a29-bafe-fa87024f6193.png)

 
