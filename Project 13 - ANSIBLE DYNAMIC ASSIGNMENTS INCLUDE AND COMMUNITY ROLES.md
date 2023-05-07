# ANSIBLE DYNAMIC ASSIGNMENTS INCLUDE AND COMMUNITY ROLES

### Ansible Dynamic Assignments (Include) and Community Roles

In this project, the UAT servers will be configured to learn about and practice using new Ansible concepts and modules.
This project will introduce dynamic assignments by using the include module.
The static assignments from Project 12 use the import Ansible module. The module that enables dynamic assignments is include.

```
import = Static
include = Dynamic
```

When the import module is used, all statements are pre-processed at the time playbooks are parsed. When site.yml playbook is executed, Ansible will process all the playbooks referenced during the time it is parsing the statements. This also means that, during actual execution, if any statement changes, such statements will not be considered. Hence, it is static.

On the other hand, when include module is used, all statements are processed only during execution of the playbook. After the statements are parsed, any changes to the statements encountered during execution will be used.

Note: For most projects, static assignments are preferred as they are more reliable. Dynamic assignments can be challenging in the debugging process. However, they can be useful for environment specific variables as in this project.

#### Introducing Dynamic Assignment Into the structure

In the `https://github.com/<your-name>/ansible-config-mgt GitHub repository `, start a new branch and call it dynamic-assignments.

Create a new folder, name it dynamic-assignments. Then inside this folder, create a new file and name it env-vars.yml. site.yml will be instructed to include this playbook later.

GitHub will have the following structure at this point:

```
├── dynamic-assignments
│   └── env-vars.yml
├── inventory
│   └── dev
    └── stage
    └── uat
    └── prod
└── playbooks
    └── site.yml
└── roles (optional folder)
    └──...(optional subfolders & files)
└── static-assignments
    └── common.yml
```

Since Ansible will be used to configure multiple environments and each of these environments will have certain unique attributes, such as servername, ip-address etc., there has to be a way to set values to variables per specific environment.

Create a folder to keep each environment’s variables file and name it as env-vars. Then for each environment, create new YAML files which will be used to set variables.

The layout should now look like this:

```
├── dynamic-assignments
│   └── env-vars.yml
├── env-vars
    └── dev.yml
    └── stage.yml
    └── uat.yml
    └── prod.yml
├── inventory
    └── dev
    └── stage
    └── uat
    └── prod
├── playbooks
    └── site.yml
└── static-assignments
    └── common.yml
    └── webservers.yml
```

Paste the instruction below into the env-vars.yml file:


```
---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - prod.yml
            - stage.yml
            - uat.yml
          paths:
            - "{{ playbook_dir }}/env-vars"
      tags:
        - always
 ```

![image](https://user-images.githubusercontent.com/116161693/236705724-e575b246-f92b-4821-9c88-42696f8ea9eb.png)

Three things to note from the above code;

The 'include*vars' syntax has been used instead of include. This is because Ansible developers decided to separate different features of the module. From Ansible version 2.8, the include module is deprecated and variants of include*\* must be used. These are:

include_role
include_tasks
include_vars

In the same version, variants of import were also introduced, such as:

import_role
import_tasks

Special variables {{ playbook_dir }} and {{ inventory_file }} are used in the above code. {{ playbook_dir }} will help Ansible to determine the location of the running playbook, and from there navigate to other path on the filesystem. {{ inventory_file }} on the other hand will dynamically resolve to the name of the inventory file being used, then append .yml, so that it picks up the required file within the env-vars folder.

The variables are included using a loop. with_first_found implies that, looping through the list of files, the first one found is used. This is good practice so that the default values can be set in case an environment specific env file does not exist.

#### Update site.yml with dynamic assignments

Update site.yml file to make use of the dynamic assignment. (At this point, testing cannot be done yet. This is just setting the stage for what is to come).

site.yml should now look like this:

```
---
- name: Include dynamic variables
  hosts: all
  tasks:
    - import_playbook: ../static-assignments/common.yml
    - include_playbook: ../dynamic-assignments/env-vars.yml
  tags:
    - always

- name: Webserver assignment
  hosts: webservers
    - import_playbook: ../static-assignments/webservers.yml
```
![image](https://user-images.githubusercontent.com/116161693/236705758-f5dd637b-9c4e-4907-972b-9c21bd8162d0.png)

#### Community Roles

It is time to create a role for MySQL database - it should install the MySQL package, create a database and configure users. There are tons of roles that have already been developed by other open source engineers. These roles are actually production ready, and dynamic to accomodate most of Linux flavours. With Ansible Galaxy again, we can simply download a ready to use ansible role.

#### Download Mysql Ansible Role

Browse the available community roles [here](https://galaxy.ansible.com/home)

This project will make use of a MySQL role developed by [geerlingguy](https://galaxy.ansible.com/geerlingguy/mysql).

On Jenkins-Ansible server make sure that git is installed with git --version, then go to ‘ansible-config-mgt’ directory and run:

```
git init
git pull https://github.com/<your-name>/ansible-config-mgt.git
git remote add origin https://github.com/<your-name>/ansible-config-mgt.git
git branch roles-feature
git switch roles-feature
```

Inside roles directory, create a new MySQL role with ansible-galaxy install geerlingguy.mysql and rename the folder to mysql:

```
ansible-galaxy install geerlingguy.mysql
mv geerlingguy.mysql/ mysql
```

Read README.md file, and edit roles configuration to use correct credentials for MySQL required for the tooling website. The main.yml file in the defaults folder of roles directory is edited as follows:

Now it is time to upload the changes into GitHub:

```
git add .
git commit -m "Commit new role files into GitHub"
git push --set-upstream origin roles-feature
```

Create a Pull Request and merge it to main branch on GitHub.

#### Load Balancer roles

In order to be able to choose which Load Balancer to use, Nginx or Apache, we will need to have two roles:

1. Nginx
2. Apache

- With the experience gained from the projects on Ansible so far, it is understood that roles could either be developed or recreated easily from available roles in the community.

- After creating the roles, update both static-assignment and site.yml files to refer the roles.

Important Hints:

- Since both Nginx and Apache load balancer cannot be used together, add a condition to enable either one - this is where the use of variables is handy.
- Declare a variable in defaults/main.yml file inside the Nginx and Apache roles. Name each variables enable_nginx_lb and enable_apache_lb respectively.
- Set both values to false like this enable_nginx_lb: false and enable_apache_lb: false.
- Declare another variable in both roles load_balancer_is_required and set its value to false as well
- Update both assignment and site.yml files respectively

On main.yml in Nginx:

On main.yml in Apache:

Create loadbalancers.yml file in static-assignments folder and copy the following code:

```
- hosts: lb
  roles:
    - { role: nginx, when: enable_nginx_lb and load_balancer_is_required }
    - { role: apache, when: enable_apache_lb and load_balancer_is_required }
```
![image](https://user-images.githubusercontent.com/116161693/236706002-96080d67-6666-449c-8e41-f2767eb04f8e.png)

Update site.yml file with the following code:


- name: Loadbalancers assignment
       hosts: lb
         - import_playbook: ../static-assignments/loadbalancers.yml
        when: load_balancer_is_required


Now the env-vars\uat.yml file can be used to define which loadbalancer to use in UAT environment by setting respective environmental variable to true.

- Activate load balancer, and enable nginx by setting these in the respective environment’s env-vars file.


- The same must work with apache LB, so it can be switched by setting respective environmental variable to true and other to false.

- To test this, update inventory for each environment and run Ansible against each environment.

![image](https://user-images.githubusercontent.com/116161693/236705501-2e40c4af-3f01-4c4f-a9fc-3d974ea84eda.png)

![image](https://user-images.githubusercontent.com/116161693/236705505-539da967-b5cd-4835-a3ac-d90e91d835a9.png)

![image](https://user-images.githubusercontent.com/116161693/236705509-67c8a5e4-8e4d-4807-abbe-2ca689c25531.png)

