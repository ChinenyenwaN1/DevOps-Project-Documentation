# TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION.

**what is Continuous Integration**

Continuous Integration is a development practice in which the developers are required to commit changes to the source code in a shared repository several times a day or more frequently. Every commit made in the repository is then built. This allows the teams to detect the problems early. Apart from this, depending on the Continuous Integration tool, there are several other functions like deploying the build application on the test server, providing the concerned teams with the build and test results, etc.

### INTRODUCTION TO JENKINS
Jenkins is an open-source automation tool written in Java with plugins built for continuous integration. Jenkins is used to build and test your software projects continuously making it easier for developers to integrate changes to the project, and making it easier for users to obtain a fresh build. It also allows you to continuously deliver your software by integrating with a large number of testing and deployment technologies.

With Jenkins, organizations can accelerate the software development process through automation. Jenkins integrates development life-cycle processes of all kinds, including build, document, test, package, stage, deploy, static analysis, and much more.

Jenkins achieves Continuous Integration with the help of plugins. Plugins allow the integration of Various DevOps stages. If you want to integrate a particular tool, you need to install the plugins for that tool. For example Git, Maven 2 project, Amazon EC2, HTML publisher etc.

The image below depicts that Jenkins is integrating various DevOps stages:
![image](https://user-images.githubusercontent.com/116161693/224283027-a91b7308-81ab-457b-a9bf-0c51e93ff6a0.png)

**Enhance the architecture prepared in Project 8 by adding a Jenkins server, configure a job to automatically deploy source codes changes from Git to NFS server.**

![Capture](https://user-images.githubusercontent.com/74002629/184101792-3c29fba2-78dd-4333-8aee-3385c605ecf1.PNG)

#### Step 1- Install Jenkins server
1. Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"
2. Install JDK (Java development kit) since Jenkins is a Java-based application:
```
sudo apt update
sudo apt install default-jdk-headless
```
![image](https://user-images.githubusercontent.com/116161693/224292955-26ff2660-9471-4442-9641-8a8131194a19.png)

3. Install Jenkins:
```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt-get install jenkins
```
![image](https://user-images.githubusercontent.com/116161693/224293016-50d7ae68-e9bd-418b-939d-1de9b8275ba1.png)

4. Make sure Jenkins is up and running: `sudo systemctl status jenkins`
![image](https://user-images.githubusercontent.com/116161693/224293064-2a50b010-ad0f-40cb-9097-1f05c323aff1.png)

5. By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group
6. Next, setup Jenkins. From your browser access `http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080` You will be prompted to provide a default admin password
![image](https://user-images.githubusercontent.com/116161693/224293914-a10710be-33f4-469f-8f3f-27c9e43241be.png)

7. Retrieve the password from your Jenkins server: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` you will see the password for your jenkins that looks like the below on your terminal
   **0c86a6a9a54c407ca4a42f0a35afc1ed**

8. Copy the password from the server and paste on Jenkins setup to unlock Jenkins.
9. Next, you will be prompted to install plugins – **choose suggested plugins**
![image](https://user-images.githubusercontent.com/116161693/224294225-17753233-bcec-4b62-92d3-4520e63bf00d.png)

10. Once plugins installation is done – create an admin user and you will get your Jenkins server address. **The installation is completed!** I decided to use my Admin User.
![image](https://user-images.githubusercontent.com/116161693/224294268-ba24fb56-bba2-449b-9f08-633f8fab1854.png)

#### Step 2 - Configure Jenkins to retrieve source codes from GitHub using Webhooks
Here I configure a simple Jenkins job/project. This job will will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.

1. Enable webhooks in your GitHub repository settings: 
```
Go to the tooling repository
Click on settings
Click on webhooks on the left panel
On the webhooks page under Payload URL enter: `http:// Jenkins server IP address/github-webhook`
Under content type select: application/json
Then add webhook
```
![image](https://user-images.githubusercontent.com/116161693/224294550-b5d4b430-06d3-407a-9d73-05562fe6b453.png)

2. Go to Jenkins web console, click **New Item** and create a **Freestyle project** and click OK
3. Connect your GitHub repository, copy the repository URL from the repository
4. In configuration of your Jenkins freestyle project under Source Code Management select **Git repository**, provide there the link to your Tooling GitHub repository and credentials (user/password) so Jenkins could access files in the repository.
![image](https://user-images.githubusercontent.com/116161693/224294624-ed1104d4-47f3-45be-b914-245c07e1b165.png)

5. Save the configuration and let us try to run the build. For now we can only do it manually.
6. Click **Build Now** button, if you have configured everything correctly, the build will be successfull and you will see it under **#1**
7. Open the build and check in **Console Output** if it has run successfully.
![image](https://user-images.githubusercontent.com/116161693/224294685-595e7b58-f313-4590-b1c0-ad1ee96164e7.png)

This build does not produce anything and it runs only when it is triggered manually. Let us fix it.
8. Click **Configure** your job/project and add and save these two configurations:
``` 
Under **Build triggers** select: Github trigger for GITScm polling
Under **Post Build Actions** select Archieve the artifacts and enter `**` in the text box.
```
9. Now, go ahead and make some change in any file in your GitHub repository (e.g. README.MD file) and push the changes to the master branch.
10. You will see that a new build has been launched automatically (by webhook) and you can see its results – artifacts, saved on Jenkins server.
11. We have successfully configured an automated Jenkins job that receives files from GitHub by webhook trigger (this method is considered as ‘push’ because the changes are being ‘pushed’ and files transfer is initiated by GitHub).
![image](https://user-images.githubusercontent.com/116161693/224295222-c1ff0170-48f0-4d60-b61a-6b9fe20cf0e8.png)

12. By default, the artifacts are stored on Jenkins server locally: `ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/`

#### Step 3 – Configure Jenkins to copy files to NFS server via SSH
1. Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory. We need a plugin called
**Publish over SSh**
2. Install "Publish Over SSH" plugin.
3. Navigate to the dashboard select **Manage Jenkins** and choose **Manage Plugins** menu item.
4. On **Available** tab search for **Publish Over SSH** plugin and install it
![image](https://user-images.githubusercontent.com/116161693/224295339-9f0a1ab3-410f-4f9b-aeeb-e58f102c4a25.png)

5. Configure the job/project to copy artifacts over to NFS server.
6. On main dashboard select **Manage Jenkins** and choose **Configure System** menu item.
7. Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:
```
Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty)
Name- NFS
Hostname – can be private IP address of your NFS server
Username – ec2-user (since NFS server is based on EC2 with RHEL 8)
Remote directory – /mnt/apps since our Web Servers use it as a mointing point to retrieve files from the NFS server
```
8. Test the configuration and make sure the connection returns **Success** Remember, that TCP port 22 on NFS server must be open to receive SSH connections.
9. Save the configuration and open your Jenkins job/project configuration page and add another one Post-build Action: **Set build actionds over SSH**
10. Configure it to send all files probuced by the build into our previouslys define remote directory. In our case we want to copy all files and directories – so we use **

11. Save this configuration and go ahead, change something in **README.MD** file the GitHub Tooling repository.
12. Webhook will trigger a new job and in the "Console Output" of the job you will find something like this:
```
SSH: Transferred 25 file(s)
Finished: SUCCESS
```
13. To make sure that the files in /mnt/apps have been updated – connect via SSH/Putty to your NFS server and check README.MD file: `cat /mnt/apps/README.md`
14. If you see the changes you had previously made in your GitHub – the job works as expected.
![image](https://user-images.githubusercontent.com/116161693/224297653-2851b518-9a72-4411-bf9f-034f663b5c0f.png)

**Note** I got a "Permission denied" error which indicated that my build was not successful
this issue was resolved by changing mode and ownership on the NFS server with the below command:
```
ll /mnt
sudo chown -R nobody:nobody /mnt
sudo chmod -R 777 /mnt
```



