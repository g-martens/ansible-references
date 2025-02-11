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

# Role Structure
Here are the main components of an Ansible role:

1. **Tasks**: The main.yml file in this directory contains the role's task definitions.
2. **Handlers**: The main.yml file in this directory contains the role's handler definitions.
3. **Files**: This directory contains static files that are referenced by role tasks.
4. **Templates**: This directory contains Jinja2 templates that are referenced by role tasks.
5. **Vars**: The main.yml file in this directory defines the role's variable values. Often these variables are used for internal purposes within the role. These variables have high precedence and are not intended to be changed when used in a playbook. (chould not modify)
6. **Defaults**: The main.yml file in this directory contains the default values of role variables that can be overwritten when the role is used. These variables have low precedence and are intended to be changed and customized in plays.
7. **Meta**: The main.yml file in this directory contains information about the role, including author, license, platforms, and optional role dependencies.
8. **Tests**: This directory can contain an inventory and test.yml playbook that can be used to test the role.

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
      inventory
      test.yml
    README.md
```
This structure helps in maintaining clean and modular code, making it easier to manage complex configurations.

# Create an role
You can create an role with the following command
```bash
ansible-galaxy role init <rolename>
```

## Naming convention
when you create a name, the common naming convention is:
```
<publisher>.<name of the role>
```

for example:
```
g-martens.postgres
```

# Defining Variables and Defaults
Role variables are defined by creating a vars/main.yml file with key-value pairs in the role directory hierarchy. These variables are referenced in role task files like any other variable: {{ VAR_NAME }}. These variables have a high precedence and can not be overridden by inventory variables. These variables are used by the internal functioning of the role.

Default variables enable you to set default values for variables that can be used in a play to configure the role or customize its behavior. These variables are defined by creating a defaults/main.yml file with key-value pairs in the role directory hierarchy. Default variables have the lowest precedence of any available variables.

Default variable values can be overridden by any other variable, including inventory variables. These variables are intended to provide the person writing a play that uses the role with a way to customize or control exactly what it is going to do. You can use default variables to provide information to the role that it needs to configure or deploy something properly.

Define a specific variable in either vars/main.yml or defaults/main.yml, but not in both places. Use default variables when you intend that the variable values might be overridden.

>[!WARNING]
>Roles should not have site specific data in them or contain any secrets like passwords or private keys because roles are supposed to be generic, reusable, and freely shareable. Therefore, site specific details should not be hard coded into them.
>
>Secrets should be provided to the role through other means. This requirement is one reason that you might want to set role variables when calling a role. Role variables set in the play could provide the secret, or point to an Ansible Vault encrypted file containing the secret.


# Using roles
## In a Play
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
Because roles run first, it generally makes sense to list the roles section before the tasks section, if you must have both. The preceding play can be rewritten as follows without changing how it runs:

```yaml
---
- name: Roles run before tasks
  hosts: remote.example.com
  roles:
    - role: role1
  tasks:
    - name: A task.
      ansible.builtin.debug:
        msg: "This task runs after the role."
```

## As a task
Below you can find a play where a role is imported as a task
```yaml
- name: Run a role as a task
  hosts: remote.example.com
  tasks:
    - name: A task to include role2 here
      ansible.builtin.import_role:
        name: role2
      vars:
        var1: val1
        var2: val2
```

# Special tasks
There are two special task sections, that are occasionally used with roles sections:
- **pre_tasks**
  The pre_tasks section is a list of tasks, similar to tasks, but these tasks run before any of the roles in the roles section.
- **post_tasks**
  The pre_tasks section is a list of tasks, similar to tasks, but these tasks run before any of the roles in the roles section. If any task in the pre-tasks section notify a handler, then those handler tasks run before the roles or normal tasks.

Plays also support a post_tasks keyword. These tasks run after the play's tasks and any handlers notified by the play's tasks.

The following play shows an example with pre_tasks, roles, tasks, post_tasks and handlers. It is unusual that a play would contain all of these sections.
```YAML
- name: Play to illustrate order of execution
  hosts: remote.example.com
  pre_tasks:
    - name: This task runs first
      ansible.builtin.debug:
        msg: This task is in pre_tasks
      notify: my handler
      changed_when: true
  roles:
    - role: role1
  tasks:
    - name: This task runs after the roles
      ansible.builtin.debug:
        msg: This task is in tasks
      notify: my handler
      changed_when: true
  post_tasks:
    - name: This task runs last
      ansible.builtin.debug:
        msg: This task is in post_tasks
      notify: my handler
      changed_when: true
  handlers:
    - name: my handler
      ansible.builtin.debug:
        msg: Running my handler
```

In the preceding example, an ansible.builtin.debug task runs in each tasks section and in the role in the roles section. Each of those tasks notifies the my handler handler, which means the my handler task runs three times:

- After all the pre_tasks tasks run
- After all the roles tasks and tasks tasks run
- After all the post_tasks run