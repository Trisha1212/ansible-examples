## Playbooks examples  

### Concepts

- Plays map hosts to tasks
- A play can have multiple tasks
- A playbook can have multiple plays
- whitespaces ! Its python of course (whitespace after - , whitespace after :)
- Skip facts in plays gathering unless needed (gather_facts: no). It takes at least 10 secs   

### Exercise

- Define an ansible.cfg in the playbooks directory
- Specify the inventory file in defaults section of ansible.cfg
- Write playbook
- Execute playbook
- Retry failed plays  

### Define ansible.cfg

```
[defaults]
hostfile = inventory
```  

### Inventory
```
#web1 ansible_ssh_host=192.168.33.20 ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
#db1 ansible_ssh_host=192.168.33.30 ansible_ssh_user=vagrant ansible_ssh_pass=vagrant

acs ansible_ssh_host=192.168.33.10 ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
web1 ansible_ssh_host=192.168.33.20 ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
db1 ansible_ssh_host=192.168.33.30

[webservers]
web1

[dbservers]
db1

[datacenter:children]
webservers
dbservers

[datacenter:vars]
ansible_ssh_user=vagrant
ansible_ssh_pass=vagrant
```  

### Play for web servers
Install apache webserver (httpd) and start the service  

```
- hosts: webservers
  sudo: yes

  tasks:
  - name: Ensure that Apache is installed
    yum: name=httpd state=present

  - name: Start Apache Services
    service: name=httpd enabled=yes state=started
```   

### Play for db servers
Install mysql and start the service  

```
- hosts: dbservers
  sudo: yes

  tasks:
  - name: Ensure MySQL is installed
    yum: name=mysql-server state=present
  - name: Start MySQL
    service: name=mysqld state=started
```   

### Play for stopping firewall
Stop iptables service  on webservers and dbservers (firewall blocks port 80 by default)

```
- hosts: webservers:dbservers
  sudo: yes

  tasks:
  - name: Stop IPTABLES NOW!
    service: name=iptables state=stopped
```  

### Full playbook

web_db.yaml  

```
---
- hosts: webservers
  sudo: yes

  tasks:
  - name: Ensure that Apache is installed
    yum: name=httpd state=present

  - name: Start Apache Services
    service: name=httpd enabled=yes state=started

- hosts: dbservers
  sudo: yes

  tasks:
  - name: Ensure MySQL is installed
    yum: name=mysql-server state=present
  - name: Start MySQL
    service: name=mysqld state=started

- hosts: webservers:dbservers
  sudo: yes

  tasks:
  - name: Stop IPTABLES NOW!
    service: name=iptables state=stopped
vagrant@acs:~/playbooks_examples$ ansible-playbook web_db.yaml
vagrant@acs:~/playbooks_examples$ cat web_db.yaml
---
- hosts: webservers
  sudo: yes

  tasks:
  - name: Ensure that Apache is installed
    yum: name=httpd state=present

  - name: Start Apache Services
    service: name=httpd enabled=yes state=started

- hosts: dbservers
  sudo: yes

  tasks:
  - name: Ensure MySQL is installed
    yum: name=mysql-server state=present
  - name: Start MySQL
    service: name=mysqld state=started

- hosts: webservers:dbservers
  sudo: yes

  tasks:
  - name: Stop IPTABLES NOW!
    service: name=iptables state=stopped
```
 

### Execute playbook

`ansible-playbook web_db.yaml`

### Retry play

- Comment :vars in inventory file so that db1 cannot authenticate
- Execute the playbook (Should fail and log the failed host in retry)
- Uncomment :vars in inventory file and run the playbook with --limit option specifying the retry file  

```
ansible-playbook web_db.yaml --limit @/home/vagrant/web_db.yaml.retry
```  





