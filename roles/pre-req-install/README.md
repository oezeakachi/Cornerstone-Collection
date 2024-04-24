Pre-Req-Install
=========

Installs required software dependables that power the cornestone role

Requirements
------------

Issue the command `sudo !!` before running the playbook

Role Variables
--------------

```yaml
aws_system_user: open-environment-ndrch-admin
aws_profile: default
aws_access_key: ""
aws_secret_key: ""
aws_region: eu-west-2
aws_format: table
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
