Pre-Req-Install
=========

Installs required software dependables that the cornestone role

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

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

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
