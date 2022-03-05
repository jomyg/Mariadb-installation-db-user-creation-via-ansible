# Mariadb-installation-db-user-creation-via-ansible

Here's a simple Ansible Playbook to create a basic MariaDB deployment on a remote amazon linux server.

Playbooks are one of the core features of Ansible and tell Ansible what to execute. They are like a to-do list for Ansible that contains a list of tasks.
Playbooks contain the steps which the user wants to execute on a particular machine.

## Pre-requisites:-

#### 1. Master server: 
The machine where Ansible is installed and from where all tasks and playbooks will be run. Here we use an amazon linux server.
```html
  #amazon-linux-extras install ansible2 -y
```
```html
  # ansible --version
 ansible 2.9.23
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/root/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.18 (default, Jun 10 2021, 00:11:02) [GCC 7.3.1 20180712 (Red Hat 7.3.1-13)]
```

#### 2. Project directory: 
This will become the working directory where we execute the ansible-playbook command and create all files for the project.
```html
   # mkdir mariadb-project
   # cd mariadb-project/
```

#### 3. Inventory: 
File containing data about the ansible client servers (Host file / Inventory file). Here we use a single client which is also an amazon linux server.

#### 4. Variable file: 
Variables can be included in the playbook via include files. Here we encrypt the variable file using Ansible-vault
```html
   # ansible-vault encrypt mariadb.vars
   New Vault password:
   Confirm New Vault password:
   Encryption successful
```

#### 5. Playbook: 
Playbooks contain the steps which the user wants to execute on a particular machine.

* Ansible modules used:-
  - yum
  - service
  - mysql_user
  - mysql_db

These are the files inside our project directory.
```html
├── hosts        ------------>  Inventory
├── mariadb.vars ------------>  Variable file
└── mariadb.yml  ------------>  Playbook
```

## Syntax Check

An Ansible Playbook is formatted with YAML, Yet Another Markup Language. This means that the whitespace is significant and items at the same configuration level should be spaced the same as other sibling elements. To check the syntax before deploying the code:-
```html
# ansible-playbook -i hosts --ask-vault-pass  mariadb.yml --syntax-check
Vault password:

playbook: mariadb.yml
```
**NOTE:** Pass the Ansible-vault password since the mariadb.vars file is encrypted.

## Playbook Execution
```html
# ansible-playbook -i hosts --ask-vault-pass  mariadb.yml
Vault password:

PLAY [Install and configure mariadb-server] ***********************************************************************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************************************************************************************************
ok: [172.31.14.171]

TASK [Mariadb - installation] *************************************************************************************************************************************************************************************
changed: [172.31.14.171]

TASK [Mariadb - Restart/Enable] ***********************************************************************************************************************************************************************************
changed: [172.31.14.171]

TASK [Mariadb - setting root password] ****************************************************************************************************************************************************************************
changed: [172.31.14.171]

TASK [Mariadb - removing anonymous user] **************************************************************************************************************************************************************************
changed: [172.31.14.171]

TASK [Mariadb - removing test database] ***************************************************************************************************************************************************************************
changed: [172.31.14.171]

TASK [Mariadb - adding database wp_db] ****************************************************************************************************************************************************************************
changed: [172.31.14.171]

TASK [Mariadb - adding database user wp_user] *********************************************************************************************************************************************************************
changed: [172.31.14.171]

PLAY RECAP ********************************************************************************************************************************************************************************************************
172.31.14.171              : ok=8    changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
playbook: mariadb.yml
```
