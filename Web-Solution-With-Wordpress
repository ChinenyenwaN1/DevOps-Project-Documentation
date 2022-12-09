# Web-Solution-With-Wordpress

**WordPress (WP, WordPress.org) is a free and open-source content management system (CMS) written in PHP[4] and paired with a MySQL or MariaDB database. Features include a plugin architecture and a template system, referred to within WordPress as Themes. WordPress was originally created as a blog-publishing system but has evolved to support other web content types including more traditional mailing lists and forums, media galleries, membership sites, learning management systems (LMS) and online stores.** 


## For WordPress to function effectively it has to be installed on a web server, either part of an Internet hosting service like WordPress.com or a computer running the software package WordPress.org in order to serve as a network host in its own right.


## Three-tier Architecture

### Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.


![image](https://user-images.githubusercontent.com/85270361/134702570-9fe3ec31-4ba7-4545-8f47-1104eda0a93d.png)


## 3-Tier Setup

* A Laptop or PC to serve as a client
* An EC2 Linux Server as a web server (This is where you will install WordPress)
* An EC2 Linux server as a database (DB) server

**Three-tier Architecture** allows any one of the three tiers to be upgraded or replaced independently.

The user interface is implemented on any platform such as a desktop PC, smartphone or tablet as a native application, web app, mobile app, voice interface, etc. It uses a standard graphical user interface with different modules running on the application server.

The relational database management system on the database server contains the computer data storage logic.

The middle tiers are usually multitiered.

Since the 3 are not physical but logical in nature, they may run in different servers both in on-premises based solutions, as well as in software-as-a-service (SaaS).


## what are the benefits of a three tier Architecture 

* it provides great freedom to development teams who can independently update or replace only specific parts of the application without affecting the product as a whole.

* The application can be scaled up and out rather easily by detaching the front-end application from the databases that are selected according to the individual needs of the customer.

* New hardware, such as new servers, can also be added at a later time to deal with massive amounts of data or particularly demanding services.

* A three-tier-architecture also provides a higher degree of flexibility to enterprises who may want to adopt a new technology as soon as it becomes available.

* Critical components of the application can be encapsulated and retained while the whole system keeps evolving organically.

* Development cycle or upgrade times are significantly improved ensuring minimal disruption in customer’s experience.

* Different teams can work on different sections of the application rather than on the full stack according to their areas of expertise, improving their efficiency and speed.


The Three Tiers in a Three-Tier Architecture

### Presentation Tier

* Occupies the top level and displays information related to services commonly available on a web browser or web-based application in the form of a graphical user interface (GUI).

* It constitutes the front-end layer of the application and the interface with which end-users will interact directly.

* This tier is usually built on web development frameworks, such as CSS or JavaScript, and communicates with other tiers by sending results to the browser and other tiers in the network through API calls.

### Application Tier

* This tier — also called the middle tier, logic tier, business logic or logic tier — is pulled from the presentation tier.

* It controls the application’s core functionality by performing detailed processing and is usually coded in programming languages, such as Python, Java, C++, .NET, etc.

### Data Tier

* Houses database servers where information is stored and retrieved.

* Data in this tier is kept independent of application servers or business logic, and is managed and accessed with programs, such as MongoDB, Oracle, MySQL, and Microsoft SQL Server.


# In this project, We will have the hands-on experience that showcases Three-tier Architecture while also ensuring that the disks used to store files on the Linux servers are adequately partitioned and managed through programs such as gdisk and LVM respectively. The following steps will be followed to achieve our aim and objectives of this project.



# LAUNCH AN EC2 INSTANCE THAT WILL SERVE AS “WEB SERVER”.

## Step 1 — Prepare the Web Server

1* **Launch an EC2 instance that will serve as "Web Server". Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.**

2* **Create and Attach all three volumes one by one to your Web Server EC2 instance**

3* **Open MobaXterm and connect Ec2 instances using public IP Address and begin configuration**


```
# lsblk
```

<img width="412" alt="lsblk" src="https://user-images.githubusercontent.com/85270361/135422111-510c5815-5504-419e-bb6a-268f76c30e46.PNG">


```
df -h 
```

<img width="452" alt="df-h" src="https://user-images.githubusercontent.com/85270361/135422419-1869062e-5e72-4554-93dd-d35a7b92c0ef.PNG">


```
sudo gdisk /dev/xvdf
```

<img width="432" alt="gdisk xvdf" src="https://user-images.githubusercontent.com/85270361/135423388-36260d77-4203-4041-9d08-b31b5e086447.PNG">


```
sudo gdisk /dev/xvdg
```

<img width="421" alt="gdisk xdvg" src="https://user-images.githubusercontent.com/85270361/135423595-197621a4-0583-4be8-bc94-49622daf6f1b.PNG">


```
sudo gdisk /dev/xvdh
```

<img width="478" alt="gdisk xvdh" src="https://user-images.githubusercontent.com/85270361/135423705-3ee99907-b81e-472e-8bf5-d40ffc734f03.PNG">


* **We will use the command below to view the newly configured partition on each of the 3 disks**


```
# lsblk
```

![image](https://user-images.githubusercontent.com/85270361/135425063-3534a598-cd1c-4c53-81d5-220dbb36ab0a.png)


* **We use this command to check for available storage for Logical Volume Manager (LVM)**

```
# sudo lvmdiskscan
```


<img width="356" alt="ctscan" src="https://user-images.githubusercontent.com/85270361/135428219-a3c55d96-89fa-4278-97f5-8ab814123803.PNG">


* **Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM**

```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```

```
sudo pvs
```


<img width="374" alt="pvs" src="https://user-images.githubusercontent.com/85270361/135429265-0e730aba-a727-4345-9e49-dbe904fa193e.PNG">


* **Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg**


```
sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
```

<img width="484" alt="vg webdata" src="https://user-images.githubusercontent.com/85270361/135429848-7188ac0c-8956-4ff5-b535-344893c70228.PNG">


* **Verify that your VG has been created successfully by running**

```
sudo vgs
```

<img width="331" alt="sudo vgs" src="https://user-images.githubusercontent.com/85270361/135436025-0466ce9d-d3fc-48b3-a13c-bd08baf5a081.PNG">


* **Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.**


```
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
```

<img width="505" alt="lvs lvs" src="https://user-images.githubusercontent.com/85270361/135436287-9aff4a1d-459b-42a2-808e-b93e68c92438.PNG">


* **Verify that your Logical Volume has been created successfully**


<img width="558" alt="sudo lvs" src="https://user-images.githubusercontent.com/85270361/135436869-6e760728-8803-403a-9379-354cc26f1bf1.PNG">


* **Verify the entire setup**

```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk 
```

<img width="418" alt="isblk 2" src="https://user-images.githubusercontent.com/85270361/135437094-a2a324c3-8a54-43b1-9afb-e70388887ef9.PNG">


* **Use mkfs.ext4 to format the logical volumes with ext4 filesystem**

```
sudo mkfs -t ext4 /dev/webdata-vg/app-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```

<img width="527" alt="mkfs" src="https://user-images.githubusercontent.com/85270361/135437641-e4579762-69ec-4732-bea6-7869ac6b9d37.PNG">


### Create /var/www/html directory to store website files

```
sudo mkdir -p /var/www/html
```

### Create /home/recovery/logs to store backup of log data

```
sudo mkdir -p /home/recovery/logs
```

### Mount /var/www/html on apps-lv logical volume


```
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```

### Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)

```
sudo rsync -av /var/log/. /home/recovery/logs/
```


### Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very important)

```
sudo mount /dev/webdata-vg/logs-lv /var/log
```

### Restore log files back into /var/log directory

```
sudo rsync -av /home/recovery/logs/. /var/log
```


### Update /etc/fstab file so that the mount configuration will persist after restart of the server.


* **The UUID of the device will be used to update the /etc/fstab file;**

```
sudo blkid
```

<img width="570" alt="sudo blik" src="https://user-images.githubusercontent.com/85270361/135439541-9b7f587d-56b4-49ef-9858-b323cecbdb03.PNG">

```
sudo vi /etc/fstab
```

### Update /etc/fstab in this format using your own UUID and rememeber to remove the leading and ending quotes.

<img width="484" alt="fstab" src="https://user-images.githubusercontent.com/85270361/135439997-c33bee94-0765-43fc-ba5b-79e5e9f56af9.PNG">


### Test the configuration and reload the daemon

```
sudo mount -a
sudo systemctl daemon-reload
```

### Verify your setup by running df -h, output must look like this:


```
df -h 
```

<img width="603" alt="dh f2" src="https://user-images.githubusercontent.com/85270361/135442051-ab931d5f-28e1-4df4-9e06-c126811e98c8.PNG">


## Step 2 — Prepare the Database Server

1* **Launch a second RedHat EC2 instance that will have a role – ‘DB Server’**

2* **Create and Attach all three volumes one by one to your DB Server EC2 instance**

3* **Open MobaXterm and connect Ec2 instances using public IP Address and begin configuration**


```
$ df -h
```


<img width="522" alt="df h db" src="https://user-images.githubusercontent.com/85270361/135603957-0c5f8582-6a1c-4c50-bd67-5e4add4f5603.PNG">



* **We need to create a single partition on each of the 3 disks we added**

```
sudo gdisk /dev/xvdf
sudo gdisk /dev/xvdg
sudo gdisk /dev/xvdh
```


<img width="617" alt="first for database" src="https://user-images.githubusercontent.com/85270361/135596418-5eb0ddc5-878d-4758-80ed-4558a1851512.PNG">

<img width="754" alt="second for database" src="https://user-images.githubusercontent.com/85270361/135596464-8f316205-5761-477c-815b-69081e75ec6f.PNG">

<img width="748" alt="third for database" src="https://user-images.githubusercontent.com/85270361/135596627-84557816-62b5-4c2c-9a20-10781fd06799.PNG">



* **We will use the command below to view the newly configured partition on each of the 3 disks**


```
$ sudo lsblk
```



<img width="420" alt="sudo lblk" src="https://user-images.githubusercontent.com/85270361/135604005-2e91b51f-c233-43fc-b80d-9723fe84a18c.PNG">



* **We use this command to check for available storage for Logical Volume Manager (LVM)**


```
$ sudo lvmdiskscan
```


<img width="379" alt="livscan" src="https://user-images.githubusercontent.com/85270361/135591419-ff220a9f-994c-465f-ba66-ec302ecf10ad.PNG">


* **Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM**


```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```


* **Verify that your Physical volume has been created successfully by running**


```
sudo pvs
```

<img width="374" alt="pvs" src="https://user-images.githubusercontent.com/85270361/135592258-9367afc6-0537-45b2-a53c-d9dfcd2bc912.PNG">


* **Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG database-vg**


```
sudo vgcreate vg-database /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
```

<img width="471" alt="sudo vgcrearte db" src="https://user-images.githubusercontent.com/85270361/135602228-3238dc9f-2fe4-43be-ba9e-3445f45bb807.PNG">



* **Verify that your VG has been created successfully by running**


```
sudo vgs
```

<img width="360" alt="database vcg" src="https://user-images.githubusercontent.com/85270361/135598420-542ad595-ddb7-475f-854c-7550e7322794.PNG">



* **Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.**


```
sudo lvcreate -n apps-lv -L 14G database-vg
sudo lvcreate -n logs-lv -L 14G database-vg
```



* **Verify that your Logical Volume has been created successfully by running**


```
sudo lvs
```

<img width="561" alt="database lvm" src="https://user-images.githubusercontent.com/85270361/135598878-8e91912e-41d0-42ae-a784-79873e579c7a.PNG">



* **Verify the entire setup**


```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk 
```


<img width="517" alt="db lsblk" src="https://user-images.githubusercontent.com/85270361/135598831-568ac86f-1a45-4675-b177-5b4a863e7520.PNG">



* **Use mkfs.ext4 to format the logical volumes with ext4 filesystem**

```
sudo mkfs -t ext4 /dev/vg-database/db-lv
```


<img width="482" alt="mkfs db" src="https://user-images.githubusercontent.com/85270361/135601938-3db6cfb5-a995-4dca-b2cd-ba09f91db192.PNG">



* **We need to create /db directory to store our database files**

```
$ sudo mkdir /db
```


* **We need to create /home/recovery/logs to store backup of log data**


```
$ sudo mkdir -p /home/recovery/logs
```


* **We need to Mount /db on db-lv logical volume**


```
sudo mount /dev/vg-database/db-lv /db
df -h
```

<img width="466" alt="db vl" src="https://user-images.githubusercontent.com/85270361/135603121-44ac41ac-b9e0-4f04-ada2-e8b3a262f134.PNG">


* **We need to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)**


```
$ sudo rsync -r /var/log/ /home/recovery/logs/
```


* **We need to Mount /var/log on logs-lv logical volume**


```
$ sudo mount /dev/dbdata-vg/logs-lv /var/log
```


* **We need to restore log files back into /var/log directory.**


```
$ sudo cp -r /home/recovery/logs/.* /var/log
```


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


3* **Start Apache**


```
sudo systemctl enable httpd
sudo systemctl start httpd
sudo systemctl status httpd
```

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
  cp wordpress/wp-config-sample.php wordpress/wp-config.php
  cp -R wordpress /var/www/html/
  ```
  
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


<img width="479" alt="Capture 1" src="https://user-images.githubusercontent.com/85270361/135607628-ff550122-e678-4aa5-8253-ebe91f36b4ec.PNG">


* **Verify that the service is up and running**


```
sudo systemctl restart mysqld
sudo systemctl enable mysqld
```

## Step 5 — Configure DB to work with WordPress


```
sudo mysql
CREATE DATABASE wordpress;
CREATE USER `myuser`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'wordpres';
GRANT ALL ON wordpres.* TO 'myuser'@'<Web-Server-Private-IP-Address>';
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```


## Step 6 — Configure WordPress to connect to remote database.

* Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, you shall allow access to the DB server ONLY from your Web Server’s IP address, so in the Inbound Rule configuration specify source as /32


<img width="960" alt="database security group" src="https://user-images.githubusercontent.com/85270361/135609816-96e8aab4-e30a-459f-8f96-239250c96dd6.PNG">


1* **Install MySQL client and test that you can connect from your Web Server to your DB server by using mysql-client**


```
sudo yum install mysql
sudo mysql -h <DB-Server-Private-IP-address> -u wordpres -p
```

<img width="577" alt="connecting web to db mysql" src="https://user-images.githubusercontent.com/85270361/135610631-715cadfd-5232-4d45-8763-530b44be7016.PNG">



2* **Verify if you can successfully execute SHOW DATABASES; command and see a list of existing databases.**


<img width="576" alt="last show databases" src="https://user-images.githubusercontent.com/85270361/135610838-c288e92f-2dea-44a5-9253-b732ff08b1c4.PNG">


3* **Change permissions and configuration so Apache could use WordPress:**


4* **Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2 (enable from everywhere 0.0.0.0/0 or from your workstation’s IP)**


5* **Try to access from your browser the link to your WordPress http://<Web-Server-Public-IP-Address>/wordpress/
  
  
<img width="885" alt="first wordpress page" src="https://user-images.githubusercontent.com/85270361/135611523-517d013a-9466-4894-b399-92104d527aed.PNG">


<img width="960" alt="welcome to wordpress pass" src="https://user-images.githubusercontent.com/85270361/135611591-3d25af8f-d706-4871-b4cb-fa659c118e2a.PNG">
