```


 ___ ___   ___   ___    __ __  _        ___  _____
|   |   | /   \ |   \  |  |  || |      /  _]/ ___/
| _   _ ||     ||    \ |  |  || |     /  [_(   \_ 
|  \_/  ||  O  ||  D  ||  |  || |___ |    _]\__  |
|   |   ||     ||     ||  :  ||     ||   [_ /  \ |
|   |   ||     ||     ||     ||     ||     |\    |
|___|___| \___/ |_____| \__,_||_____||_____| \___|
                                                  


```
On this page, you can find some modules and some tips. You can also find the documentation of each module on the Ansible Documentation.
# Deploying Files to Managed Hosts
Below you can find some modules, which you can use to copy or place files to the Managed Hosts.
## ansible.builtin.file
### Create file with permissions
Make sure an file exist and have te permissions set:
```YAML
- name: Touch a file and set permissions
  ansible.builtin.file:
    path: /path/to/file
    owner: user1
    group: group1
    mode: 0640
    state: touch
```
### Change file attribute
Fot example, set an file with the selinux context
```YAML
- name: Make sure a file does not exist on managed hosts
  ansible.builtin.file:
    dest: /path/to/file
    state: absent
```

### Remove an file
With the snippet below you an remove an file on the remote Managed Hosts
```YAML
- name: SELinux type is set to samba_share_t
  ansible.builtin.file:
    path: /path/to/samba_file
    setype: samba_share_t
```

##  ansible.builtin.copy
With the copy module, you can copy files from the ansible working directory to te managed hosts:
```YAML
- name: Copy a file to managed hosts
  ansible.builtin.copy:
    src: file
    dest: /path/to/file
```
##  ansible.builtin.fetch
With fetch, you can retrieve files from thr remote host, to the ansible working directory
```YAML
- name: Retrieve SSH key from reference host
  ansible.builtin.fetch:
    src: /home/{{ user }}/.ssh/id_rsa.pub
    dest: files/keys/{{ user }}.pub
```

## ansible.posix.synchronize
this module is a wrapper around the rsync tool. for this module rsync need te be installed on both the machines

The following example synchronizes a file located in the Ansible working directory to the managed hosts:
```YAML
- name: synchronize local file to remote files
  ansible.posix.synchronize:
    src: file
    dest: /path/to/file
```


# Verify if a file or directory Exist
## ansible.builtin.stat
with the stat module, you can do a lot of things:
- Check if an file exist
- Check if an folder exist
- Get the md5 checksum of a file

### Check the checksum of a file
```YAML
- name: Verify the checksum of a file
  ansible.builtin.stat:
    path: /path/to/file
    checksum_algorithm: md5
  register: result

- ansible.builtin.debug
    msg: "The checksum of the file is {{ result.stat.checksum }}"
```

# Find/replace add lines in file
With the modules below, you can change the content of the files from the Managed Hosts
## ansible.builtin.lineinfile
With this module, you can add or edit a single line to a file on the Managed Hosts
```YAML
- name: Add a line of text to a file
  ansible.builtin.lineinfile:
    path: /path/to/file
    line: 'Add this line to the file'
    state: present
```


## ansible.builtin.blockinfile
To add a block of text to an existing file, you can use this module.

```YAML
- name: Add additional lines to a file
  ansible.builtin.blockinfile:
    path: /path/to/file
    block: |
      First line in the additional block of text
      Second line in the additional block of text
    state: present
```
