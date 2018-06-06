# Ansible Role for Azure

An Ansible role to configure and deploy :
  - Resource groups
  - Virtual Network
  - Subnet
  - Storage Accounts
  - Availability Sets
  - Load Balancer
  - Windows VMs
  - Linux VMs

It's also possible to Destroy all you have created by using this role.

# Requirements

Azure SDK for Python must be installed on Ansible's venv

pip install azure

Today, you need to use Ansible 2.6.0 Dev 0 if you want to register VMs to Load Balancer

# Role Variables
```YAML
---
customer: ""
instance_name: db
instance_role: ""
instance_ansible_port: "22"
instance_ansible_user: "ansible"
instance_win_initial_password: "P@ssw0rd01"
az_region: "westeurope"
az_location: "{{ az_region }}"
az_resourcegroup_name: "{{ az_environment }}_rg"
az_environment: "dev"
az_storageaccount_name: "{{ customer }}{{ az_environment }}sto01"
az_storageaccount_type: "Standard_LRS"
az_storageaccount_kind: "Storage"
az_storageaccount_tier: "cool"
az_subnet_name: "{{ az_environment }}_subnet"
az_virtualnet_name: "{{ az_environment }}_virtnet"
az_vn_cidr_prefix: "10.0.0.0/8"
az_sub_cidr_prefix: "10.0.0.0/8"
az_vm_nic_name: "{{ az_vm_name}}-nic01"
az_public_ip_name: "{{ az_vm_name }}-publicip"
az_public_ip_method: "Static"
az_vm_name: "{{ az_environment }}-{{ instance_name }}"
az_vm_size: "Standard_DS1_v2"
az_vm_admin_user: "{{ instance_ansible_user }}"
az_vm_admin_pwd: "{{ instance_win_initial_password }}"
az_image_offer: "CentOS"
az_image_publisher: "OpenLogic"
az_image_sku: "7.3"
az_image_version: latest
az_application: "{{ instance_role }}"
az_secgroup_name: "{{ az_environment }}_{{ instance_role }}_secgroup"
az_os_type_vm: "linux"
az_ssh_pwd_enabled: "false"
az_availabilitysets_name: "{{ customer }}_{{ az_environment }}_availsets"
az_lb_public_ip_name: "{{ az_loadbalancer_name }}-publicip"
az_loadbalancer_name: "{{ az_environment }}-lb"
az_loadbalancer_fport: "80"
az_loadbalancer_bport: "80"
az_lb_domain_name_label: "hdnhd"
az_image_name: "w2k12r2_template"
az_image_resource_group: "templates"
az_availabilitysets_sku: "Aligned"
az_managed_disk_type: "Standard_LRS"
```

# Dependencies

None

# Example Playbook

Provision full environment:
```YAML
- name: Setup Azure Environment
  include_role:
    name: nehrman.ansible-role-azure
    tasks_from: setup_env.yml

- name: Deploy Windows Web Apps Instances
  include_role:
    name: nehrman.ansible-role-azure
    tasks_from: create_win_web.yml
  vars:
    instance_name: "webwin0{{ item }}"
    instance_role: "web"
    instance_ansible_port: "5986"
    instance_ansible_user: "testuser"
    instance_win_initial_password: "P@ssw0rd01"
    az_os_type_vm: "Windows"
    az_image_name: "w2k12r2template-image"
    az_image_resource_group: "templates"
  with_sequence: count="{{ az_win_web_count }}"
```

Destroy environment:
```YAML
- name: Delete {{ az_environment }} from Azure
   include_role:
     name: nehrman.azure
     tasks_from: delete_env.yml
```
# License

GPLv3

# Author Information

Nicolas EHRMAN
