```


 __ __   ____  ____   ___    _        ___  ____    _____
|  |  | /    ||    \ |   \  | |      /  _]|    \  / ___/
|  |  ||  o  ||  _  ||    \ | |     /  [_ |  D  )(   \_ 
|  _  ||     ||  |  ||  D  || |___ |    _]|    /  \__  |
|  |  ||  _  ||  |  ||     ||     ||   [_ |    \  /  \ |
|  |  ||  |  ||  |  ||     ||     ||     ||  .  \ \    |
|__|__||__|__||__|__||_____||_____||_____||__|\_|  \___|
                                                        


```
Ansible modules are designed to be idempotent. This means that if you run a playbook multiple times, the result is always the same. You can run plays and their tasks multiple times, but managed hosts are only changed if those changes are required to get the managed hosts to the desired
state.

However, sometimes when a task does make a change to the system, a further task might need to be run. For example, a change to a service's configuration file might then require that the service be reloaded so that the changed configuration takes effect.

Handlers are tasks that respond to a notification triggered by other tasks. Tasks only notify their handlers when the task changes something on a managed host. Each handler is triggered by its name after the play's block of tasks.

If no task notifies the handler by name then the handler does not run. If one or more tasks notify the handler, the handler runs once after all other tasks in the play have completed. Because handlers are tasks, administrators can use the same modules in handlers that they would use for
any other task.

Normally, handlers are used to reboot hosts and restart services.

>[!WARNING]
>Always use unique names for your handlers. You might have unexpected results if more than one handler uses the same name.

Handlers can be considered as inactive tasks that only get triggered when explicitly invoked using a notify statement. The following snippet shows how the Apache server is only restarted by the restart apache handler when a configuration file is updated and notifies it:
```YAML
tasks:
- name: copy demo.example.conf configuration template
  ansible.builtin.template:
    src: /var/lib/templates/demo.example.conf.template
    dest: /etc/httpd/conf.d/demo.example.conf
  notify:
    - restart apache

handlers:
- name: restart apache
  ansible.builtin.service:
    name: httpd
    state: restarted
```
>[!WARNING]
>Keep in mind, a Handler is always started at the end of all tasks.
>if you need to start the handlers at a curtain point, you can use:
> ansible.builtin.meta: flush_handlers