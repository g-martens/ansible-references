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

# File
## Create
```bash
[user@server]$ ansible-vault create my-secret.yml
New Vault password:
Confirm New Vault Password:
```

## View
```bash
[user@server]$ ansible-vault view my-secret.yml
Vault password:
```

## Edit
```bash
[user@server]$ ansible-vault edit my-secret.yml
Vault password:
```
## Encrypt
```bash
[user@server]$ ansible-vault encrypt file.yml
New Vault password:
Confirm New Vault Password:
Encryption successful
```

## Decrypt
```bash
[user@server]$ ansible-vault decrypt file.yml
Vault password:
Decryption successful
```

# Encrypt/decrypt strings
## Encrypting
instead of encrypting a whole file, you can also encrypt only a string.

```bash
 ansible-vault encrypt_string 'this is an encrypted string' -n this_is_encrypted >> mixed_vars.yml
```

Now you can see the content of mixed_vars.yml
```yaml
---
this_is_encrypted: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          34623938623265396538623965313539306463633132323135633238666433333865613662646637
          3435623761656633656434633631666335313537323566340a333864303363626634303866303430
          33363630376333663738393632633636386332663234386132343964616565376266656163643831
          3531663033376666340a393131333766643465303939653638326561656137623133623830333562
          33663030336632653933343137393333373232663137643038323161383461396437[
```



## Decrypting
Decrypting strings is more difficult than files. For this you can use the debug module

```bash
ansible localhost -m debug -a var='this_is_encrypted' -e "@mixed_vars.yml" --ask-vault-pass

# Output:
Vault password:
localhost | SUCCESS => {
    "this_is_encrypted": "this is an encrypted string"
}

```
