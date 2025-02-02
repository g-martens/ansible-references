You can create and edit two files in each of your ansible project directorys that configures the behavour of ansible and the ansible-navigator command. an ansible project directory is directory from which you runs your playbooks by using the ansible, ansible-playbook and ansible-navigator commands.

The file you can create or edit are:

1. ansible.cfg
2. ansible-navigator.cfg

Below you can find how an where you can create these files:

# Ansible.cfg
With this config file, you can manipulate how ansible works. you can create/edit this file in the following places:
1. ANSIBLE_CONFIG (environment variable if set)
2. ansible.cfg (in the current directory)
3. ~/.ansible.cfg (in home directory)
4. /etc/ansible/ansible.cfg

The precendence order is from top to bottom.

## Example of an ansible.cfg file

```INI
[defaults]
inventory = ./inventory
remote_user = ansible
ask_pass = false

[privilege_escelation]
become = true
become_method = sudo
become_user = root
become_ask_pass = false
```


>[!WARNING]
>Keep in mind how you connect to the remote machine
>1. does the user exist
>2. can you login with the user with ssh-keys, or with password
>3. does the user have the correct permissions
>4. can the user use sudo commands without giving the password


## Tips to create an config file
U can use the folling command to dump all the config settings that are available.

```bash
ansible-config list
```

and to generate an example config 
```bash
ansible-config init | grep  "^[^#]" > /tmp/ansible-config.cfg
```





# Ansible-navigator.cfg
With this config file, you can manipulate how ansible works. you can create/edit this file in the following places:
1. ANSIBLE_CONFIG (environment variable if set)
2. ansible-navigator.cfg (in the current directory)
3. ~/.ansible-navigator.cfg (in home directory)
4. /etc/ansible/ansible-navigator.cfg

The precendence order is from top to bottom.

## Example of an ansible.cfg file
```yaml
---
ansible-navigator:
  execution-environment:
    image: utility.lab.example.com/ee-supported-rhel8:latest
    pull: registry
      policy: missing

  playbook-artifact:
    enable: false
  mode: stdout
...
```
