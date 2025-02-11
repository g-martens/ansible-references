```


  ____  ____  ____   ____   ____      ______    ___  ___ ___  ____  _       ____  ______    ___  _____
 |    ||    ||    \ |    | /    |    |      |  /  _]|   |   ||    \| |     /    ||      |  /  _]/ ___/
 |__  | |  | |  _  ||__  ||  o  |    |      | /  [_ | _   _ ||  o  ) |    |  o  ||      | /  [_(   \_ 
 __|  | |  | |  |  |__|  ||     |    |_|  |_||    _]|  \_/  ||   _/| |___ |     ||_|  |_||    _]\__  |
/  |  | |  | |  |  /  |  ||  _  |      |  |  |   [_ |   |   ||  |  |     ||  _  |  |  |  |   [_ /  \ |
\  `  | |  | |  |  \  `  ||  |  |      |  |  |     ||   |   ||  |  |     ||  |  |  |  |  |     |\    |
 \____j|____||__|__|\____||__|__|      |__|  |_____||___|___||__|  |_____||__|__|  |__|  |_____| \___|
                                                                                                      


```
Ansible uses the Jinja2 templating system for template files. Ansible also uses Jinja2 syntax to reference variables in playbooks, so you already know a little about how to use it.


# Example of an Jinja Template
The following example shows how to create a template for /etc/ssh/sshd_config with variables and facts retrieved by Ansible from managed hosts. When the template is deployed by a play, any facts are replaced by their values for the managed host being configured.
```JINJA
# {{ ansible_managed }}
# DO NOT MAKE LOCAL MODIFICATIONS TO THIS FILE BECAUSE THEY WILL BE LOST

Port {{ ssh_port }}
ListenAddress {{ ansible_facts['default_ipv4']['address'] }}

HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key

SyslogFacility AUTHPRIV

PermitRootLogin {{ root_allowed }}
AllowGroups {{ groups_allowed }}

AuthorizedKeysFile /etc/.rht_authorized_keys .ssh/authorized_keys

PasswordAuthentication {{ passwords_allowed }}

ChallengeResponseAuthentication no

GSSAPIAuthentication yes
GSSAPICleanupCredentials no

UsePAM yes

X11Forwarding yes
UsePrivilegeSeparation sandbox

AcceptEnv LANG LC_CTYPE LC_NUMERIC LC_TIME LC_COLLATE LC_MONETARY LC_MESSAGES
AcceptEnv LC_PAPER LC_NAME LC_ADDRESS LC_TELEPHONE LC_MEASUREMENT
AcceptEnv LC_IDENTIFICATION LC_ALL LANGUAGE
AcceptEnv XMODIFIERS

Subsystem sftp	/usr/libexec/openssh/sftp-server
```

# How to deploy Jinja Templates
Jinja2 templates are a powerful tool that you can use to customize configuration files to be deployed on managed hosts. When the Jinja2 template for a configuration file has been created, it can be deployed to managed hosts by using the ansible.builtin.template module, which supports the transfer of a local file on the control node to the managed hosts.

To use the ansible.builtin.template module, use the following syntax. The value associated with the src key specifies the source Jinja2 template, and the value associated with the dest key specifies the file to be created on the destination hosts.
```YAML
tasks:
  - name: template render
    ansible.builtin.template:
      src: /tmp/j2-template.j2
      dest: /tmp/dest-config-file.txt
```

# Structures
You can use Jinja2 control structures in template files to reduce repetitive typing, to enter entries for each host in a play dynamically, or conditionally insert text into a file.

## Comments
In Jinja you can create comments with the folling:
```JINJA
{# This is an example of a comment #}
```
>[!WARNING]
>The comments in the Jinja Templates, are not going to be rendered!

## Using Loops
Jinja2 uses the for statement to provide looping functionality. In the following example, the users variable has a list of values. The user variable is replaced with all the values in the users variable, one value per line.
```JINJA
{% for user in users %}
      {{ user }}
{% endfor %}
```

The following example template uses a for statement and a conditional to run through all the values in the users variable, replacing myuser with each value, unless the value is root.
```JINJA
{{ '{#' }} for statement {{ '#}' }}
{% for myuser in users if not myuser == "root" %}
User number {{ loop.index }} - {{ myuser }}
{% endfor %}
```

The loop.index variable expands to the index number that the loop is currently on. It has a value of 1 the first time the loop executes, and it increments by 1 through each iteration.

As another example, this template also uses a for statement. It assumes a myhosts variable that contains a list of hosts to be managed has been defined by the inventory being used. If you put the following for statement in a Jinja2 template, all hosts in the myhosts group from the inventory would be listed in the resulting file.
```JINJA
{% for myhost in groups['myhosts'] %}
{{ myhost }}
{% endfor %}
```
For a more practical example, you can use this example to generate an /etc/hosts file from host facts dynamically. Assume that you have the following playbook:

```YAML
- name: /etc/hosts is up to date
  hosts: all
  gather_facts: true
  tasks:
    - name: Deploy /etc/hosts
      ansible.builtin.template:
        src: templates/hosts.j2
        dest: /etc/hosts
```
The following three-line templates/hosts.j2 template constructs the file from all hosts in the group all. (The middle line is extremely long in the template due to the length of the variable names.) It iterates over each host in the group to get three facts for the /etc/hosts file.
```YAML
{% for host in groups['all'] %}
{{ hostvars[host]['ansible_facts']['default_ipv4']['address'] }} {{hostvars[host]['ansible_facts']['fqdn'] }} {{ hostvars[host]['ansible_facts']['hostname'] }}
{% endfor %}
```

## Using Conditionals
Jinja2 uses the if statement to provide conditional control. This allows you to put a line in a deployed file if certain conditions are met.

In the following example, the value of the result variable is placed in the deployed file only if the value of the finished variable is True.
```JINJA
{% if finished %}
{{ result }}
{% endif %}
```


## Variable Filters
Jinja2 provides filters which change the output format for template expressions, essentially converting the data in a variable to some other format in the file that results from the template.

For example, filters are available for languages such as YAML and JSON. The to_json filter formats the expression output using JSON, and the to_yaml filter formats the expression output using YAML.

```JINJA
{{ output | to_json }}
{{ output | to_yaml }}
```

Additional filters are available, such as the to_nice_json and to_nice_yaml filters, which format the expression output in either JSON or YAML human-readable format.
```JINJA
{{ output | to_nice_json }}
{{ output | to_nice_yaml }}
```

Both the from_json and from_yaml filters expect strings in either JSON or YAML format, respectively.
```JINJA
{{ output | from_json }}
{{ output | from_yaml }}
```


# Advanced Examples

## Example 1: create an host file based on vars
Source data:
```YAML
ansible_managed: "This file is managed by Ansible. DO NOT EDIT!"
network_data:
  - sname: severa
    lname: severa.example.com
    ipv4: 172.25.250.7/24
    gw4: 172.25.250.254
    mtu: 1500
    dns: 172.25.250.254

  - sname: severb
    lname: severb.example.com
    ipv4: 172.25.250.8/24
    gw4: 172.25.250.254
    mtu: 1500
    dns: 172.25.250.254

  - sname: severc
    lname: severc.example.com
    ipv4: 172.25.250.9/24
    gw4: 172.25.250.254
    mtu: 1500
    dns: 172.25.250.254
```
What we want to do, is to take somne values, and place this in the /etc/hosts file.

We can do this as follows:
```JINJA
{# Add a comment in the rendered file #}
# {{ ansible_managed }}

{# First we create an for loop #}
{% for host in network_data %}
{# Now we add for each host an line with it values#}
{{ host['ipv4'] | ipaddr('address')  }}  {{ host['lname'] }}  {{ host['sname'] }}

{# As you can see in the first part, it takes the value if ipv4, en with the pipe, it takes only the value of an ip (filter) (for example 172.25.250.7 #}

{# Now we end the loop #}
{% endfor %}
```
The output of the rendered file is:
```
172.25.250.7 severa.example.com servera
172.25.250.8 severb.example.com serverb
172.25.250.9 severc.example.com serverc
```

## Example 2: Create an inventory File based on ansible_facts
```JINJA
Hostname: {{ ansible_facts['fqdn'] }}

Network Configuration:
  ipv4: {% for ips in ansible_facts['all_ipv4_addresses'] %}
        {{ ips }}
        {% endfor %}

  ipv6: {% for ips in ansible_facts['all_ipv6_addresses'] %}
        {{ ips }}
        {% endfor %}

Here is a big list of software installed on the system

{% for package in ansible_facts['packages'] | dict2items %}
=============================================================
{{ package['value'] | json_query('[*].name') | regex_replace('\'') | regex_replace('\[') | regex_replace('\]') }} : {{ package['value'] | json_query('[*].name') | regex_replace('\'') | regex_replace('\[') | regex_replace('\]') }}
{% endfor %}

```

See for more info the Official docs: [https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_filters.html](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_filters.html)