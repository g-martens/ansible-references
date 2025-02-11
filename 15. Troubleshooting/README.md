```


 ______  ____   ___   __ __  ____   _        ___  _____ __ __   ___    ___   ______  ____  ____    ____ 
|      ||    \ /   \ |  |  ||    \ | |      /  _]/ ___/|  |  | /   \  /   \ |      ||    ||    \  /    |
|      ||  D  )     ||  |  ||  o  )| |     /  [_(   \_ |  |  ||     ||     ||      | |  | |  _  ||   __|
|_|  |_||    /|  O  ||  |  ||     || |___ |    _]\__  ||  _  ||  O  ||  O  ||_|  |_| |  | |  |  ||  |  |
  |  |  |    \|     ||  :  ||  O  ||     ||   [_ /  \ ||  |  ||     ||     |  |  |   |  | |  |  ||  |_ |
  |  |  |  .  \     ||     ||     ||     ||     |\    ||  |  ||     ||     |  |  |   |  | |  |  ||     |
  |__|  |__|\_|\___/  \__,_||_____||_____||_____| \___||__|__| \___/  \___/   |__|  |____||__|__||___,_|
                                                                                                        


```
Below you can find some issues, and how you can troubleshoot these

# Troubleshoot Connections
Many common problems when using Ansible to manage hosts are associated with connections to the host and with configuration problems around the remote user and privilege escalation.

## Problems Authenticating to Managed Hosts
If you are having problems authenticating to a managed host, make sure that you have remote_user set correctly in your configuration file or in your play.
For example, you might see the following output when running a playbook that is designed to connect to the remote root user account:
```bash
[student@controlnode ~]$ ansible-navigator run -m stdout playbook.yml

PLAY [Install a samba server] **************************************************

TASK [Gathering Facts] *********************************************************
fatal: [host.lab.example.com]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: developer@host: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).", "unreachable": true}

PLAY RECAP *********************************************************************
host.lab.example.com    : ok=0    changed=0    unreachable=1    failed=0    skipped=0    rescued=0    ignored=0
Please review the log for errors.
```
In this case, ansible-navigator is trying to connect as the developer user account, according to the preceding output. One reason this might happen is if ansible.cfg has been configured in the project to set the remote_user to the developer user instead of the root user.

Another reason you could see a "permission denied" error like this is if you do not have the correct SSH keys set up, or did not provide the correct password for that user.
```bash
[root@controlnode ~]# ansible-navigator run -m stdout playbook.yml

TASK [Gathering Facts] *********************************************************
fatal: [host.lab.example.com]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: root@host: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).", "unreachable": true}

PLAY RECAP *********************************************************************
host    : ok=0    changed=0    unreachable=1    failed=0    skipped=0    rescued=0    ignored=0

Please review the log for errors.
```
In the preceding example, the playbook is attempting to connect to the host machine as the root user but the SSH key for the root user on the controlnode machine has not been added to the authorized_keys file for the root user on the host machine.



## Problems with Privilege Escalation
If your playbook connects as a remote_user and then uses privilege escalation to become the root user (or some other user), make sure that become is set properly, and that you are using the correct value for the become_user directive. The setting for become_user is root by default.

If the remote user needs to provide a sudo password, you should confirm that you are providing the correct sudo password, and that sudo on the managed host is configured correctly.
```bash
[user@controlnode ~]$ ansible-navigator run -m stdout playbook.yml

TASK [Gathering Facts] *********************************************************
fatal: [host]: FAILED! => {"msg": "Missing sudo password"}

PLAY RECAP *********************************************************************
host             : ok=0    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0

Please review the log for errors.
```
In the preceding example, the playbook is attempting to run sudo on the host machine but it fails. The remote_user is not set up to run sudo commands without a password on the host machine. Eithersudo on the host machine is not properly configured, or it is supposed to require a sudo password and you neglected to provide one  when running the playbook. (-k)

>[!WARNING]
>Normally, ansible-navigator runs as root inside its automation execution environment. However, the root user in the container has access to SSH keys provided by the user that ran ansible-navigator on the workstation. This can be slightly confusing when you are trying to debug remote_user and become directives, especially if you are used to the earlier ansible-playbook command that runs as the user on the workstation.

## Problems with Name or Address Resolution
A more subtle problem has to do with inventory settings. For a complex server with multiple network addresses, you might need to use a particular address or DNS name when connecting to that system. You might not want to use that address as the machine's inventory name for better readability. You can set a host inventory variable, ansible_host, that overrides the inventory name with a different name or IP address and be used by Ansible to connect to that host. This variable could be set in the host_vars file or directory for that host, or could be set in the inventory file itself.

For example, the following inventory entry configures Ansible to connect to 192.0.2.4 when processing the web4.phx.example.com host:
```bash
web4.phx.example.com ansible_host=192.0.2.4
```
This is a useful way to control how Ansible connects to managed hosts. However, it can also cause problems if the value of ansible_host is incorrect.

## Python on Managed Hosts
For normal operation, Ansible requires a Python interpreter to be installed on managed hosts running Linux. Ansible attempts to locate a Python interpreter on each Linux managed host the first time a module is run on that host.
```bash
[user@controlnode ~]$ ansible-navigator run -m stdout playbook.yml

TASK [Gathering Facts] *********************************************************
fatal: [host]: FAILED! => {"ansible_facts": {}, "changed": false, "failed_modules": {"ansible.legacy.setup": {"ansible_facts": {"discovered_interpreter_python": "/usr/bin/python"}, "failed": true, "module_stderr": "Shared connection to host closed.\r\n", "module_stdout": "/bin/sh: 1: /usr/bin/python: not found\r\n", "msg": "The module failed to execute correctly, you probably need to set the interpreter.\nSee stdout/stderr for the exact error", "rc": 127, "warnings": ["No python interpreters found for host host (tried ['python3.10', 'python3.9', 'python3.8', 'python3.7', 'python3.6', 'python3.5', '/usr/bin/python3', '/usr/libexec/platform-python', 'python2.7', 'python2.6', '/usr/bin/python', 'python'])"]}}, "msg": "The following modules failed to execute: ansible.legacy.setup\n"}

PLAY RECAP *********************************************************************
host    : ok=0    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
Please review the log for errors.
```
In the preceding example, the playbook fails because Ansible is unable to find a suitable Python interpreter on the host machine.

# Using Check Mode as a Testing Tool
You can use the ansible-navigator run --check command to run "smoke tests" on a playbook. This option runs the playbook, connecting to the managed hosts normally but without making changes to them.

If a module used within the playbook supports "check mode", then the changes that would have been made to the managed hosts are displayed but not performed. If check mode is not supported by a module, then ansible-navigator does not display the predicted changes, but the module still takes no action.
```
[student@demo ~]$ ansible-navigator run -m stdout playbook.yml --check
```
You can force tasks to always run in check mode or to always run normally with the check_mode setting. If a task has check_mode: true set, it always runs in its check mode and does not perform any action, even if you do not pass the --check option to ansible-navigator. Likewise, if a task has check_mode: false set, it always runs normally, even if you pass --check to ansible-navigator.

The following task always runs in check mode, and does not make changes to managed hosts.
```yaml
  tasks:
    - name: task always in check mode
      ansible.builtin.shell: uname -a
      check_mode: true

  tasks:
    - name: task always runs even in check mode
      ansible.builtin.shell: uname -a
      check_mode: false
```
This can be useful because you can run most of a playbook normally and test individual tasks with check_mode: true. Many plays use facts or set variables to conditionally run tasks. Conditional tasks might fail if a fact or variable is undefined, due to the task that collects them or sets them not executing on a managed node. You can use check_mode: false on tasks that gather facts or set variables but do not otherwise change the managed node. This enables the play to proceed further when using --check mode.

A task can determine if the playbook is running in check mode by testing the value of the magic variable ansible_check_mode. This Boolean variable is set to true if the playbook is running in check mode.

>[!WARNING]
>The ansible-navigator run --check command might not work properly if your tasks use conditionals. One reason for this might be that the conditionals depend on some preceding task in the play actually running so that the condition evaluates correctly.

The ansible-navigator command also provides a --diff option. This option reports the changes made to the template files on managed hosts. If used with the --check option, those changes are displayed in the command's output but not actually made.
```
[student@demo ~]$ ansible-navigator run -m stdout playbook.yml --check --diff
```

# Testing with Modules
Some modules can provide additional information about the status of a managed host. The following list includes some Ansible modules that can be used to test and debug issues on managed hosts.

The ansible.builtin.uri module provides a way to verify that a RESTful API is returning the required content.
```yaml
 tasks:
    - ansible.builtin.uri:
        url: http://api.myapp.example.com
        return_content: true
      register: apiresponse

    - ansible.builtin.fail:
        msg: 'version was not provided'
      when: "'version' not in apiresponse.content"
```
The ansible.builtin.script module runs a script on managed hosts, and fails if the return code for that script is nonzero. The script must exist in the Ansible project and is transferred to and run on the managed hosts.
```yaml
  tasks:
    - ansible.builtin.script: scripts/check_free_memory --min 2G
```
The ansible.builtin.stat module gathers facts for a file much like the stat command. You can use it to register a variable and then test to determine if the file exists or to get other information about the file. If the file does not exist, the ansible.builtin.stat task does not fail, but its registered variable reports false for *['stat']['exists'].

In this example, an application is still running if /var/run/app.lock exists, in which case the play should abort.
```yaml
  tasks:
    - name: Check if /var/run/app.lock exists
      ansible.builtin.stat:
        path: /var/run/app.lock
      register: lock

    - name: Fail if the application is running
      ansible.builtin.fail:
      when: lock['stat']['exists']
```

The ansible.builtin.assert module is an alternative to the ansible.builtin.fail module. The ansible.builtin.assert module supports a that option that takes a list of conditionals. If any of those conditionals are false, the task fails. You can use the success_msg and fail_msg options to customize the message it prints if it reports success or failure.

The following example repeats the preceding one, but uses ansible.builtin.assert instead of the ansible.builtin.fail module:
```yaml
  tasks:
    - name: Check if /var/run/app.lock exists
      ansible.builtin.stat:
        path: /var/run/app.lock
      register: lock

    - name: Fail if the application is running
      ansible.builtin.assert:
        that:
          - not lock['stat']['exists']
```



# Running Ad Hoc Commands with Ansible
An ad hoc command is a way of executing a single Ansible task quickly, one that you do not need to save to run again later. They are simple, online operations that can be run without writing a playbook.

Ad hoc commands do not run inside an automation execution environment container. Instead, they run using Ansible software, roles, and collections installed directly on your workstation. To use ad hoc Ansible Core 2.13 commands, you need to install the ansible-core RPM package on your workstation.

>[!WARNING]
>The ansible-core RPM package provides only the modules in the ansible.builtin Ansible Content Collection. If you need modules from other collections, you need to install those on your workstation separately.

Ad hoc commands are useful for quick tests and troubleshooting. For example, you can use an ad hoc command to make sure that hosts are reachable using the ansible.builtin.ping module. You could use another ad hoc command to view resource usage on a group of hosts using the ansible.builtin.command module.

Ad hoc commands do have their limits, and in general you want to use Ansible Playbooks to realize the full power of Ansible.

Use the ansible command to run ad hoc commands:
```bash
[user@controlnode ~]$ ansible host-pattern -m module [-a 'module arguments']  [-i inventory]
```
The host-pattern argument is used to specify the managed hosts against which the ad hoc command should be run. The -i option is used to specify a different inventory location to use from the default in the current Ansible configuration file. The -m option specifies the module that Ansible should run on the targeted hosts. The -a option takes a list of arguments for the module as a quoted string.

Ansible ad hoc commands can be useful, but should be kept to troubleshooting and one-time use cases. For example, if you are aware of multiple pending network changes, it is more efficient to create a playbook with an ansible.builtin.ping task that you can run multiple times, compared to typing out a one-time use ad hoc command multiple times.

## Testing Managed Hosts Using Ad Hoc Commands
The following examples illustrate some tests that can be made on a managed host using ad hoc commands.

You have used the ansible.builtin.ping module to test whether you can connect to managed hosts. Depending on the options that you pass, you can also use it to test whether privilege escalation and credentials are correctly configured.
```bash
[student@demo ~]$ ansible demohost -m ansible.builtin.ping
demohost | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
[student@demo ~]$ ansible demohost -m ansible.builtin.ping --become
demohost | FAILED! => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "module_stderr": "sudo: a password is required\n",
    "module_stdout": "",
    "msg": "MODULE FAILURE\nSee stdout/stderr for the exact error",
    "rc": 1
}
```
This example returns the current available space on the disks configured on the demohost managed host. That can be useful to confirm that the file system on the managed host is not full.

```bash
[student@demo ~]$ ansible demohost -m ansible.builtin.command -a 'df'
```

This example returns the current available free memory on the demohost managed host.
```bash
[student@demo ~]$ ansible demohost -m ansible.builtin.command -a 'free -m'
```