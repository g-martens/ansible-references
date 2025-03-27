```

  _____   __  ____     ___ ______  _____
 / ___/  /  ]|    \   /  _]      |/ ___/
(   \_  /  / |  D  ) /  [_|      (   \_ 
 \__  |/  /  |    / |    _]_|  |_|\__  |
 /  \ /   \_ |    \ |   [_  |  |  /  \ |
 \    \     ||  .  \|     | |  |  \    |
  \___|\____||__|\_||_____| |__|   \___|
                                        


```
Ansible might need access to sensitive data, such as passwords or API keys, to configure managed hosts. Normally, this information is stored as plain text in inventory variables or other Ansible files.

In that case, however, any user with access to the Ansible files, or a version control system that stores the Ansible files, would have access to this sensitive data.

This poses an obvious security risk. Ansible Vault, which is included with Ansible, can be used to encrypt and decrypt any data file used
by Ansible.

To use Ansible Vault, use the command-line tool named ansible-vault to create, edit, encrypt, decrypt, and view files.
Ansible Vault can encrypt any data file used by Ansible. This might include inventory variables, included variable files in a playbook, variable files passed as arguments when executing the playbook, or variables defined in Ansible roles

# Create
```bash
[user@server]$ ansible-vault create my-secret.yml
New Vault password:
Confirm New Vault Password:
```

# View
```bash
[user@server]$ ansible-vault view my-secret.yml
Vault password:
```

# Edit
```bash
[user@server]$ ansible-vault edit my-secret.yml
Vault password:
```
# Encrypt
```bash
[user@server]$ ansible-vault encrypt file.yml
New Vault password:
Confirm New Vault Password:
Encryption successful
```

# Decrypt
```bash
[user@server]$ ansible-vault decrypt file.yml
Vault password:
Decryption successful
```