```


 ____   _       ___     __  __  _  _____
|    \ | |     /   \   /  ]|  |/ ]/ ___/
|  o  )| |    |     | /  / |  ' /(   \_ 
|     || |___ |  O  |/  /  |    \ \__  |
|  O  ||     ||     /   \_ |     \/  \ |
|     ||     ||     \     ||  .  |\    |
|_____||_____| \___/ \____||__|\_| \___|
                                        


```
In playbooks, blocks are clauses that logically group tasks, and can be used to control how tasks are executed. For example, a task block can have a when keyword to apply a conditional to multiple tasks:
```YAML
- name: block example
  hosts: all 
  tasks:
    - name: installing and configuring DNF versionlock plugin
      block:
      - name: package needed by dnf
        ansible.builtin.dnf:
          name: python3-dnf-plugin-versionlock
          state: present
      - name: lock version of tzdata
        ansible.builtin.lineinfile:
          dest: /etc/yum/pluginconf.d/versionlock.list
          line: tzdata-2016j-1
          state: present
      when: ansible_distribution == "RedHat"
```

Blocks also allow for error handling in combination with the rescue and always statements. If any task in a block fails, then rescue tasks are executed to recover.

After the tasks in the block clause run, as well as the tasks in the rescue clause if there was a failure, then tasks in the always clause run.
To summarize:
- **block**: Defines the main tasks to run.
- **rescue**: Defines the tasks to run if the tasks defined in the block clause fail.
- **always**: Defines the tasks that always run independently of the success or failure of tasksdefined in the block and rescue clauses.

The following example shows how to implement a block in a playbook.
```YAML
tasks:
  - name: Upgrade DB
  block:
    - name: upgrade the database
      ansible.builtin.shell:
        cmd: /usr/local/lib/upgrade-database
  rescue:
    - name: revert the database upgrade
      ansible.builtin.shell:
      cmd: /usr/local/lib/revert-database
  always:
    - name: always restart the database
      ansible.builtin.service:
        name: mariadb
        state: restarted
```

The when condition on a block clause also applies to its rescue and always clauses if present.