## ANSIBLE CONFIGURATION MANAGEMENT – AUTOMATE PROJECT 7 TO 10

![image](https://user-images.githubusercontent.com/116161693/233007313-6d3c10c0-7d01-49aa-b801-3e2ca0149358.png)

#### Task
- Install and configure Ansible client to act as a Jump Server/Bastion Host
- Create a simple Ansible playbook to automate servers configuration

### Install and configure Ansible client to act as a Jump Server/Bastion Host

An SSH jump server is a regular Linux server, accessible from the Internet, which is used as a gateway to access other Linux machines on a private network using the SSH protocol. Sometimes an SSH jump server is also called a “jump host” or a “bastion host”. The purpose of an SSH jump server is to be the only gateway for access to your infrastructure reducing the size of any potential attack surface.

#### Step 1 - INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE
This is a continuation of [project 9](https://github.com/ChinenyenwaN1/DevOps-Project-Documentation/blob/main/Project%209%20-%20Continuous%20Integration%20Pipeline%20for%20tooling%20website.md), update Name tag on your Jenkins EC2 Instance to **Jenkins-Ansible**. This server will be used to run playbooks.
In your GitHub account create a new repository and name it **ansible-config-mgt**.
In your **Jenkin-Ansible** server, instal **Ansible**
```
sudo apt update
sudo apt install ansible
```
Check your Ansible version by running `ansible --version`
![image](https://user-images.githubusercontent.com/116161693/233055574-f5268f61-0f84-4105-ba68-8ef233ab7e18.png)

Configure Jenkins build job to save your repository content every time you change it. See [project 9](https://github.com/ChinenyenwaN1/DevOps-Project-Documentation/blob/main/Project%209%20-%20Continuous%20Integration%20Pipeline%20for%20tooling%20website.md) for detailed steps
- Create a new Freestyle project ansible in Jenkins and point it to your **ansible-config-mgt** repository.
- Configure Webhook in GitHub and set webhook to trigger ansible build.
  ![image](https://user-images.githubusercontent.com/116161693/233049004-9d3317d1-5394-4382-a98a-22cafca57256.png)

- Configure a Post-build job to save all (**) files. 
- Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder `ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/`
  ![image](https://user-images.githubusercontent.com/116161693/233049703-e779bf0f-5410-4299-9ee7-950d3364907c.png)
  ![image](https://user-images.githubusercontent.com/116161693/233055181-baaf9531-d9dc-4835-aaae-58997fb73c31.png)
    
#### Step 2 – Prepare your development environment using Visual Studio Code
- Install Visual Studio Code (VSC)- an Integrated development environment (IDE) or Source-code Editor. You can get it [here](https://code.visualstudio.com/download)
- After you have successfully installed VSC, configure it to connect to your newly created GitHub repository.
- Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance: `git clone <ansible-config-mgt repo link>`

### Create a simple Ansible playbook to automate servers configuration

#### Step 3 - Begin Ansible development
- In your **ansible-config-mgt** GitHub repository, create a new branch that will be used for development of a new feature.
- Checkout the newly created feature branch to your local machine and start building your code and directory structure
- Create a directory and name it **playbooks** – it will be used to store all your playbook files.
- Create a directory and name it **inventory** – it will be used to keep your hosts organised.
- Inside the playbooks folder, create your first playbook, and name it **common.yml**
- Inside the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) **dev**, **staging**, **uat**, and **prod** respectively.

![image](https://user-images.githubusercontent.com/116161693/233051411-8514d728-7654-4170-90eb-60e42cd0295e.png)

#### Step 4 – Set up an Ansible Inventory
- An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since the intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

- Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.
- Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host – for this you can implement the concept of ssh-agent. Now you need to import your key into ssh-agent: 

```
eval `ssh-agent -s`
ssh-add <path-to-private-key>
```
- Confirm the key has been added with this command, you should see the name of your key: `ssh-add -l`
![image](https://user-images.githubusercontent.com/116161693/233050056-9af96918-0651-4591-9fb2-c6f0e5a2f372.png)

- Now, ssh into your Jenkins-Ansible server using ssh-agent: `ssh -A ubuntu@public-ip`
- Also notice, that your ubuntu user is ubuntu and user for RHEL-based servers is ec2-user.
- Update your inventory/dev.yml file with this snippet of code:
```
[nfs]
<NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

[webservers]
<Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
<Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'

[db]
<Database-Private-IP-Address> ansible_ssh_user='ubuntu' 

[lb]
<Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu'
```
![image](https://user-images.githubusercontent.com/116161693/233051740-5e066ff5-d1de-4ea4-ac35-f69554366cc1.png)

#### Step 5 – Create a Common Playbook
Now we give Ansible the instructions on what you needs to be performed on all servers listed in **inventory/dev**. In **common.yml** playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.
- Update your playbooks/common.yml file with following code:
```
---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest

- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest
```
![image](https://user-images.githubusercontent.com/116161693/233050712-55d461fc-b8ba-47ff-ae6d-6586ed14d09d.png)

2. This playbook is divided into two parts, each of them is intended to perform the same task: install **wireshark utility** (or make sure it is updated to the latest version) on your RHEL 8 and Ubuntu servers. It uses **root** user to perform this task and respective package manager: **yum** for RHEL 8 and **apt** for Ubuntu.

#### Step 6 – Update GIT with the latest code
- Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.
- Commit your code into GitHub: use git commands to **add**, **commit** and **push** your branch to GitHub.
```
git status
git add <selected files>
git commit -m "commit message"
```
- Create a Pull request (PR)
- Once your code changes appear in master branch – Jenkins will do its job and save all the files (build artifacts) to **/var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/** directory on Jenkins-Ansible server.

![image](https://user-images.githubusercontent.com/116161693/233051903-94332f60-663d-4d99-b32a-2031eea856b6.png)

#### Step 7 – Run Ansible test
- Now, it is time to execute ansible-playbook command and verify if your playbook actually works: `cd ansible-config-mgt`
- Run ansible-playbook command: `ansible-playbook -i inventory/dev.yml playbooks/common.yml`
![image](https://user-images.githubusercontent.com/116161693/233052358-70deed5e-4f89-4571-bc10-6aaf6d8a143e.png)

- If your command ran successfully, go to each of the servers and check if wireshark has been installed by running `which wireshark` or `wireshark --version`
![image](https://user-images.githubusercontent.com/116161693/233052432-d142a979-6836-4a20-8992-7a22d7a8f89d.png)
