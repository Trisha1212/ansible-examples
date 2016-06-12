## Inventory file  

###Inventory options/jargon

 - Behavioral Parameters
 - Groups
 - Groups of Groups
 - Assign Variables
 - Scaling out using multiple files
 - Static/Dynamic
 
## Example inventory file

```
[web]
web1.foo.com ansible_ssh_user=pradeep ansible_ssh_pass=blah
web2.foo.com ansible_python_interpreter=/usr/bin/python

[datacenter-east:children]
web
[datacenter-east:vars]
ansible_ssh_user=pradeep
ansible_ssh_pass = blah
ntp-server=1.2.3.4
```  

[web] is a group of two hosts and each host has its own parameters and values  
We can explicitly mention python interpreter on systems that have 3.x, since ansible is supported only on 2.4+ and <3.0 at this time  
datacenter-east:children tells ansible that this is groups of group  
The :vars will get applied to all groups inside the group datacenter-east (and each system in the group)
