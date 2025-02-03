```

 _       ___    ___   ____    _____
| |     /   \  /   \ |    \  / ___/
| |    |     ||     ||  o  )(   \_ 
| |___ |  O  ||  O  ||   _/  \__  |
|     ||     ||     ||  |    /  \ |
|     ||     ||     ||  |    \    |
|_____| \___/  \___/ |__|     \___|
                                   


```
Using loops makes it posible to avoid writing multiple task that use the same module. For example instead of writing five task to ensure that five users exist. you can write one rask that iterates over a list of five users to ensure that they all exist.

To iterate a task over a set of items, you can use the loop keyword. You can configure loops to repeate a task using each item in a list, the content of each of the files in a list, a geneterated sequence of numbers or using more complicated structures.

# Simple Loops
A simple loop iterate a task over a list of items. The loop keyword is addded to the task and takes a value of the list over which the task should be iterated. the loop variable item holds the value used during each iteration.

consider the follwing snippet that uses the ansible.builtin.service  module to ensure that two network services are running
```YAML
- name: Postfix is running
  ansibke.builtin.service:
    name: postfix
    state: started

- name: Dovecot is running
  ansibke.builtin.service:
    name: dovecot
    state: started
```
these two task can be rewritten to user a simple loop so that only ione task is needed to ensure that both services are running
```YAML
- name: Postfix and Dovecotis running
  loop:
    - postfix
    - dovecot
  ansible.builtin.service:
    name: "{{ item }}"
    state: started
```

in the following example, the mail_services variable contains a list of services that need to be running

```YAML
vars:
  mail_services:
    - postfix
    - dovecot
tasks:

  - name: Postfix and Dovecotis running
    loop: "{{ mail_services}}"
    ansible.builtin.service:
      name: "{{ item }}"
      state: started

```


# Loop over a list of Dictonaries
The loop list does not need to be a list of simple values.

in the follwing example, each item in the list is actually a dictonary. Each dictonary in the example has two keys, name and groups, and the values of each key in the current item loop variable can be retrieved with the item['name'] and item['groups'] variables.

```YAML
- name: Users exist and are in the correct groups
  loop:
    - name: jane
      groups: wheel
    - name: joe
      groups: root
  ansible.builtin.user:
    name: "{{ item['name'] }}"
    state: present
    groups: "{{ item['groups'] }}"
```