Pre-Req-Install
=========

Installs required software dependables that power the cornestone role

Requirements
------------

Issue the command `sudo !!` before running the playbook and populate the vars file with required values to proceed with the build

Role Variables
--------------

```yaml
aws_system_user:
aws_profile:
aws_access_key: "" #access-key
aws_secret_key: "" # secret-key
aws_region: 
aws_format: 
foundation: "" # Defines the which packages will be installed be installed e.g aws,libvrt, or azure
key_name: # Defines name of aws keypair (public)
key_file: # Defines file path and name of priuvate aws key
```

Dependencies
------------


Example Playbook
----------------



```yml
- name: Build Instance  
  hosts: localhost
  vars_files:
    - vars/main.yml

  tasks:
    - include_role:
        name: /filepath
```
License
-------

BSD

Author Information
------------------

Obi Ezeakachi - Contributor
