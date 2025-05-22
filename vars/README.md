```

 __ __   ____  ____    _____
|  |  | /    ||    \  / ___/
|  |  ||  o  ||  D  )(   \_ 
|  |  ||     ||    /  \__  |
|  :  ||  _  ||    \  /  \ |
 \   / |  |  ||  .  \ \    |
  \_/  |__|__||__|\_|  \___|
                            


```
Ansible support variables that can be used to store values that you can reuse throughout files in an Ansible project. With variables you can create te plays/roles more dynamic, which improves reusability.

# Variables Precedence
Ansible does apply variable precedence, and you might have a use for it. Here is the order of precedence from least to greatest (the last listed variables override all other variables):

1. command line values (for example, -u my_user, these are not variables) 1
2. role defaults (as defined in Role directory structure) 1
3. inventory file or script group vars 2
4. inventory group_vars/all 3
5. playbook group_vars/all 3
6. inventory group_vars/* 3
7. playbook group_vars/* 3
8. inventory file or script host vars 2
9. inventory host_vars/* 3
10. playbook host_vars/* 3
11. host facts / cached set_facts 4
12. play vars
13. play vars_prompt
14. play vars_files
15. role vars (as defined in Role directory structure)
16. block vars (only for tasks in block)
17. task vars (only for the task)
18. include_vars
19. set_facts / registered vars
20. role (and include_role) params
21. include params
22. extra vars (for example, -e "user=my_user")(always win precedence)

### Footnotes
1. Tasks in each role see their own role’s defaults. Tasks defined outside of a role see the last role’s defaults.
2. **(1,2)** Variables defined in inventory file or provided by dynamic inventory.
3. **(1,2,3,4,5,6)** Includes vars added by ‘vars plugins’ as well as host_vars and group_vars which are added by the default vars plugin shipped with Ansible.
4. When created with set_facts’s cacheable option, variables have the high precedence in the play, but are the same as a host facts precedence when they come from the cache.


See for more info about vars: https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable


# Naming Variables
Variable MUST start with a letter, and they can only contain letters, numbers and underscores.

**Examples:**
```YAML
# Bad examples
web server:
remote.file:
1st file:
remoteserver$1:

# Good examples
web_server:
remote_file:
file_1:
file1:
remote_server_1:
remote_server1:
```

>[!WARNING]
>Keep in mind that the variable name bust be declarative
>http: 80 is not clear
>http_web_port_insecure: 80 is better

# Using Variables
After you have declared variables, you can use the variables in tasks. Variables are referenced by placing the variable name in double braces ({{  }}). Ansible substitutes the variable with its value when a task is executed.

## Simple Variables
**Example:**
```YAML
---
- name: Variable examples
  host: all
  become: false

  vars:
    user: joe
    home: /home/joe

  tasks:
    - name: this line will read: Creates the user joe
      user:
        # This line will create the user named joe
        name: "{{ user }}"
```

You can also choose to create an file where you will define the variables:
an example of this file is: vars.yml:
```YAML
user: joe
home: /home/joe
```

You can define this file with vars_file:
**Example:**
```YAmL
---
- name: Variable examples
  host: all
  become: false

  vars_file: vars.yml

  tasks:
    - name: this line will read: Creates the user joe
      user:
        # This line will create the user named joe
        name: "{{ user }}"
```

## Using Dictionaries as Variables
Instead of assigning configuration data that relates to the same element to multiple variables, Administrators can use dictionaries. A Dictionary is a data structure containing key-value pairs, where the values can also be dictionaries.

For example, consider the following snippet:
```YAML
user1_first_name: Bob
user1_last_name: Jones
user1_home_dir: /users/bjones
user2_first_name: Anne
user2_last_name: Cook
user2_home_dir: /users/acook
```
This could be written as a dictionary called users
```YAML
users:
  bjones:
    first_name: Bob
    last_name: Jones
    home_dir: /home/bjones
  acook:
    first_name: Anne
    last_name: Cook
    home_dir: /users/acook
```
Now you can use the following variables to access user data:

```YAML
# Returns 'Bob'
user.bjones.first_name

# Returns '/users/acook'
users.acook.home_dir
```

Because the variable is defined as a python dictionary, an alternative syntax is available.
```YAML
# Returns 'Bob'
users['bjones']['first_name']

# Returns '/users/acook'
users['acook']['home_dir']
```
>[!WARNING]
>The dot notation can cause problems if the key names are the same as names of Python methods or attributes, such as discard, copy, add, and so on.
>Using the brackets notation can help avoid conflicts and errors.
>Both syntaxes are valid, but to make troubleshooting easier, Red Hat recommends that you use one syntax consistently in all files throughout any given Ansible project.

## Using List as Variables
An list variable is defined as following:
```YAML
users:
  - name: alice
    description: developer
  - name: bob
    description: tester
  - name: charlie
    description: developer
```
For example, if you want to create the user when his/her description is developer, you can define this as following in your playbook:
```YAML
---
- name: Create developer users
  hosts: all
  become: yes
  vars:
    users:
      - name: alice
        description: developer
      - name: bob
        description: tester
      - name: charlie
        description: developer

  tasks:
    - name: Add developer users
      user:
        name: "{{ item.name }}"
        state: present
      loop: "{{ users }}"
      when: item.description == "developer"
```



# Capturing Command output with Registered Variables
You can use the register statement of a task, to capture the output of a command, or  other information about the execution of an module. this output is saved into a variable that can be used later in the playbook for either debugging purposes or to achieve something else, such as applying  a particular configuration setting based on a command's output.

**Example:**
```YAML
---
- name: Installs a package and print the result
  hosts: all
  tasks:
    - name: Install the package
      register: install_result
      ansible.buitlin.dnf:
        name: httpd
        state: started

    - debug:
        var: install_result
```