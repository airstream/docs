# Ansible docs

## Ansible install (CentOS7)
```markdown
$ yum list epel-release
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
echo 'ansible-master ansible_ssh_host=127.0.0.1' >> /etc/ansible/hosts
```

### Ansible remote host configuration
```markdown
echo 'ansible-host ansible_ssh_host=192.168.0.20' >> /etc/ansible/hosts
```
