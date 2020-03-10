# Jenkins setup using Ansible
This project is setting up complete Jenkins using ansible, along with the creation of the different types of users (admin, writer, and reader)  

Note: This is specific to Ubuntu server.

## Requirements
To start with this setup you need:
* Ansible
* git
* Passwords for admin, writer and reader users handy

## Beginning
The commands below set everything up to run the examples:
```
$ git clone https://github.com/nsayed123/jenkins_setup.git
$ cd jenkins_setup
```
```
.
├── README.md
├── roles
│   └── jenkins
│       ├── defaults
│       │   └── main.yml
│       ├── files
│       │   ├── csrf.groovy
│       │   ├── disable-csrf-protection.groovy
│       │   └── enable-csrf-protection.groovy
│       ├── handlers
│       │   └── main.yml
│       ├── meta
│       │   └── main.yml
│       ├── README.md
│       ├── tasks
│       │   ├── jenkins_configs.yml
│       │   ├── jenkins_install.yml
│       │   ├── jenkins.yml
│       │   └── main.yml
│       ├── templates
│       │   └── create_user.groovy
│       ├── tests
│       │   ├── inventory
│       │   └── test.yml
│       └── vars
│           └── main.yml
└── site.yml
```
Jenkins role was created by issuing a command
```
ansible-galaxy init jenkins
```
### Roles expect files to be in certain directory names. Each directory must contain a main.yml file. Below is a description of each directory.
* tasks - contains the main list of tasks to be executed by the role.

* handlers - contains handlers, which may be used by this role or even anywhere outside this role.

* defaults - default variables for the role.

* vars - other variables for the role.

* files - contains files which can be deployed via this role.

* templates - contains templates which can be deployed via this role.

* meta - defines some meta data for this role.

I have included the jenkins_install.yml and jenkins_configs.yml file in the jenkins/tasks/main.yml file.

- Files directory contain files which is used to disable the csrf, which will bypass the GUI.
- Task directory contain files to install and configure Jenkins.
- Templates directory contain the create_user.groovy, which is invoked to create users and assign respective permissions.
- Test directory contain inventory file, where we define our target host where we want to install and configure Jenkins.
- vars directory contain the variable, which are invoked in jenkins_install.yml and jenkins_configs.yml

## Run

Make sure you are in the right directory "jenkins_setup"
# 
`
Note: replace "<admin_password>" with admin password, <reader_password> with reader password and <writer_password>" with writer password in the below command before issuing it.
To find the Jenkins plugins - https://plugins.jenkins.io/
`
# 
This will take 10 mins to setup. It depends on number of Jenkins plugins which is defined in **vars/main.yml** file.
Put the host name inside roles/jenkins/test/inventory file.
Change the hosts entry inside **site.yml** file.


Issue the below command
```
ansible-playbook site.yml --extra-vars "admin_user_pass=<admin_password> reader_user_pass=<reader_password> writer_user_pass=<writer_password>"

If you want to run in verbose mode

ansible-playbook site.yml --extra-vars "admin_user_pass=<admin_password> reader_user_pass=<reader_password> writer_user_pass=<writer_password>" -vvvv
```
