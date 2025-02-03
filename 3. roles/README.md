```

 ____   ___   _        ___  _____
|    \ /   \ | |      /  _]/ ___/
|  D  )     || |     /  [_(   \_ 
|    /|  O  || |___ |    _]\__  |
|    \|     ||     ||   [_ /  \ |
|  .  \     ||     ||     |\    |
|__|\_|\___/ |_____||_____| \___|
                                 


```
An Ansible role is a way to organize and reuse Ansible code. Roles allow you to break down your playbooks into smaller, reusable components. Each role can contain tasks, variables, files, templates, and handlers that are related to a specific functionality or configuration.

Here are the main components of an Ansible role:

1. **Tasks**: The main list of tasks to be executed by the role.
2. **Handlers**: Handlers that can be triggered by tasks.
3. **Files**: Files that can be deployed by the role.
4. **Templates**: Jinja2 templates that can be deployed by the role.
5. **Vars**: Variables for the role.
6. **Defaults**: Default variables for the role.
7. **Meta**: Metadata about the role, including dependencies.
8. **Tests**: Test playbooks to verify the role's functionality.

A typical directory structure for an Ansible role looks like this:

```plaintext
roles/
  my_role/
    tasks/
      main.yml
    handlers/
      main.yml
    files/
    templates/
    vars/
      main.yml
    defaults/
      main.yml
    meta/
      main.yml
    tests/
      test.yml
```
This structure helps in maintaining clean and modular code, making it easier to manage complex configurations.

An example of how you can use the role in a play:
```yaml
- name: Apply my_role to RedHat systems
    hosts: all
    become: yes
    vars:
        ansible_os_family: RedHat
    roles:
        - role: my_role
            when: ansible_os_family == "RedHat"
```
