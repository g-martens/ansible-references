```


   ___  ____   ____   ___   ____       __ __   ____  ____   ___    _      ____  ____    ____ 
  /  _]|    \ |    \ /   \ |    \     |  |  | /    ||    \ |   \  | |    |    ||    \  /    |
 /  [_ |  D  )|  D  )     ||  D  )    |  |  ||  o  ||  _  ||    \ | |     |  | |  _  ||   __|
|    _]|    / |    /|  O  ||    /     |  _  ||     ||  |  ||  D  || |___  |  | |  |  ||  |  |
|   [_ |    \ |    \|     ||    \     |  |  ||  _  ||  |  ||     ||     | |  | |  |  ||  |_ |
|     ||  .  \|  .  \     ||  .  \    |  |  ||  |  ||  |  ||     ||     | |  | |  |  ||     |
|_____||__|\_||__|\_|\___/ |__|\_|    |__|__||__|__||__|__||_____||_____||____||__|__||___,_|
                                                                                             


```
With error handling you can control the behavour of ansible when a tasks fails, and what the conditions are for a task to fail.
For this you can use the following keywords:
1. ignore_errors
2. force_handlers
3. failed_when
4. changed_when


# Igonore Errors
By default, if a tasks fails, the play is aborted. However this behavour can be overridden by ignoring failed_tasks. You can use the ignore_errors keyword in a task to accomplish this.
```YAML
- name: Latest version of notapkg is installed
  ansible.builtin.dnf:
    name: notapkg
    state: latest
  ignore_errors: true
```

# Force Handlers
Normaly when a task fails and the play aborts on that host, any handlers that had ben notified by earlier tasks in the play wont run. if you set the force_handlers: true keyword on the play, than notified handlers are called even if the play aborted because a later task failed.
See the following snippet for an example:
```YAML
---
- hosts: all
  force_handlers: true
  tasks:
    - name: a task which always notifies its handler
      ansible.builtin.command: /bin/true
      notify: restart the database

    - name: a task which fails because the package doesn't exist
      ansible.builtin.dnf:
        name: notapkg
      state: latest

   handlers:
    - name: restart the database
      ansible.builtin.service:
        name: mariadb
        state: restarted
```

# Force handlers to run now instead of after the play
Normaly any handlers are executed after finishing all task. But when you want to run a handler at a certain point, you can use the meta: flush_handlers

```YAML
- name: Force any notified handlers to execute now
  ansible.buildin.meta: flush_handlers
```

>[!WARNING]
>Remember that handlers are notified when a task reports a changed result but are not notified when it reports an ok or failed result.
>
>If you set force_handlers: true on the play, then any handlers that have been notified are run even if a later task failure causes the play to fail. Otherwise, handlers are not run at all when a play fails.
>
>Setting force_handlers: true on a play does not cause handlers to be notified for tasks that report ok or failed; it only causes the handlers to run that have already been notified before the point at which the play failed.

# Specifying Task Failure Conditions
You can use the failed_when keyword on a task to specify which conditions indicate that thetask has failed. This is often used with command modules that might successfully execute a command, but where the command's output indicates a failure.

For example, you can run a script that outputs an error message and then use that message to  define the failed state for the task. The following example shows one way that you can use the failed_when keyword in a task:
```YAML
tasks:
  - name: Run user creation script
    ansible.builtin.shell: /usr/local/bin/create_users.sh
    register: command_result
    failed_when: "'Password missing' in command_result.stdout"
```
The ansible.builtin.fail module can also be used to force a task failure. You could instead write that example as two tasks:
```YAML
tasks:
- name: Run user creation script
  ansible.builtin.shell: /usr/local/bin/create_users.sh
  register: command_result
  ignore_errors: true

- name: Report script failure
  ansible.builtin.fail:
    msg: "The password is missing in the output"
  when: "'Password missing' in command_result.stdout"
```
You can use the ansible.builtin.fail module to provide a clear failure message for the task. This approach also enables delayed failure, which means that you can run intermediate tasks to complete or roll back other changes.


# Specifying When a Task Reports "Changed" Results
When a task makes a change to a managed host, it reports the changed state and notifies handlers. When a task does not need to make a change, it reports ok and does not notify handlers.

Use the changed_when keyword to control how a task reports that it has changed something on the managed host. For example, the ansible.builtin.command module in the next example validates the httpd configuration on a managed host.

This task validates the configuration syntax, but nothing is actually changed on the managed host. Subsequent tasks can use the value of the httpd_config_status variable. It normally would always report changed when it runs. To suppress that change report, changed_when: false is set so that it only reports ok or failed.
```YAML
- name: Validate httpd configuration
  ansible.builtin.command: httpd -t
  changed_when: false
  register: httpd_config_status
```

The following example uses the ansible.builtin.shell module and only reports changed if the string "Success" is found in the output of the registered variable. If it does report changed, then it notifies the handler.
```YAML
tasks:
  - ansible.builtin.shell:
      cmd: /usr/local/bin/upgrade-database
    register: command_result
    changed_when: "'Success' in command_result.stdout"
    notify:
    - restart_database

handlers:
  - name: restart_database
    ansible.builtin.service:
      name: mariadb
      state: restarted
```