# DEVOPS TOOLING WEBSITE SOLUTION
![Capture](https://user-images.githubusercontent.com/74002629/183053774-9dddd124-bdb1-4e78-b077-e5877b85fb33.PNG)

### Prerequites
Provision 4 Red Hat Enterprise Linux 8. One will be the NFS server and the other as the Web servers.
Provision 1 Ubuntu 20.04 for the the databaes server.
![image](https://user-images.githubusercontent.com/116161693/214545961-1dde13ec-4dcf-4700-838e-5508475b3cd0.png)

### Step 1 - Prepare NFS server

![image](https://user-images.githubusercontent.com/116161693/214545932-fcd6c078-30ac-49e3-9071-0814328817b0.png)

To view all logical volumes, run the command `lsblk` The 3 newly created block devices are names **xvdf**, **xvdh**, **xvdg** respectively.
Use gdisk utility to create a single partition on each of the 3 disks 
```
sudo gdisk /dev/xvdf
```
![image](https://user-images.githubusercontent.com/116161693/214546016-6ccabdc9-ac9a-4410-a9e8-3d891c8792ca.png)

A prompt pops up, type `n` to create new partition, enter no of partition(1), hex code is 8300, `p` to view partition and `w` to save newly created partition.
Repeat this process for the other remaining block devices.

```
sudo gdisk /dev/xvdh
sudo gdisk /dev/xvdg
```
![image](https://user-images.githubusercontent.com/116161693/214546079-43cead31-3a4e-4388-817c-77ad00d5863a.png)

Type lsblk to view newly created partition.

![image](https://user-images.githubusercontent.com/116161693/214546188-9d7be661-6d01-4072-99d4-9788a29496d9.png)

Install lvm2 package by running the below command to check for available partitions.

```
sudo yum install lvm2
``` 
![image](https://user-images.githubusercontent.com/116161693/214546219-8e44d0be-a93b-48ec-9a76-a2aaf162dffc.png)

```
sudo lvmdiskscan
``` 
Create physical volume to be used by lvm by using the pvcreate command:
```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```
- To check if the PV have been created successfully, run: `sudo pvs`
- Next, Create the volume group and name it webdata-vg: `sudo vgcreate webdata-vg /dev/xvdf1 /dev/xvdg1 /dev/xvdh1`
- View newly created volume group type: `sudo vgs`

![image](https://user-images.githubusercontent.com/116161693/214547252-3e067ef3-4d79-43fe-b6d0-f3097000c129.png)

- Create 3 logical volumes using lvcreate utility. Name them: lv-apps for storing data for the website, lv-logs for storing data for logs and lv-opt for Jenkins Jenkins server in project 8.
```
sudo lvcreate -n lv-apps -L 9G webdata-vg
sudo lvcreate -n lv-logs -L 9G webdata-vg
sudo lvcreate -n lv-opt -L 9G webdata-vg
```
- Verify Logical Volume has been created successfully by running: `sudo lvs`
- 
![image](https://user-images.githubusercontent.com/116161693/214547530-89cdf293-cd13-492c-b5b2-e68e9c8899ba.png)

- then format the logical volumes with ext4 filesystem:
```
sudo mkfs -t xfs /dev/webdata-vg/lv-apps
sudo mkfs -t xfs /dev/webdata-vg/lv-logs
sudo mkfs -t xfs /dev/webdata-vg/lv-opt
```
- Next, create mount points for the logical volumes. Create **/mnt/apps** the following directory to store website files: 
```
sudo mkdir /mnt/apps
sudo mkdir /mnt/logs
sudo mkdir /mnt/opt
```
- Mount to **/dev/webdata-vg/lv-apps** **/dev/webdata-vg/lv-apps** and **/dev/webdata-vg/lv-opt** respectievly : 
```
sudo mount /dev/webdata-vg/lv-apps /mnt/apps
sudo mount /dev/webdata-vg/lv-logs /mnt/logs
sudo mount /dev/webdata-vg/lv-opt /mnt/opt
```
![image](https://user-images.githubusercontent.com/116161693/214547599-714238d6-dff0-4b65-b369-f524de31f778.png)

Install NFS server, configure it to start on reboot and make sure it is up and running
```
sudo yum -y update
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```
![image](https://user-images.githubusercontent.com/116161693/214547649-f2b7f1db-386b-49cb-b4a7-a79a1ddec6b0.png)
![image](https://user-images.githubusercontent.com/116161693/214547705-6dd4c27f-9b4d-45e7-b01d-38b81897a5e4.png)

- Export the mounts for webservers’ subnet cidr to connect as clients. For simplicity, install your all three Web Servers inside the same subnet, but in production set up you would probably want to separate each tier inside its own subnet for higher level of security.
- Set up permission that will allow our Web servers to read, write and execute files on NFS:
```
sudo chown -R nobody: /mnt/apps
sudo chown -R nobody: /mnt/logs
sudo chown -R nobody: /mnt/opt

sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt

sudo systemctl restart nfs-server.service
```
![image](https://user-images.githubusercontent.com/116161693/214547829-a7363978-b659-4370-a6bf-c4ae19b7b000.png)

using text editor, configure access to NFS for clients within the same subnet (my Subnet CIDR – 172.31.80.0/20 ):
```
sudo vi /etc/exports
```

/mnt/apps 10.0.2.0/24(rw,sync,no_all_squash,no_root_squash)
/mnt/logs 10.0.2.0/24(rw,sync,no_all_squash,no_root_squash)
/mnt/opt 10.0.2.0/24(rw,sync,no_all_squash,no_root_squash)

Esc + :wq!

![image](https://user-images.githubusercontent.com/116161693/214547953-e74adcef-9a74-4729-b2cb-b81885fe20ef.png)

```
sudo exportfs -arv
```
Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)

![image](https://user-images.githubusercontent.com/116161693/214548402-c1802031-f793-4414-a360-e665e6365285.png)

```
rpcinfo -p | grep nfs
```

![image](https://user-images.githubusercontent.com/116161693/214548043-bbc7befe-6393-4407-b754-2732dd5d04f4.png)

### CONFIGURE THE DATABASE SERVER
Install and configure a MySQL DBMS to work with remote Web Server
SSH in to the provisioned DB server and run an update and install mysql

```
sudo apt update
```

![image](https://user-images.githubusercontent.com/116161693/214548449-3d30976b-1343-4a43-9be3-f2e2ef24bb05.png)

```
sudo apt install mysql-server -y
```

![image](https://user-images.githubusercontent.com/116161693/214548480-f783bb73-f26b-4d95-9f60-6033915fa8e2.png)

Create a database and name it **tooling**: 

```
sudo mysql
```
![image](https://user-images.githubusercontent.com/116161693/214548730-f5380a2e-a8f0-4858-85e7-d85794948943.png)

create database tooling;
Create a database user and name it **webaccess** and grant permission to **webaccess** user on tooling database to do anything only 
from the webservers subnet cidr:
```
create user 'webaccess'@'10.0.2.0/24' identified by 'password';
grant all privileges on tooling.* to 'webaccess'@'10.0.2.0/24';
flush privileges;
```
- To show database run: 
```
show databases;
```
![image](https://user-images.githubusercontent.com/116161693/214548843-f8dc999c-8151-4461-a03f-6bcf59ae8e40.png)

### Step 3 — Prepare the Web Servers

- Install NFS client on the webserver1: `sudo yum install nfs-utils nfs4-acl-tools -y`

![image](https://user-images.githubusercontent.com/116161693/214549354-7c332cba-9f81-4080-982e-d9320b11c624.png)

- Mount /var/www/ and target the NFS server’s export for apps (Use the private IP of the NFS server)
```
sudo mkdir /var/www
sudo mount -t nfs -o rw,nosuid 172.31.85.14:/mnt/apps /var/www
```
![image](https://user-images.githubusercontent.com/116161693/214549309-ef4e409f-8dca-46ad-8f6c-518e0e8568da.png)

Verify that NFS was mounted successfully by running `df -h` Make sure that the changes will persist on Web Server after reboot:
```
sudo vi /etc/fstab
```
- Add the following line in the configuration file: 
```
10.0.2.153:/mnt/apps /var/www nfs defaults 0 0
```
![image](https://user-images.githubusercontent.com/116161693/214550009-78a3ad4e-a59d-43dc-98bb-b352e7a9a7f5.png)

- Install Remi’s repository, Apache and PHP:
```
sudo yum install httpd -y
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo dnf module reset php
sudo dnf module enable php:remi-7.4
sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
setsebool -P httpd_execmem 1
```

![image](https://user-images.githubusercontent.com/116161693/214551322-abb20b11-dbe8-4e49-b600-93fac36a8337.png)

6. Repeat steps 1-5 for the other 2 webservers
7. Verify that Apache files and directories are available on the Web Server in /var/www and also on the NFS server in /mnt/apps.

![image](https://user-images.githubusercontent.com/116161693/214549938-2a298dc0-67c8-43a9-aed9-3159c96ff4f0.png)
![image](https://user-images.githubusercontent.com/116161693/214549907-09249e98-a015-49b8-8820-352e85d9560a.png)

8. Locate the log folder for Apache on the Web Server and mount it to NFS server’s export for logs. Repeat step №3 and №4 to make sure the mount point will persist after reboot:
```
sudo mount -t nfs -o rw,nosuid 10.0.2.153:/mnt/logs /var/log/httpd
sudo vi /etc/fstab
10.0.2.153:/mnt/logs /var/log/httpd nfs defaults 0 0
```
- Fork the tooling source code from **https://github.com/ChinenyenwaN1/tooling.git** Github Account to your Github account. 
Begin by installing git on the webserver: `sudo yum install git -y`
Initialize Git: `git init`

Then run: `git clone https://github.com/darey-io/tooling.git`

![image](https://user-images.githubusercontent.com/116161693/214550880-1c116115-2e29-4ab1-b6a4-c657d1e5f936.png)

Deploy the tooling website’s code to the Webserver. Ensure that the html folder from the repository is deployed to /var/www/html

On the webserver, ensure port 80 in open to all traffic in the security groups.
Update the website’s configuration to connect to the database: `sudo vi /var/www/html/functions.php`


Apply tooling-db.sql script to your database using this command `mysqli_connect ('172.31.80.140', 'webaccess', 'password', 'tooling')`
In the database server update the bind address to 0.0.0.0: `sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`

run 
```
mysql -h 10.0.2.244 -u webaccess -p tooling < tooling-db.sql
```
![image](https://user-images.githubusercontent.com/116161693/214551546-f9f20ab7-f0a6-4887-8528-0ad6ec4d0735.png)

Then create in MySQL a new admin user with username: myuser and password: password:
```
INSERT INTO ‘users’ (‘id’, ‘username’, ‘password’, ’email’, ‘user_type’, ‘status’) VALUES (1, ‘myuser’, ‘8b333b5aa765d61d1324bagf3’, ‘chinenye@gmail.com’, ‘admin’, ‘1’);
```
![image](https://user-images.githubusercontent.com/116161693/214551956-6728b6fb-ef2d-43b2-b7a0-b39c5970969a.png)

![image](https://user-images.githubusercontent.com/116161693/214552106-1a4c6bf4-803f-4498-9359-8c1d5373ead6.png)

Finally, open the website in your browser with the public IP of the webserver and make sure you can login into the website with admin user.

![image](https://user-images.githubusercontent.com/116161693/214552245-c5bc06e1-1786-4c58-98e4-dfe3b8005ba5.png)

![image](https://user-images.githubusercontent.com/116161693/214552172-81718df1-b3e5-4a35-b5b4-301dbe642b52.png)

