```


    __   ___   ___ ___  ___ ___   ____  ____   ___   _____
   /  ] /   \ |   |   ||   |   | /    ||    \ |   \ / ___/
  /  / |     || _   _ || _   _ ||  o  ||  _  ||    (   \_ 
 /  /  |  O  ||  \_/  ||  \_/  ||     ||  |  ||  D  \__  |
/   \_ |     ||   |   ||   |   ||  _  ||  |  ||     /  \ |
\     ||     ||   |   ||   |   ||  |  ||  |  ||     \    |
 \____| \___/ |___|___||___|___||__|__||__|__||_____|\___|
                                                          


```
You can always find more info on the ansible docs [here](https://docs.ansible.com/ansible/latest/command_guide/index.html)

# Ansible
You can use this command to run ad-hoc commands against nodes.


```bash
# Collect ansible_facts from localhost
[user@server]$ ansible localhost -m setup

# Collect facts from remote server
[user@server]$ ansible server -i inventoryfile -m setup
```

or for example to make store that httpd is stopped
```bash
[user@server]$ ansible webservers -m ansible.builtin.service -a "name=httpd state=stopped"
```

# Ansible-Playbook
With this tool you can execute ansible-playbook against inventorys.
Below you can find an example:
```bash
[user@server]$ ansible-playbook -i inventory/production site.yml --ask-vault-pass --tags install,configure
```


# Ansible-lint
With this tool you can verify you code against the best practises of ansible.
```bash
[user@server]$ ansible-lint site.yml
```

# Ansible-galaxy
With ansible-galaxy you can do a lot of stuff. Below you can find te most usefull examples

## Generate a role
```bash
[user@server]$ ansible-galaxy init <rolname>
```

## Install items from requirments.yml
```bash
[user@server]$ ansible-galaxy install -r roles/requirements.yml
```

## Install collection from galaxy
With the example below, you can install collections to the path you specify
```bash
[user@server]$ ansible-galaxy collection install ansible.posix -p ./collections
```

of from tarball
```bash
[user@server]$ ansible-galaxy collection install my_namespace-my_collection-1.0.0.tar.gz -p ./collections
```
## Show installed collections
```bash
[user@server]$ ansible-galaxy collection list
```
if needed, you can specify a path with -p


# Ansible-navigator
You can find the origional docs [here](https://ansible.readthedocs.io/projects/navigator/)
when you use the ansible-navigator, you will run the playbooks through an Ansible Execution Environment (EE) container. To use this you need to have docker or podman installed.

Most of the options are relatable to different ansible commands.
With the ansible-navigator you have subcommands.
1. images
2. collections
3. config (ansible-config)
4. doc (ansible-doc)
5. exec (ansible)
6. inventory (ansible-inventory)
7. lint (ansible-lint)
8. run (ansible-playbook)

## Images
```bash
[user@server]$ ansible-navigator images
```

## Collections
```bash
[user@server]$ ansible-navigator collections
```
## Config
```bash
[user@server]$ ansible-navigator config
```

## Doc
```bash
[user@server]$ ansible-navigator doc
```

## Exec
```bash
[user@server]$ ansible-navigator exec -- <command>
```
## inventory
```bash
[user@server]$ ansible-navigator inventory
```
## Lint
```bash
[user@server]$ ansible-navigator lint
```

## Run
```bash
[user@server]$ ansible-navigator run
```
