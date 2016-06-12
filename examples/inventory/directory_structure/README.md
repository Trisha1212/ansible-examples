## Create a user
```
ansible webservers -i inventory_prod -m user -a "name={{username}} password=12345 --sudo
```  

Now create a file webservers inside /production/group_vars and inside create username: group_user  

```
vagrant@acs:~/directory_structure/production/group_vars$ ansible webservers -i ../inventory_prod -m user -a "name={{username}} password=12345" --sudo
```  

This will now create a user group_user instead of all_username , which is in "all" file in group_vars folder  

## Order of precedence
The order of evaluation of variables is all, group and system. Hence if a variable is defined in all 3 , the one in system takes precedence  
In the above example the variable defined in webservers file in group_vars took precedence over the same variable in all file  

### Create host_var
Now lets create a web1 yaml file in host_vars folder and put the key username: web1_user. We expect this to take precedence  

```
vagrant@acs:~/directory_structure/production/host_vars$ ansible webservers -i ../inventory_prod -m user -a "name={{username}} password=12345" --sudo
web1 | success >> {
    "changed": true,
    "comment": "",
    "createhome": true,
    "group": 503,
    "home": "/home/web1_user",
    "name": "web1_user",
    "password": "NOT_LOGGING_PASSWORD",
    "shell": "/bin/bash",
    "state": "present",
    "system": false,
    "uid": 503
}
```