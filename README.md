## Ansible Examples

` vagrant up`  
This should start three vm's with hostnames acs, web and db (Open the vagrant file to see detailed configuration for the VMs)  

`vagrant status`  
This should list the vm's upped by the Vagrantfile in current directory.  

`vagrant ssh name`  
name is one of the list of names returned from vagrant status command  


## Install ansible  

### ubuntu/trusty64
```
sudo apt-get update
sudo apt-get install ansible
```  

### CentOS
```
sudo yum install epel-release
sudo yum install ansible
```  

### From Source
```
sudo yum install gcc
sudo yum install python-setuptools
sudo easy_install pip
sudo yum install python-devel
sudo pip install ansible
```  

If you get errors around python 2.6 not supported, then upgrade to python 2.7 using [instructions](http://tuxlabs.com/?p=194).   
Alias python (default in /usr/bin/python pointing to 2.6.6) to point to 2.7 in /usr/lobal/bin/python2.7 in ~/.bashrc  

```
alias python=/usr/local/bin/python2.7
```


## Known Issues  

### Invalid nic  

If you encounter error during "vagrant up" with Virtualbox 5.18+ and Vagrant 1.8.1+ with error message "Invalid nic number..."   
Try modifying base.rb at the location "C:\HashiCorp\Vagrant\embedded\gems\gems\vagrant-1.8.3\plugins\providers\virtualbox\driver\base.rb" from 36 to 8   

```
# Returns the maximum number of network adapters.
        def max_network_adapters
          8
        end
```  

The full issue is described [here](https://github.com/mitchellh/vagrant/issues/7417)  

### vagrant ssh  

If vagrant can't find ssh on windows, then install openSSH on windows from [here](https://sourceforge.net/projects/sshwindows/)  
If passphrase is asked when ssh'ng, just hit enter. The credentials are vagrant/vagrant for ubuntu/trusty64 image