## Cornerstone Collection
-------------------------

This collection leverages the cornerstone and pre-req-install role to provision servers in cloud-based environments. Please look into each role for further information

## Playbook Ordering
-------------------

```
[oezeakac@oezeakac-thinkpadt14gen3 cornerstone-playbooks]$ tree
.
├── pre-req-run.yml
├── run.yml
└── vars
    └── main.yml
```

- Have a separate directory (e.g. Cornerstone-Playbooks) that contains the needed playbooks 
- Within this directory it contains another directory called vars, which hosts a global vars file called main.yml
- main.yml contains variables integral to the provisioning process across the 2 roles. Look down below for the example vars files for each cloud platform

## Example playbooks
--------------------


## pre-req-run.yml 
------------------

```
---
- name: Build Instance  
  hosts: localhost
  vars_files:
    - vars/main.yml

  tasks:
    - include_role:
        name: cornerstone.cornerstone.pre_req_install
```

## run.yml
----------

```
---
- name: Build Instance  
  hosts: localhost
  vars_files:
    - vars/main.yml

  tasks:
    - include_role:
        name: cornerstone.cornerstone.Cornerstone
```

## AWS vars file example

```yaml
---
aws_system_user:
aws_profile:             #Ex: profile1
aws_access_key:          #access-key
aws_secret_key:          # secret-key
aws_region:              #Ex: "eu-west-1"
aws_format:              #Ex: table
foundation:              #Ex: "aws" 
key_name:                #Ex: aws_keypair
key_file:                #Ex: /home/oezeakac/labs/Ansible/cornerstone-playbooks/aws_keypair #non-root dir
ansible_python_interpreter: #Ex: /usr/bin/python3.11
cornerstone_prefix:         #Ex: cs
cornerstone_ssh_admin_username: #Ex: rhadmin
cornerstone_ssh_admin_pubkey: #Ex: rsa.pub
cornerstone_aws_ssh_key_name: #Ex: "{{ key_name }}"
cornerstone_aws_profile: #Ex: default
cornerstone_ssh_user: #Ex: ec2-user
cornerstone_ssh_key_path: #Ex: "ssh_key.pem"
cornerstone_platform: #Ex: aws
cornerstone_location: #Ex: eu-west-1

cornerstone_sg:
  - name: "testworkshop-sg"
    description: Security group for aws
    region: "{{ cornerstone_location }}"
    rules:
      - proto: tcp
        from_port: 22
        to_port: 22
        group_name: ""
        cidr_ip: 0.0.0.0/0
        rule_desc: "allowSSHin_all"
      - proto: tcp
        from_port: 443
        to_port: 443
        group_name: ""
        cidr_ip: 0.0.0.0/0
        rule_desc: "allowHttpsin_all"
      - proto: all
        from_port: ""
        to_port: ""
        group_name: "testworkshop-sg"
        cidr_ip: 0.0.0.0/0
        rule_desc: "allowAllfromSelf"

vm_state: present

guests:
  testsystem1:
      cornerstone_vm_state:            #Ex: "{{vm_state}}"
      cornerstone_platform:            #Ex: aws
      cornerstone_tag_purpose:         #Ex: "Testing"
      cornerstone_tag_role:            #Ex: "testsystem"
      cornerstone_vm_name:             #Ex: testsystem
      cornerstone_location:            #Ex: eu-west-1
      cornerstone_vm_aws_az:           #Ex: eu-west-1a
      cornerstone_vm_flavour:          #Ex: t3.2xlarge
      cornerstone_vm_aws_ami:          #Ex: ami-0b04ce5d876a9ba29
      cornerstone_vm_aws_sg:           #Ex:  obitestworkshop-sg
      cornerstone_virtual_network_name: #Ex: "{{ cornerstone_prefix }}vnet"
      cornerstone_virtual_network_cidr: #Ex: "10.1.0.0/16"
      cornerstone_subnet_name:          #Ex: "{{ cornerstone_prefix }}subnet"
      cornerstone_public_private_ip:    #Ex: public
      cornerstone_vm_private_ip:              
      cornerstone_vm_assign_public_ip:  #Ex: yes
      cornerstone_vm_public_ip:         #Ex: 63.34.15.95
      cornerstone_publicip_allocation_method: #Ex: Dynamic
      cornerstone_publicip_domain_name: #Ex: null
      cornerstone_vm_os_disk_size:      #Ex: 10
      cornerstone_vm_data_disk:         #Ex: false
      cornerstone_vm_data_disk_device_name: #Ex: "/dev/xvdb"
      cornerstone_aws_vm_data_disk_managed: #Ex: "gp2"
      cornerstone_vm_data_disk_size:        #Ex: "50"

## duplicate the testsystem config if you want to create more instances i.e testsystem3 and its settings and so forth 
```



## Libvrt vars file example 

```yaml
ssh_key_name:  # Define ssh key name e.g. "key"
ssh_role_dir:  # Define directory to store ssh key e.g. ".ssh"
cornerstone_prefix: cs
cornerstone_ssh_user: root
cornerstone_ssh_key_path: # Give a path to ssh key or it will just default to the root user ssh key
foundation: "libvrt" 
cornerstone_platform: libvirt

vm_state: present 
vm_ip: #Ex:192.168.122.134

guests:
  rhel8-basicvm01:
    cornerstone_platform: libvirt
    cornerstone_vm_location: '/home/oezeakac/VirtualMachines/' # This where the qemu vm lives with the qemu image file so when creating in qemu gui make sure they live here
    cornerstone_working_dir:                   #Ex:'/tmp/'
    cornerstone_vm_libvirt_template:           #Ex:'rhel9.3' 
    cornerstone_vm_libvirt_file_type:          #Ex:'qcow2'
    cornerstone_vm_libvirt_vmtype:
    cornerstone_vm_libvirt_vmos:               #Ex:'linux'
    cornerstone_vm_subnet:                     #Ex:24
    cornerstone_vm_gateway:                    #Ex:192.168.122.1
    cornerstone_vm_dns1:                       #Ex:192.168.122.1
    cornerstone_vm_dns2:                       #Ex:8.8.8.8 
    cornerstone_vm_name:                       #Ex:'rhel8-basicvm02'
    cornerstone_vm_state: "{{vm_state}}" 
    cornerstone_public_private_domain_name:    #Ex:'obilab.local'
    cornerstone_vm_os_disk_size:      #Ex:20
    cornerstone_vm_data_disk:         #Ex:false
    cornerstone_vm_data_disk_size:    #Ex:10
    cornerstone_vm_data_disk_dev:     #Ex:"vdb"
    cornerstone_vm_libvirt_vmmem:     #Ex:2048
    cornerstone_vm_libvirt_vmcores:   #Ex:2
    cornerstone_vm_ip: "{{ vm_ip }}"
    cornerstone_tag_purpose:          #Ex:"basicvm"
    cornerstone_tag_role:             #Ex:"testing"
    cornerstone_virtual_network_name: #Ex:default
    cornerstone_python_interpreter:   #Ex:"/bin/python"
    cornerstone_vm_extra_nics:        #Ex:0
    cornerstone_vm_netname:           #Ex:default  
```

## Azure vars file example 

For Azure you cannot use the guest layout yet. The task will only create one instance at a time.
In addition to this to avoid a dependancy mismatch when installing the azure.azcollection use an ee environment: https://github.com/oezeakachi/Cornerstone-EE/blob/main  while running the run.yml playbook

```yaml
---
ssh_key_name:             #Ex: "obi3" # Define ssh key name
ssh_role_dir:             #Ex: "/home/oezeakac/.ssh" # Define directory to store ssh key
foundation:               #Ex: "azure" 
cornerstone_prefix:       #Ex: cs
cornerstone_platform:     #Ex: azure
cornerstone_ssh_user:     #Ex: azureuser
cornerstone_ssh_key_path: #Ex:"/home/oezeakac/.ssh/{{ ssh_key_name }}"
cornerstone_ssh_admin_username: #Ex: azureadmin
cornerstone_ssh_admin_pubkey:
ansible_python_interpreter:     #Ex: /usr/bin/python
az_subscription_id:             # Subscription ID
az_client_id:                   # Client ID
az_secret:                      # Secret
az_tenant:                      # tenant value

vm_state: present

guests:
  basicvm01:
    cornerstone_platform:                  #Ex:azure
    cornerstone_azure_vm_disk_managed:     #Ex: Standard_LRS
    cornerstone_working_dir:               #Ex: '/tmp/'
    cornerstone_vm_state:                  #Ex: "{{vm_state}}"
    cornerstone_vm_name:                   #Ex: "bastion"
    cornerstone_storage_type:              #Ex: StandardSSD_LRS
    cornerstone_location:                  #Ex: uksouth
    cornerstone_azure_resource_group:      #Ex: "{{ cornerstone_prefix }}-rg"
    cornerstone_virtual_network_name:      #Ex: "{{ cornerstone_prefix }}-vnet"
    cornerstone_subnet_name:               #Ex: "{{ cornerstone_prefix }}subnet"
    cornerstone_azure_subnet_cidr:         #Ex: "10.1.0.0/20"
    cornerstone_vm_assign_public_ip:       #Ex: false
    cornerstone_vm_public_ip:
    cornerstone_public_private_ip:          #Ex: public
    cornerstone_publicip_allocation_method: #Ex: Dynamic
    cornerstone_publicip_domain_name: null
    cornerstone_vm_flavour:                 #Ex: Standard_D2s_v3
    cornerstone_vm_azure_image:             #Ex: rhel8-gm
    cornerstone_vm_image_offer:             #Ex: "RHEL"
    cornerstone_vm_image_publisher:         #Ex: "RedHat"
    cornerstone_vm_image_sku:               #Ex: "87-gen2"
    cornerstone_vm_image_version:           #Ex: "latest"
    cornerstone_vm_os_disk_size:            #Ex: 32
    cornerstone_vm_data_disk:               #Ex: false
    cornerstone_tag_purpose:                #Ex: "bastion"
    cornerstone_tag_role:                   #Ex: "testing"
```

## GCP vars file example 

```yaml
ssh_key_name:           #Ex: "obi4" # Define ssh key name
ssh_role_dir:           #Ex: "/home/oezeakac/.ssh" # Define directory to store ssh key
cornerstone_prefix:     #Ex: cs
cornerstone_platform:   #Ex: gcp
foundation:             #Ex: "gcp" 
cornerstone_ssh_user:   #Ex: obi 
cornerstone_ssh_key_path:   #Ex: /home/oezeakac/.ssh" ## file_private_to_key
ansible_python_interpreter: #Ex: /usr/bin/python3
cornerstone_gcp_project:    #Ex: openenv-d2p2t
cornerstone_gcp_auth_kind:  #Ex: "serviceaccount"
cornerstone_service_account_file: #Ex: /home/oezeakac/openenv-prbtd-6d274028b755.json

cornerstone_virtual_network_name:  #Ex: "{{ cornerstone_prefix }}-vnet"
cornerstone_location:              #Ex: europe-west2
cornerstone_gcp_zone:              #Ex: europe-west2-a
cornerstone_virtual_network_cidr:  #Ex: 172.16.0.0/28
cornerstone_gcp_use_serviceaccount: true

cornerstone_ssh_admin_username: root user
cornerstone_ssh_admin_pubkey:       #<generate pub ssh key and sub in here>

ocp4_platform: gcp

cornerstone_sg:
  - name: "cs-sg"
    description: "firewall rules"
    rules:
      - proto: tcp
        from_port: 22

vm_state: present

guests:
  bastion:
    cornerstone_platform:               #Ex: gcp
    cornerstone_gcp_project:            #Ex: openenv-d2p2t
    cornerstone_gcp_auth_kind:          #Ex: "serviceaccount"
    cornerstone_service_account_file: ""
    cornerstone_working_dir:            #Ex:'/tmp/'
    cornerstone_vm_state:               #Ex:"{{vm_state}}"
    cornerstone_vm_name:                #Ex: "bastion"
    cornerstone_location:               #Ex:   europe-west2
    cornerstone_gcp_zone:               #Ex:   europe-west2-a
    cornerstone_virtual_network_name:   #Ex: "{{ cornerstone_prefix }}-vnet"
    cornerstone_virtual_network_cidr:   #Ex: 172.16.0.0/28
    cornerstone_subnet_name:            #Ex: "{{ cornerstone_prefix }}subnet"
    cornerstone_vm_flavour:             #Ex: e2-medium
    cornerstone_vm_gcp_source_image:    #Ex:"filepath"
    cornerstone_vm_os_disk_size:        #Ex: 30
    cornerstone_tag_purpose:            #Ex: "bastion"
    cornerstone_tag_role:               #Ex: "testing"
```