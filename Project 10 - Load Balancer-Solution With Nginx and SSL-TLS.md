# Load-Balancer-Solution-With-Nginx-and-SSL-TLS

## Load Balancing

**Load balancing** refers to efficiently distributing incoming network traffic across a group of backend servers.

A load balancer acts as the “traffic cop” sitting in front of your servers and routing client requests across all servers capable of fulfilling those requests in a manner that maximizes speed and capacity utilization and ensures that no one server is overworked, which could affects performance. If a single server goes down, the load balancer redirects traffic to the remaining online servers. When a new server is added to the server group, the load balancer automatically starts to send requests to it.

In this manner, a load balancer performs the following functions:

- Distributes client requests or network load efficiently across multiple servers
- Ensures high availability and reliability by sending requests only to servers that are online
- Provides the flexibility to add or subtract servers as demand dictates

![image](https://user-images.githubusercontent.com/116161693/228163764-84975383-6285-4721-822c-0ee6e9a01dc5.png)

## Configure Nginx As A Load Balancer

- **Create an Instance and name it nginx-lb**

- **Installing Nginx Web Server.**


```
sudo apt update

sudo apt install nginx
```
![image](https://user-images.githubusercontent.com/116161693/228164672-831faa94-09cd-46ec-a5f9-513f2aca6990.png)

- **Start Nginx at boot time**

```
sudo systemctl restart nginx

sudo systemctl status nginx
```

![image](https://user-images.githubusercontent.com/116161693/228164779-37a5ca09-b7db-42e6-923b-e636a527dd16.png)

- **Update /etc/hosts file for local DNS with our web servers**


```
sudo nano /etc/hosts
```
- **We will input our web server ip address there.**


```
3.86.148.131  web1
44.204.45.179 web2
```

![image](https://user-images.githubusercontent.com/116161693/228165054-ab234035-8c09-455b-b844-b8743602325f.png)

- **Let us now make use of above upstream and server block to create a load balancer for a domain. We need to create a configuration file using a text editor.**


```
sudo nano /etc/nginx/sites-available/load_balancer.conf
```

- **We need to imput the follow into the editor.**


```
upstream web {
   server 3.86.148.131;
   server 44.204.45.179;

}

server {
        listen 80;

        location / {

            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://web;
        }
}
```


- **We need to test our Nginx config file to know if there is any syntax error.**


```
sudo nginx -t
```
![image](https://user-images.githubusercontent.com/116161693/228171627-1a8c41b6-22a3-4273-b1b3-fb94057e1143.png)

- **We need to link our config file from site-enabled to site-available.**


```
cd /etc/nginx/sites-enabled/

sudo ln -s ../sites-available/load_balancer.conf .

sudo systemctl reload nginx
```

# Configure Secure Connection To The Load Balancer


## First we need to understand What Is TLS/SSL? and Why Do We Need It?


Secure Sockets Layer (SSL) technology is security that is implemented at the transport layer. SSL allows web browsers and web servers to communicate over a secure connection. In this secure connection, the data is encrypted before being sent and then is decrypted upon receipt and before processing. Both the browser and the server encrypt all traffic before sending any data.

SSL addresses the following important security considerations.


- Authentication: During your initial attempt to communicate with a web server over a secure connection, that server will present your web browser with a set of credentials in the form of a server certificate (also called a public key certificate). The purpose of the certificate is to verify that the site is who and what it claims to be. In some cases, the server may request a certificate proving that the client is who and what it claims to be; this mechanism is known as client authentication.

- Confidentiality: When data is being passed between the client and the server on a network, third parties can view and intercept this data. SSL responses are encrypted so that the data cannot be deciphered by the third party and the data remains confidential.

- Integrity: When data is being passed between the client and the server on a network, third parties can view and intercept this data. SSL helps guarantee that the data will not be modified in transit by that third party.



## What is an SSL Certificate And Why Is It Needed?

SSL certificates are used to secure data transfers, credit card transactions, logins and other personal information. They provide security to customers and make visitors more likely to stay on a website for longer periods of time.

The reason that visitors stay longer on websites when they see the green padlock from an SSL certificate is because it keeps information that is being transmitted between a web browser and a web server safe. This is especially important for websites that are dealing with sensitive information from customers and visitors, like personal information on a membership site or credit card information in an online store.


## Types of SSL Certificates

There are many different types of SSL certificate options available, all with their unique use cases and value propositions. The level of authentication assured by 
the Certificate Authority (CA) is a significant differentiator between the types. There are three recognized categories of SSL certificate authentication types:



- Extended Validation (EV)
- Organization Validation (OV)
- Domain Validation (DV)


# Configuring TLS for Tooling website using Letsencrypt

## What is TLS?

TLS is a cryptographic protocol that provides end-to-end security of data sent between applications over the Internet. It is mostly familiar to users through its use in secure web browsing, and in particular the padlock icon that appears in web browsers when a secure session is established. However, it can and indeed should also be used for other applications such as e-mail, file transfers, video/audioconferencing, instant messaging and voice-over-IP, as well as Internet services such as DNS and NTP.



## How does TLS work?

TLS uses a combination of symmetric and asymmetric cryptography, as this provides a good compromise between performance and security when transmitting data securely.

## REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES

Register a domain name with any registrar of your choice in any domain zone (e.g. .com, .net, .org, .edu, .info, .xyz or any other)
Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP
Create a static IP address, allocate the Elastic IP and associate it with an EC2 server to ensure your IP remain the same everytime you restart the instance.
Update **A record** in your registrar to point to Nginx LB using Elastic IP address

![image](https://user-images.githubusercontent.com/116161693/228178475-0e6d582e-4136-42d3-aebd-3a59bf72ba9b.png)

Check that your Web Servers can be reached from your browser using new domain name using HTTP protocol – http://nenyetech.ml

![image](https://user-images.githubusercontent.com/116161693/228178456-afef4060-2313-4c75-bc32-8bb31b56fcee.png)


Configure Nginx to recognize your new domain name, update your nginx.conf with server_name www.nenyetech.ml instead of server_name www.domain.com

## Letsencrypt

This is a solution geared at making the process of acquiring certificates from a certificate authority (CA) free, automated, and open. It is run by Internet Security Research Group and it gives people the digital certificates they need in order to enable HTTPS (SSL/TLS) for websites, for free, in the most user-friendly way we can


## Step 1: Get the Certbot client

To install Let’s Encrypt certificate, we need to use some client tool to facilitate the process. We will be using the certbot client in this project. However, there are other client tools that can be used. For example, ACMEv2

Certbot is an extensible client that fetches a security certificate from Let’s Encrypt Authority and lets you automate the validation and configuration of the certificate for use by the webserver.


- **Create a folder to do all your letsencrypt work. you can name it anything. Perhaps letsencrypt**


```
sudo curl -O https://dl.eff.org/certbot-auto
```
![image](https://user-images.githubusercontent.com/116161693/228172989-2158807a-1577-4866-bf77-31775ad9a3b4.png)

- **Move the certificate to the /usr/local/bin directory.**


```
sudo mv certbot-auto /usr/local/bin/certbot-auto
```

- **Assign file permission to the certbot file**


```
sudo chmod 0755 /usr/local/bin/certbot-auto
```

## Ensure Nginx already work based on http before we update Nginx configuration to use https


## Step 2: Install Lets Encrypt Certificate

Use certbot command to initialize the fetching and configuration of Let’s Encrypt security certificate. the flag --nginx tells certbot to automatically update nginx configuration file accordingly. Certbot is a python application, hence it will install multiple Python packages and their dependencies.


```
sudo /usr/local/bin/certbot-auto --nginx -d nenyetech.ml
```

Follow the interactive prompt and answer the questions as requested. You will be asked to provide an email address and accept the terms of service. If everything works out well, you will get a congratulations message at the end. To confirm that your Nginx site is encrypted, reload the webpage and observe the padlock symbol at the beginning of the URL. This indicates that the site is secured using an SSL/TLS encryption.

![image](https://user-images.githubusercontent.com/116161693/228173940-061b4e75-22ae-45fb-9576-63a71accab66.png)

Check that your Web Servers can be reached from your browser using new domain name using HTTP protocol – http://nenyetech.ml

![image](https://user-images.githubusercontent.com/116161693/228175269-dafd132f-afe7-479e-96b9-29ae10a8650d.png)

![image](https://user-images.githubusercontent.com/116161693/228175205-b221ca80-c9fc-466f-b456-1ecaf7110c87.png)


## Step 2: Set up Automatic Renewal

Right now, we have a valid certificate from LEtsencrypt. But we’ll also need to make sure that our certificate stays renewed and valid.

By default, Let’s Encrypt certificates are valid for 90 days. It is recommended to renew the certificate before it expires since an expired certificate will give
users a safety warning when they try to visit your website.

You can test the renewal process manually with a --dry-run flag. So that you can understand what happens internally.

Simply run


```
certbot-auto renew --dry-run
```

The above command will automatically check the currently installed certificates and will attempt to renew them if they are less than 30 days away from the 
expiration date.

Best pracice is to have some scheduler to run a job that runs that command periodically. Assuming we want to always run the command twice everyday. We can use 
cronjob to do the job.


- **To do so, lets edit the crontab file with the following command:**

```
crontab -e
```

![image](https://user-images.githubusercontent.com/116161693/228174350-5a35a7e1-d049-44cb-bf06-2781c4d6146f.png)

- **Add the following line:**

```
* */12 * * *   root /usr/local/bin/certbot-auto renew >/dev/null 2>&1
```
We can always change the interval of this cronjob if twice a day is too often by adjusting the values on the far left.
