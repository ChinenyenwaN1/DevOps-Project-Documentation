# CLIENT-SERVER-ARCHITECTURE-USING-A-MYSQL-RELATIONAL-DATABASE-MANAGEMENT-SYSTEM

## Client-Server Architecture

**Client-server architecture is an architecture of a computer network in which many clients (remote processors) request and receive service from a centralized server (host computer). Client computers provide an interface to allow a computer user to request services of the server and to display the results the server returns. Servers wait for requests to arrive from clients and then respond to them. Ideally, a server provides a standardized transparent interface to clients so that clients need not be aware of the specifics of the system (i.e., the hardware and software) that is providing the service. Clients are often situated at workstations or on personal computers, while servers are located elsewhere on the network, usually on more powerful machines. This computing model is especially effective when clients and the server each have distinct tasks that they routinely perform. In hospital data processing, for example, a client computer can be running an application program for entering patient information while the server computer is running another program that manages the database in which the information is permanently stored.**

**Client Server Architecture**

![image](https://user-images.githubusercontent.com/85270361/133891630-fe8bffb2-64bf-4e3a-8fa6-86ac0d0a9d20.png)


**At its simplest, the client/server architecture is about dividing up application processing into two or more logically distinct pieces. The database makes up half of the client/server architecture. The database is the “server”; any application that uses that data is a “client.” In many cases, the client and server reside on separate machines; in most cases, the client application is some sort of user-friendly interface to the database.**


![image](https://user-images.githubusercontent.com/85270361/133891791-09de575c-232e-4aa0-a07b-b80c9de989eb.png)

**A graphical representation of a simple client/server system.**

## Lets take a very quick example and see Client-Server communicatation in action.
* We open up out terminal and install *curl* if it doesnt exist.

```
$ sudo apt -y install curl
```

## In this example, the linux terminal will be the client, while www.propitixhomes.com will be the server

* Send a request from client (Linux Terminal) with the *curl* command below


```
$ curl -Iv www.propitixhomes.com
```

## We should see the response from the remote server in below output.

<img width="599" alt="Capture" src="https://user-images.githubusercontent.com/85270361/133892652-c3976695-5e58-4dac-bbb4-f48a5b8a41c7.PNG">


# In this project we will be Implementing a Client Server Architecture using a Database Management System (MySQL).

**The instructions below will be followed to complete the project.**

* We are going to configure 2 new Linux servers in our Virtualbox

```
Server A - mysql server
Server B - mysql client
```

* On mysql server Linux Server, we will install the MySQL software.

```
$ sudo apt install mysql-server -y
```

* We need to configure our mysql server installation using this command

```
$ sudo mysql_secure_installation
```


<img width="659" alt="Capture 3" src="https://user-images.githubusercontent.com/85270361/133924810-39cb0ce5-1f48-4da5-a245-0b4d6a809567.PNG">

<img width="726" alt="Capture 2" src="https://user-images.githubusercontent.com/85270361/133924820-3e413746-2dc3-4bf4-b44d-9e1ef55ad9bf.PNG">


* To test that our mysql server installation is successful we use this command


<img width="214" alt="mysq" src="https://user-images.githubusercontent.com/85270361/133927637-e2b72b64-1648-4638-8eaa-0b6aa9d548ac.PNG">


* Created a user on the mysql server with the following command

```
mysql> CREATE USER ann@'%' IDENTIFIED BY 'your_password';
```

```
mysql> GRANT ALL ON *.* TO ann@'%'; FLUSH PRIVILEGES;
```

### **We should see the below output**


<img width="947" alt="mysql" src="https://user-images.githubusercontent.com/85270361/133925109-61de8590-2b6f-457e-91c4-f930d5dd5ed4.PNG">


* **On mysql client Linux Server, we will install the MySQL client software.**

```
$ sudo apt install mysql-client -y
```

* Using the ip addr or ifconfig command, We will figure out the IP address of the mysql server Linux Server.

```
ifconfig
or
ipaddr
```

By default, both of your Ec2 virual serves are located in the same local virtual network, so they can communicate to eachother using the local ip addresses.
use *mysql server's* local ip address to connect from *mysql client* . MySQL Server uses TCP port 3306 by default, so you have to open it by creating a new entry in 'inbound rules' in '*MySQL server*' security groups.


<img width="958" alt="inbound" src="https://user-images.githubusercontent.com/85270361/133926263-10dcb910-0cc8-4d1f-9864-1c2bf7cc7ad6.PNG">

For extra security, do not allow all IP Addresses to reach '*MySQL Server*' allow access only to a specific local IP Address of '*MySQL Client*' .

1* **You might need to configure MySQL Server to allow connections from remote hosts.**


```
$ sudo iv /etc/mysql/mysql.config.d/mysqld.cnf
```

Replace '127.0.0.1' to '0.0.0.0'

As shown in the diagram

<img width="656" alt="bind" src="https://user-images.githubusercontent.com/85270361/133926606-74231a75-92ab-437c-b530-9da5138c2a79.PNG">

From '*MySQL Client*' Linux server connect remotely to '*MySQL Server*' DataBase Engine without using 'SSH'. You must use the mysql utility to perform this action.  


```
$ mysql -u remote_user -h IP-address -p
```

<img width="744" alt="connected to db" src="https://user-images.githubusercontent.com/85270361/133927008-08a5189e-33fb-4bc8-b257-13b1955f4ee4.PNG">


Check that you have successfully connected to a remote MySQL Server and can perform MySQL Queries.

```
Show databases;
```

<img width="688" alt="anndbase" src="https://user-images.githubusercontent.com/85270361/133927307-3b0529a1-ad44-4236-bf70-870c54a05a90.PNG">
