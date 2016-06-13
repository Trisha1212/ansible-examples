## Advanced playbook operations

### Include Files to Extend Playbook  

- Breakup long playbooks
- Use to add external variable files
- Reuse other playbooks

```
tasks:
   - include: wordpress.yml
     vars: 
	   sitename: some website
   - include: loadbalancer.yml
   - include_vars: variables.yml
```  

### Register Task Output

- Useful to use tasks to feed data into other tasks
- Useful to create custom error trapping

```
tasks:
   - shell: /user/bin/whoami
     register: username
   - file: path=/home/myfile.txt
           owner= {{username}}
```  

### Debug Module

- Useful to send output to screen during execution
- Helps find problems

```
tasks:
   - debug: msg="This host is {{ inventory_hostname}} during execution"
   - shell: /user/bin/whoami
     register: username
   - debug: var=username
```  

### Prompting for input

- Creates Dynamic playbooks  

```
- hosts: web1

  vars_prompt:
   - name: "sitename"
     prompt: "What is your site name?"
	 
  tasks:
   - debug: msg="The name is {{ sitename }}"
```

### Playbook Handlers

- Tasks with asynchronous execution
- Only run tasks when notified
- Tasks only notify when state=changed
- Does not run until all playbook tasks have executed
- Most common for restarting services to load changes (if changes are made)

- Copies config file to host
- If state=change on "COPY", tell "Apache Restart"
- Run "Service" module  

```
tasks:
   - copy: src=files/httpd.conf
           dest=/etc/httpd/conf
	 notify:
	     - Apache Restart
handlers:
   - name: Apache Restart
     service: name=httpd state=restarted

```  

### Conditional Execution

- Use the clause "when" to choose if task should run  

```
tasks:
  - yum : name=httpd state=present
    when: ansible_os_family == "RedHat"
   
  - apt: name=apache2 state=present
    when: ansible_os_family == "Debian"
```  

### Conditional Clause based on output

- Track whether previous tasks ran
- Searches JSON result for status
- Status Options:
  - success
  - failed
  - skipped

```
tasks:
  - command: ls /path/doesnt/exist
    register: result
	ignore_errors: yes
  - debug: msg="Failure!"
    when: result|failed
```   

## Jinja2 templates to inject values of parameters at execution time

```
tasks:
  -template:
    src=templates/httpd.j2
	dest=/etc/httpd/conf/httpd.conf
	owner=httpd
```   

```
...
<VirtualHost*:80>
ServerAdmin {{ server_admin }}
DocumentRot {{ site_root }}
ServerName {{ inventory_hostname }}
</VirtualHost>
```