```

 ____  _       ____  __ __  ____    ___    ___   __  _  _____
|    \| |     /    ||  |  ||    \  /   \  /   \ |  |/ ]/ ___/
|  o  ) |    |  o  ||  |  ||  o  )|     ||     ||  ' /(   \_ 
|   _/| |___ |     ||  ~  ||     ||  O  ||  O  ||    \ \__  |
|  |  |     ||  _  ||___, ||  O  ||     ||     ||     \/  \ |
|  |  |     ||  |  ||     ||     ||     ||     ||  .  |\    |
|__|  |_____||__|__||____/ |_____| \___/  \___/ |__|\_| \___|
                                                             


```
An Ansible playbook is a file containing a series of instructions (called "plays") that automate tasks on remote systems. It's written in YAML (Yet Another Markup Language), which makes it human-readable and easy to write. Playbooks are typically used to define the configuration, deployment, or orchestration of systems.

# Create an Playbook
An playbook can exist of or more plays

In each play you can set the following items
1. Name: (Description of what this play does)
2. Hosts: (again wich host, group, localhost, all do you want to run the play at)
3. become: (Do you want to elevate the permissions (become an different user))
4. vars: (which variable do you want to specify)
5. vars_files: (Which var file do you want to use)
6. tasks: ( which tasks do you want to execute)
7. Modules (a task exist of 1 or more modules)

When you create an playbook, always start de first line with the tree dashes (---) to show the beginning of the document. You can use the three dots (...) to show of the file. 

**Example of an playbook**
```YAML
---
- name: Install Apache and start the service
  hosts: webservers
  become: true
  tasks:
    - name: Install Apache
      ansible.builtin.yum:
        name: httpd
        state: present

    - name: Start Apache service
      ansible.builtin.service:
        name: httpd
        state: started


```
>[!WARNING]
> When you create an playbook, it is alway smart to use an Style Guide.
> For example: [Style Guide](https://github.com/g-martens/Ansible-Style-Guide)

## Find the available modules
By default you can use al the modules which are embedded with ansible.
These are the [ansible.builtin](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html#plugins-in-ansible-builtin) modules.

within ansible-navigator you can find which collections and modules are available
```bash
ansible-navigator collections list
```