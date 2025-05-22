```

  _____ __ __  _____ ______    ___  ___ ___         ____   ___   _        ___  _____
 / ___/|  |  |/ ___/|      |  /  _]|   |   |       |    \ /   \ | |      /  _]/ ___/
(   \_ |  |  (   \_ |      | /  [_ | _   _ | _____ |  D  )     || |     /  [_(   \_ 
 \__  ||  ~  |\__  ||_|  |_||    _]|  \_/  ||     ||    /|  O  || |___ |    _]\__  |
 /  \ ||___, |/  \ |  |  |  |   [_ |   |   ||_____||    \|     ||     ||   [_ /  \ |
 \    ||     |\    |  |  |  |     ||   |   |       |  .  \     ||     ||     |\    |
  \___||____/  \___|  |__|  |_____||___|___|       |__|\_|\___/ |_____||_____| \___|
                                                                                    

```

# What are System Roles
Ansible system roles are predefined roles that provide a standardized way to manage and configure various aspects of a system using Ansible. They are designed to simplify the automation of common tasks and to promote best practices in system administration. System roles are typically used for tasks such as:

- ***Configuration Management***: Automating the configuration of system settings, services, and applications.
- ***Package Management***: Installing, updating, and removing software packages on the target systems.
- ***Service Management***: Managing system services, including starting, stopping, and enabling services to run at boot.
- ***User and Group Management***: Creating and managing user accounts and groups on the system.
- ***Firewall and Security Settings***: Configuring firewall rules and security settings to protect the system.
- ***Networking***: Managing network interfaces, IP addresses, and routing.

Ansible system roles are often organized in a way that allows for easy reuse and sharing among different projects. They can be found in the Ansible Galaxy repository, where users can download and contribute roles.

The use of system roles helps to ensure consistency across different environments and reduces the complexity of playbooks by encapsulating common tasks into reusable components. This makes it easier for teams to collaborate and maintain their automation code.

# How can I install system roles
When youre ansible controler is an red hat server, then you can install the rhel-system-roles with the yum/dnf package manager

```bash
dnf install -y rhel-system-role
```

Now, these roles are installed in the following directory:
```bash
/usr/share/ansible/roles
```

You can also check this with ansible-galaxy
```bash
ansible-galaxy list
```

# How can i use these system-roles.
You can find the documentation of how you can use these system roles in the following directory:
```bash
/usr/share/doc/rhel-system-roles/<module-name>

# Example output
-rw-r--r--.  1 root root 5937 Apr 24  2022 README.md
-rw-r--r--.  1 root root 7933 Apr 24  2022 README.html
-rw-r--r--.  1 root root 2620 Apr 24  2022 example-selinux-playbook.yml
drwxr-xr-x. 23 root root 4096 May 18  2022 ..
drwxr-xr-x.  2 root root   78 Sep 15  2022 .
```
