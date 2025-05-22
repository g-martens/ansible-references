```


    __   ___   _      _        ___    __ ______  ____  ___   ____   _____
   /  ] /   \ | |    | |      /  _]  /  ]      ||    |/   \ |    \ / ___/
  /  / |     || |    | |     /  [_  /  /|      | |  ||     ||  _  (   \_ 
 /  /  |  O  || |___ | |___ |    _]/  / |_|  |_| |  ||  O  ||  |  |\__  |
/   \_ |     ||     ||     ||   [_/   \_  |  |   |  ||     ||  |  |/  \ |
\     ||     ||     ||     ||     \     | |  |   |  ||     ||  |  |\    |
 \____| \___/ |_____||_____||_____|\____| |__|  |____|\___/ |__|__| \___|
                                                                         


```
# What are collections
With Ansible Content Collections, Ansible code updates are separated from updates to modules and plug-ins. An Ansible Content Collection provides a set of related **modules**, **roles**, and other **plug-ins** that you can use in your playbooks. This approach enables vendors and developers to maintain and distribute their collections at their own pace, independently of Ansible releases.

**For example:**

- The **redhat.insights** content collection provides modules and roles that you can use to register a system with Red Hat Insights for Red Hat Enterprise Linux.
- The **cisco.ios** content collection, supported and maintained by Cisco, provides modules and plug-ins that manage Cisco IOS network appliances.
- The **community.crypto** content collection provides modules that create SSL/TLS certificates.

Ansible Content Collections also provide flexibility. You can install only the content you need instead of installing all supported modules.

You can also select a specific version of a collection (possibly an earlier or later one) or choose between a version of a collection supported by Red Hat or vendors, or one provided by the community.

Ansible 2.9 and later support Ansible Content Collections. Upstream Ansible unbundled most modules from the core Ansible code in Ansible Base 2.10 and Ansible Core 2.11 and placed them in collections. Red Hat Ansible Automation Platform 2.2 provides automation execution environments based on Ansible Core 2.13 that inherit this feature.

# Installing Collections
Before your playbooks can use content from an Ansible Content Collection, you must ensure that the collection is available in your automation execution environment.
The Ansible configuration collections_paths setting specifies a colon separated list of paths on the system where Ansible looks for installed collections.

You can set this directive in the ansible.cfg configuration file.

```ini
[defaults]
collections_paths = ~/.ansible/collections:/usr/share/ansible/collections
```

>[!WARNING]
> if you set collections_paths to some other value in your Ansible project's ansible.cfg file and eliminate those two directories ( ~/.ansible/collections:/usr/share/ansible/collections), then ansible-navigator cannot find the Ansible Content Collections provided inside the automation execution environment in its version of those directories.

## Installing Ansible Content Collections
The following example uses the ansible-galaxy collection install command to download and install the community.crypto Ansible Content Collection. The -p collections option installs the collection in the local collections subdirectory.

```bash
# From galaxy
ansible-galaxy collection install community.crypto -p collections

# From Tarball
ansible-galaxy collection install /tmp/community-dns-1.2.0.tar.gz -p collections
ansible-galaxy collection install http://www.example.com/redhat-insights-1.0.5.tar.gz -p collections

# From git
ansible-galaxy collection install git@git.example.com:organization/repo_name.git -p collections
```

## Installing Ansible Content Collections with a Requirements File
If your Ansible project needs additional Ansible Content Collections, you can create a collections/requirements.yml file in the project directory that lists all the collections that the project requires. Automation controller detects this file and automatically installs the specified collections before running your playbooks.

A requirements file for Ansible Content Collections is a YAML file that consists of a collections dictionary key that has the list of collections to install as its value. Each list item can also specify the particular version of the collection to install, as shown in the following example:
```YAML
---
collections: 1
  - name: community.crypto

  - name: ansible.posix
    version: 1.2.0

  - name: /tmp/community-dns-1.2.0.tar.gz

  - name: http://www.example.com/redhat-insights-1.0.5.tar.gz

  - name: git+https://github.com/ansible-collections/community.general.git
    version: main
```

Then you can install the collections by specifying the requirements.yml file
```bash
ansible-galaxy collection install -r collections/requirements.yml -p collections
```

# Configuring Ansible Content Collection Sources
By default, the ansible-galaxy command uses Ansible Galaxy at https://galaxy.ansible.com/ to download Ansible Content Collections. Yu might not want to use this command, preferring automation hub or your own private automation hub. Alternatively, you might want to try automation hub first, and then try Ansible Galaxy.

You can configure the sources that ansible-galaxy uses to get Ansible Content Collections in your Ansible project's ansible.cfg file. The relevant parts of that file might look like the following example:
```INI
[galaxy]
server_list = automation_hub, galaxy

[galaxy_server.automation_hub]
url=https://console.redhat.com/api/automation-hub/
auth_url=https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token 3
token=eyJh...Jf0o

[galaxy_server.galaxy]
url=https://galaxy.ansible.com/
```

# Using Resources from Ansible Content Collections
After you install an Ansible Content Collection in your Ansible project, you can use it with that project's Ansible Playbooks.

You can use the ansible-navigator collections command in your Ansible project directory to list all the collections that are installed in your automation execution environment. This includes the Ansible Content Collections in your project's collections subdirectory.

You can select the line number of the collection in the interactive mode of ansible-navigator to view its contents. Then, you can select the line number of a module, role, or other plug-in to see its documentation. You can also use other tools, such as ansible-navigator doc, with the FQCN of a module to view that module's documentation.

The following playbook invokes the **mysql_user** module from the **community.mysql** collection for a task.

```YAML
---
- name: Create the operator1 user in the test database
  hosts: db.example.com

  tasks:
    - name: Ensure the operator1 database user is defined
      community.mysql.mysql_user:
        name: operator1
        password: Secret0451
        priv: '.:ALL'
        state: present
```