## Ansible Config
Ansible looks for the global configuration as per the following precedence  

 - $ANSIBLE_CONFIG
 - ./ansible.cfg
 - ~/.ansible.cfg
 - /etc/ansible/ansible.cfg
 
Configuration files are NOT merged. The first file found will be evaluated.  
That said, we can override settings using <ANSIBLE_configsetting>. For example "export ANSIBLE_FORKS=10"  

## Some variables
 - Default forks is 5 , however production setting can be 20 etc. This is number of parallel ansible executions  
 - host_key_checking, highly recommended for production i.e. true. For development set false
 - log_path, Default set to null OR set path to log file. Users executing ansible should have write 
 

## Ansible docs
[Here](http://docs.ansible.com/ansible/intro_configuration.html)