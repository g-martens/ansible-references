```

  ____  ____   _____ ____  ____   _        ___         __   ___   ____   _____  ____   ____ 
 /    ||    \ / ___/|    ||    \ | |      /  _]       /  ] /   \ |    \ |     ||    | /    |
|  o  ||  _  (   \_  |  | |  o  )| |     /  [_       /  / |     ||  _  ||   __| |  | |   __|
|     ||  |  |\__  | |  | |     || |___ |    _]     /  /  |  O  ||  |  ||  |_   |  | |  |  |
|  _  ||  |  |/  \ | |  | |  O  ||     ||   [_     /   \_ |     ||  |  ||   _]  |  | |  |_ |
|  |  ||  |  |\    | |  | |     ||     ||     |    \     ||     ||  |  ||  |    |  | |     |
|__|__||__|__| \___||____||_____||_____||_____|     \____| \___/ |__|__||__|   |____||___,_|
                                                                                            


```

You can create and edit two files in each of your ansible project directory's that configures the behavior of ansible and the ansible-navigator command. an ansible project directory is directory from which you runs your playbooks by using the ansible, ansible-playbook and ansible-navigator commands.

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

The precedence order is from top to bottom.

## Example of an ansible.cfg file

```INI
[defaults]
inventory = ./inventory
remote_user = ansible
ask_pass = false
log_path = ~/ansible-playbook.log
roles_path = ~/ansible/roles
collections_path= ~/ansible/collections

[privilege_escalation]
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
U can use the following command to dump all the config settings that are available.

```bash
ansible-config list
```

and to generate an example config 
```bash
ansible-config init | grep  "^[^#]" > /tmp/ansible-config.cfg
```





# Ansible-navigator.yml
With this config file, you can manipulate how ansible works. you can create/edit this file in the following places:
1. ANSIBLE_CONFIG (environment variable if set)
2. ansible-navigator.yml (in the current directory)
3. ~/.ansible-navigator.yml (in home directory)
4. /etc/ansible/ansible-navigator.yml

The precedence order is from top to bottom.

## Example of an ansible.yml file
```yaml
---
ansible-navigator:
  ansible:
    config:
      path: ~/.ansible.cfg
  execution-environment:
    image: utility.lab.example.com/ee-supported-rhel8:latest
    pull:
      policy: missing

  playbook-artifact:
    enable: false
  mode: stdout
...
```

To generate an example config, you can execute the following command
```bash
ansible-navigator settings --gs > /where/to/save.yml
```