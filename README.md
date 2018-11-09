Role Name
=========

Setup prereqs for Ansible, e.g. create ansible user and groups and setup ssh keys.

This both sets up the Control nodes and the Managed nodes. Tasks are split accordingly into separate task yaml files.

This is run before any other Ansible script and is run interactively, supplying the root password on command line, e.g.

```
# ansible-playbook playbooks/ansible_setup.yml --ask-pass 
```

If a vault is used to hold the 'ansible_setup_keygen_passphrase' variable:
```
# ansible-playbook playbooks/ansible_setup.yml --vault-password=<file vault passwword> --ask-pass 
```

Requirements
------------

Role Variables
--------------

```
| Variable                          | Description                                                       | Default                                 |
| --------                          | -----------                                                       | -------                                 |
| ansible_setup_keygen_passphrase   | (required) Passphrase for ansible user's ssh key                  | ''                                      |
| ansible_setup_keygen_filename     | The filename in which to place the ansible user's ssh private key | '/home/ansible/.ssh/ansible_id_rsa'     |
| ansible_setup_keygen_filename_pub | The filename in which to place the ansible user's ssh public key  | '/home/ansible/.ssh/ansible_id_rsa.pub' |
| ansible_setup_ansible_user        | The Ansible user name                                             | 'ansible'                               |
| ansible_setup_ansible_group       | The Ansible group name                                            | 'ansible'                               |
| ansible_setup_ansible_user_id     | The Ansible user id                                               | '2002'                                  |
| ansible_setup_ansible_group_id    | The Ansible group id                                              | '2002'                                  |

```

Dependencies
------------

Example Playbook
----------------

  - hosts: control-nodes
    gather_facts: false 

    vars_files:
      - vaults/vault.yml # Containing ansible_setup_keygen_passphrase

    tasks:
        - name: "Include ansible_setup role for control nodes"
          include_role:
            name: ansible_setup
            tasks_from: control_node

  - hosts: managed-nodes
    gather_facts: false 

    vars_files:
      - vaults/vault.yml # Containing ansible_setup_keygen_passphrase

    tasks:
        - name: "Include ansible_setup role for managed nodes"
          include_role:
            name: ansible_setup

        
License
-------

BSD

Author Information
------------------

- david.stewart@estafet.com
