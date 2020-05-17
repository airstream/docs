# Ansible docs
## About 
**Ansible** is the simplest way to automate apps and IT infrastructure. Application Deployment + Configuration Management + Continuous Delivery.

## Ansible install (CentOS7)
```markdown
$ yum list epel-release
$ yum update
$ yum install ansible
```

## Ansible file locations
```markdown
# /etc/ansible/hosts 		- default ansible 'hosts' file
# /etc/ansible/ansible.cfg 	- config file for ansible
```

## Configuration
### Ansible remote host (not master) ### 
```markdown
$ useradd ansible
$ echo 'ansible	ALL=(ALL)	NOPASSWD: ALL' >> /etc/sudoers
```

### Ansible master ###
```markdown
$ useradd ansible
$ su - ansible
$ ssh-keygen
$ cd /home/ansible/.ssh
$ ssh-copy-id 192.168.0.20
```

### Ansible master local host configuration
```markdown
$ echo 'ansible-master ansible_ssh_host=127.0.0.1' >> /etc/ansible/hosts
```

### Ansible remote host configuration
```markdown
$ echo 'ansible-host ansible_ssh_host=192.168.0.20' >> /etc/ansible/hosts
```

### Ansible version
```markdown
$ ansible --version
ansible 2.8.4
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/ansible/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/site-packages/ansible
  executable location = /bin/ansible
  python version = 2.7.5 (default, Aug  7 2019, 00:51:29) [GCC 4.8.5 20150623 (Red Hat 4.8.5-39)]
```

### Documentation
```markdown
$ ansible-doc --help
```

### List of available plugins

```markdown
$ ansible-doc --list
$ ansible-doc yum
```
---
## Ad-hoc
### Modules
Ansible **modules** are reusable, standalone scripts that can be used by the Ansible API, or by the ansible or ansible-playbook programs. They return information to ansible by printing a JSON string to stdout before exiting.

```markdown
$ ansible <ansible_host> -m <module_name>
```

```markdown
# setup module for localhost ansible
$ ansible localhost -m setup		# ansible facts

# ping module for localhost ansible
$ ansible localhost -m ping		# ping localhost

# service module for localhost ansible
$ ansible localhost -m service -a "name=httpd state=started"

# yum module for localhost ansible
$ ansible localhost -m yum -a "name=httpd state=present"		# install httpd package
$ ansible localhost -m yum -a "name=nano state=present" 		# install nano package
$ ansible localhost -m yum -a "name=nano state=absent"  		# remove  nano package
$ ansible localhost -b -m yum -a "name=nano state=absent"	        # remove  nano package (-b, --become run operations with become (does not imply password prompting)
```

## Playbooks
**Playbooks** are the files where Ansible code is written. Playbooks are written in YAML format. YAML stands for Yet Another Markup Language. **Playbooks** are one of the core features of Ansible and tell Ansible what to execute. They are like a to-do list for Ansible that contains a list of tasks.

**Playbooks** contain the steps which the user wants to execute on a particular machine. Playbooks are run sequentially. Playbooks are the building blocks for all the use cases of Ansible.

```markdown
$ touch /home/ansible/inv
[servers]
ansible-host ansible_host=192.168.0.20
```

```markdown
$ touch /home/ansible/web.yml
--- # Bootstrap Server
- hosts: servers
  become: yes
  tasks:
  - name: install httpd
    yum:
      name: httpd
      state: latest

  - name: create index.html file
    file:
      name: /var/www/html/index.html
      state: touch
  - name: add web content
    lineinfile:
      line: "Cool bananas!"
      path: /var/www/html/index.html
  - name: start httpd
    service:
      name: httpd
      state: started
```

### Playbook dry dun (does not affect systems)
```markdown
$ ansible-playbook web.yml --check
$ ansible-playbook -i inv web.yml --check
```

### Playbook run
```markdown
$ ansible-playbook -i inv web.yml
PLAY [servers] **********************************************************************************

TASK [Gathering Facts] **************************************************************************
ok: [ansible-host]

TASK [install httpd] ******************************************************************************
changed: [ansible-host]

TASK [create index.html file] *******************************************************************
changed: [ansible-host]

TASK [add web content] **************************************************************************
changed: [ansible-host]

TASK [start httpd] ******************************************************************************
changed: [ansible-host]

PLAY RECAP **************************************************************************************
ansible-host               : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## Handlers in Ansible
**Handlers** - running operations on change.

```markdown
$ touch /home/ansible/inv
[servers]
ansible-host ansible_host=192.168.0.20
```

```markdown
$ touch /home/ansible/web-handler.yml
--- # Bootstrap Server
- hosts: servers
  become: yes
  tasks:
  - name: install httpd
    yum:
      name: httpd
      state: latest
    notify:
      - restart httpd
  - name: create index.html file
    file:
      name: /var/www/html/index.html
      state: touch
  - name: add web content
    lineinfile:
      line: "Cool bananas!"
      path: /var/www/html/index.html
    notify:
      - restart httpd
  - name: start httpd
    service:
      name: httpd
      state: started
# Handler
  handlers:
  - name: "Attempt to restart httpd"
    service:
      name: httpd
      state: restarted
    listen: "restart httpd"
```

## Ansible Galaxy
Initialize a new galaxy role
```markdown
$ ansible-galaxy init <role_name>
```
Search galaxy roles
```markdown
$ ansible-galaxy search <role_name>
```
