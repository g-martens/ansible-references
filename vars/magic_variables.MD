# Magic Variables
Ansible sets some special variables automatically.These magic variables can also be useful to get information specific to a particular managed host.

Magic variable names are reserved, so you should not define variables with these names.

Four of the most usefull magic variables are:
- **hostvars**
    Contains the variables for managed hosts, and can be used to get the values for another managed host's variables. It does not include the managed host's facts if they have not yet been gathered for that host.
    ```YAML
    ---
    - name: Configure Load Balancer with backend IPs
      hosts: loadbalancer
      tasks:
          - name: Show backend IPs using hostvars
            debug:
              msg: |
              Backend IPs:
              {% for host in groups['web'] %}
              - {{ hostvars[host]['ansible_host'] }}
              {% endfor %}
        ```

- **group_names**
    Lists all groups that the current managed host is in.
    ```YAML
    when: "'developers' in group_names"
    ```

- **groups**
    Lists all groups and hosts in the inventory.
    ```YAML
    when: inventory_hostname in groups['developers']
    ```
- **inventory_hostname**
    Contains the hostname for the current managed host as configured in the inventory. This might be different from the hostname reported by facts for various reasons
    ```YAML
    when: inventory_hostname in groups['developers']
    ```

For all the magic variables, see: [Ansible Community Documentation](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html)