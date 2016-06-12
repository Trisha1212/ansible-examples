## Ping  
First create a file inventory and type the ip addresses of machines that you want ansible to ping to (assuming one of the ip is 192.168.33.20)
```
ansible 192.168.33.20 -i inventory  -u vagrant -m ping -k
```  
ssh into the box once to create known_hosts key on control server  

Output should be similar to   

```
vagrant@acs:~/exercise1$ ansible 192.168.33.20 -i inventory  -u vagrant -m ping -k
SSH password:
192.168.33.20 | success >> {
    "changed": false,
    "ping": "pong"
}
```

For all hosts in inventory file  
```
ansible all -i inventory  -u vagrant -m ping -k
```  

## Debug mode
Append -vvv to the command (3 v's for deeper level debugging)  
```
ansible 192.168.33.20 -i inventory  -u vagrant -m ping -k -vvv
```  

 - You can see that first ansible compiles the module Ping
 - Next is makes the module executable
 - Finally it executes it 
 
## Command

```
ansible all -i inventory -u vagrant -m command -a "/usr/sbin/reboot"
ansible all -i inventory -u vagrant -m command -a "/usr/sbin/yum update -y"
ansible all -i inventory -u vagrant -m -a "/usr/sbin/yum update -y"
```  

By default command is assumed, so you can skip this  

There is Shell module too. Command module runs in python, whereas shell module creates a shell (shell variables are accessible)  

### Known Issues

- Install sshpass on control server if you get error around sshpass not installed