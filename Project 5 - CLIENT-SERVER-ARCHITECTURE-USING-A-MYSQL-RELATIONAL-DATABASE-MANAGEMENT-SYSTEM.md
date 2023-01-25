 # CLIENT-SERVER-ARCHITECTURE-USING-A-MYSQL-RELATIONAL-DATABASE-MANAGEMENT-SYSTEM

## Client-Server Architecture

**Client-server architecture is an architecture of a computer network in which many clients (remote processors) request and receive service from a centralized server (host computer). Client computers provide an interface to allow a computer user to request services of the server and to display the results the server returns. Servers wait for requests to arrive from clients and then respond to them. Ideally, a server provides a standardized transparent interface to clients so that clients need not be aware of the specifics of the system (i.e., the hardware and software) that is providing the service. Clients are often situated at workstations or on personal computers, while servers are located elsewhere on the network, usually on more powerful machines. This computing model is especially effective when clients and the server each have distinct tasks that they routinely perform. In hospital data processing, for example, a client computer can be running an application program for entering patient information while the server computer is running another program that manages the database in which the information is permanently stored.**

**Client Server Architecture**

![image](https://user-images.githubusercontent.com/116161693/209810523-fd9ad4ac-5b0c-47fd-aab5-08494ff3f146.png)

**At its simplest, the client/server architecture is about dividing up application processing into two or more logically distinct pieces. The database makes up half of the client/server architecture. The database is the “server”; any application that uses that data is a “client.” In many cases, the client and server reside on separate machines; in most cases, the client application is some sort of user-friendly interface to the database.**

![image](https://user-images.githubusercontent.com/85270361/133891791-09de575c-232e-4aa0-a07b-b80c9de989eb.png)

**A graphical representation of a simple client/server system.**

# In this project we will be Implementing a Client Server Architecture using a Database Management System (MySQL).

* We are going to configure 2 new Linux servers in our Virtualbox
```
Server A - mysql server
Server B - mysql client
```

![image](https://user-images.githubusercontent.com/116161693/209873871-12d6f898-5fd3-4c5d-8a55-e47dd053fac0.png)

* On mysql server Linux Server, we will install the MySQL software.

```
$ sudo apt install mysql-server -y
```
![image](https://user-images.githubusercontent.com/116161693/209873856-b78302e8-f5ec-422b-bf5c-5abcacedae8f.png)

* We need to configure our mysql server installation using this command
```
sudo mysql
```

```
sudo mysql_secure_installation
```
![image](https://user-images.githubusercontent.com/116161693/209874158-59c3dbb5-7be0-4454-988a-a77206c866d5.png)

login into mysql

![image](https://user-images.githubusercontent.com/116161693/209874598-ed181751-fac7-4386-ad99-ab285e7b19f8.png)

* Created a user on the mysql server with the following command

```
Create User 'remote_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```

![image](https://user-images.githubusercontent.com/116161693/209874632-17aceefc-1029-4e9f-bac2-bbb63acaa6c0.png)

```
CREATE DATABASE test_db;
```
![image](https://user-images.githubusercontent.com/116161693/209874855-ecb3dda2-7ebe-46d1-8aad-76c3ff9cd169.png)

```
GRANT ALL ON test_db.* TO 'remote_user'@'%'; FLUSH PRIVILEGES;
```

![image](https://user-images.githubusercontent.com/116161693/209874926-50d60e9b-7939-43ee-8cfa-6fc598dc986e.png)

* **On mysql client Linux Server, we will install the MySQL client software.**

```
sudo apt install mysql-client -y
```

* Using the ip addr or ifconfig command, We will figure out the IP address of the mysql server Linux Server.

```
ifconfig
or
ipaddr
```

By default, both of your Ec2 virual serves are located in the same local virtual network, so they can communicate to eachother using the local ip addresses.
use *mysql server's* local ip address to connect from *mysql client* . MySQL Server uses TCP port 3306 by default, so you have to open it by creating a new entry in 'inbound rules' in '*MySQL server*' security groups.

![image](https://user-images.githubusercontent.com/116161693/209875228-6fcfa9b1-4cab-4c61-993b-00091a3e86b9.png)

For extra security, do not allow all IP Addresses to reach '*MySQL Server*' allow access only to a specific local IP Address of '*MySQL Client*' .

1* **You might need to configure MySQL Server to allow connections from remote hosts.**

```
sudo iv /etc/mysql/mysql.config.d/mysqld.cnf
```

![image](https://user-images.githubusercontent.com/116161693/209875672-9ac9c43d-1033-4324-a9fd-bbb4f4fb7100.png)

Replace '127.0.0.1' to '0.0.0.0'

As shown below

![image](https://user-images.githubusercontent.com/116161693/209875565-22f39104-6d69-4090-b6bf-345653ee83fb.png)

From '*MySQL Client*' Linux server connect remotely to '*MySQL Server*' DataBase Engine without using 'SSH'. You must use the mysql utility to perform this action.  

```
mysql -u remote_user -h IP-address -p
```

![image](https://user-images.githubusercontent.com/116161693/209875719-74bc2f41-9a01-4a79-a264-9accf5d393fc.png)


Check that you have successfully connected to a remote MySQL Server and can perform MySQL Queries.

```
Show databases;
```

![image](https://user-images.githubusercontent.com/116161693/209875756-70d4e1ca-5b53-446b-8b42-499b5396d7b5.png)



