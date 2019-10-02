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

### check ansible version
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

### List available plugins
```markdown
$ ansible-doc --list
$ ansible-doc yum
```
