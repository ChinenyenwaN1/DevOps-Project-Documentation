# LOAD BALANCER SOLUTION WITH APACHE
![Capture](https://user-images.githubusercontent.com/74002629/183334671-0641051c-31e2-44e9-950c-b2f7197b6343.PNG)

## Configure Apache As A Load Balancer

- Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb.
![image](https://user-images.githubusercontent.com/116161693/218478729-b23f5ea5-f21e-4966-b3cf-585e6214ef05.png)

Open TCP port 80 on **Project-8-apache-lb** by creating an Inbound Rule in Security Group.
Connect to the server through the SSh terminal and install Apache Load balancer then configure it to point traffic coming to LB to the Web Servers by running the following:
```
sudo apt update
```
![image](https://user-images.githubusercontent.com/116161693/218479134-fa6ccadc-8bb7-4b05-ba96-5433cd7bcc9f.png)

```
sudo apt install apache2 -y
```
![image](https://user-images.githubusercontent.com/116161693/218479268-b9090572-4257-4f80-b1cb-8471d68f8d9c.png)

```
sudo apt-get install libxml2-dev
```
![image](https://user-images.githubusercontent.com/116161693/218479325-7c6373e4-1096-46ce-b188-14cac4989350.png)

Enable the following and restart the service:
```
sudo a2enmod rewrite
sudo a2enmod proxy
sudo a2enmod proxy_balancer
sudo a2enmod proxy_http
sudo a2enmod headers
sudo a2enmod lbmethod_bytraffic
```
![image](https://user-images.githubusercontent.com/116161693/218479629-404512b4-f430-4585-b969-b2f3b662fd05.png)

```
sudo systemctl restart apache2
```
Ensure Apache2 is up and running: `sudo systemctl status apache2`
![image](https://user-images.githubusercontent.com/116161693/218479677-4eece0c0-092c-4c7c-9d5a-4f1f6d0f67ab.png)

Next, configure load balancing in the default config file: `sudo vi /etc/apache2/sites-available/000-default.conf`
In the config file add and save the following configuration into this section **<VirtualHost *:80>  </VirtualHost>** making sure to enter the IP of the webservers 
```
<Proxy "balancer://mycluster">
               BalancerMember http://10.0.26:80 loadfactor=5 timeout=1
               BalancerMember http://10.0.26:80 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/
```
![image](https://user-images.githubusercontent.com/116161693/218480229-c25717c8-7073-47e5-a733-697962585804.png)

8. Restart Apache server: `sudo systemctl restart apache2`
9. Verify that our configuration works – try to access your LB’s public IP address or Public DNS name from your browser:
`http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php`

![image](https://user-images.githubusercontent.com/116161693/218480421-ed85dd94-3897-4c34-83e5-97ce19e11a8a.png)

* The load balancer accepts the traffic and distributes it between the servers according to the method that was specified.
10. In Project-7 I had mounted **/var/log/httpd/** from the Web Servers to the NFS server, here I shall unmount them and give each Web Server has its own log directory: `sudo umount -f /var/log/httpd`
11. Open two ssh/Putty consoles for both Web Servers and run following command: `sudo tail -f /var/log/httpd/access_log`
12.  Refresh the browser page `http://10.0.2.92/index.php` with the load balancer public IP several times and make sure that both servers receive HTTP GET requests from your LB – new records will appear in each server’s log file. The number of requests to each server will be approximately the same since we set loadfactor to the same value for both servers – it means that traffic will be disctributed evenly between them.
![image](https://user-images.githubusercontent.com/116161693/218480678-91db1090-71d6-4fd8-ad31-82825186b89f.png)

![image](https://user-images.githubusercontent.com/116161693/218480721-1e92aa1d-18b9-4c7f-8b49-fed4e3780f03.png)


### Step 2 – Configure Local DNS Names Resolution
1. Sometimes it may become tedious to remember and switch between IP addresses, especially when you have a lot of servers under your management.
We can solve this by configuring local domain name resolution. The easiest way is to use **/etc/hosts file**, although this approach is not very scalable, but it is very easy to configure and shows the concept well. 
2. Open this file on your LB server: `sudo vi /etc/hosts`
3. Add 2 records into this file with Local IP address and arbitrary name for both of your Web Servers
```
10.0.2.26 web1
10.0.2.242 web2
```
![image](https://user-images.githubusercontent.com/116161693/218480780-52adb7c0-d210-443a-be84-9870e20535f6.png)

Now you can update your LB config file with those names instead of IP addresses.
```
BalancerMember http://Web1:80 loadfactor=5 timeout=1
BalancerMember http://Web2:80 loadfactor=5 timeout=1
```
![image](https://user-images.githubusercontent.com/116161693/218480989-81f29146-5846-45a1-9ba6-03b0b0a691ee.png)

You can try to curl your Web Servers from LB locally `curl http://Web1` or `curl http://Web2` to see the HTML formated version of your website.

![image](https://user-images.githubusercontent.com/116161693/218481131-8b677963-4bf9-4fcf-9dd3-8dc539680773.png)
![image](https://user-images.githubusercontent.com/116161693/218481179-b4f22dcb-c274-4060-9d18-f555a0404ac7.png)


