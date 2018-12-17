# ansible-example-tableau-server
> Ansible Automation for Tableau Server 2018.2+ running on Windows Server

*Note: This is an example project and is not production-ready or supported*

## Overview
This project is intended to be an example of how to automate the installation and upgrade of a multi-node, cloud-based software deployment running on Windows Server. The subject of the example is Tableau Server. The following concepts are shown:
- Provisioning on AWS
- Orchestration
- Command Execution
- Configuration Management
- Patching

## Playbooks
All of the playbooks are easy to run. To run in your specific AWS account you'll need to modify the variables in `inventory.example/group_vars/all/aws.yml`. The variables needed in this file can also be placed in the same file as the `extra_vars` detailed below.

### [`tableau-cluster-provision.yml`](tableau-cluster-provision.yml)
Provision two EC2 instances on AWS for installing Tableau Server.
##### Required Extra Vars
- **`aws_provisioning_password`**
    - Password used when provisioning the Windows VMs on AWS
    - Either provide this through an Ansible Vault encrypted file or with an Ansible Tower credential

##### Example
`ansible-playbook tableau-cluster-provision.yml -i inventory.example -e @secrets.yml`

### [`tableau-cluster-setup.yml`](tableau-cluster-setup.yml)
Orchestrate installation of Tableau Server on the two EC2 instances.
##### Required Extra Vars
- **`aws_provisioning_password`**
    - Password used when provisioning the Windows VMs on AWS
    - Either provide this through an Ansible Vault encrypted file or with an Ansible Tower credential
- **`tableau_local_admin_user`**
    - Name of the local Windows user to create for Tableau administration purposes
- **`tableau_local_admin_pass`**
    - Password of the local Windows user to create for Tableau administration purposes
- **`tableau_content_admin_user`**
    - Name of the initial Tableau admin user
- **`tableau_content_admin_pass`**
    - Password of the initial Tableau admin user
- **`tableau_product_keys`**
    - A list of one or more product keys to enable on the Tableau nodes 

##### Example
`ansible-playbook tableau-cluster-setup.yml -i inventory.example -e @secrets.yml`

### [`tableau-cluster-upgrade.yml`](tableau-cluster-upgrade.yml)
Orchestrate the upgrade of Tableau Server on the two EC2 instances.

*Note*: This does not actually perform an upgrade at this time since a Tableau Server trial key does not allow it. it is just an example of the orchestration needed to do the upgrade.

##### Required Extra Vars
- **`aws_provisioning_password`**
    - Password used when provisioning the Windows VMs on AWS
    - Either provide this through an Ansible Vault encrypted file or with an Ansible Tower credential
- **`tableau_local_admin_user`**
    - Name of the local Windows user to create for Tableau administration purposes
- **`tableau_local_admin_pass`**
    - Password of the local Windows user to create for Tableau administration purposes
- **`tableau_content_admin_user`**
    - Name of the initial Tableau admin user
- **`tableau_content_admin_pass`**
    - Password of the initial Tableau admin user
- **`tableau_product_keys`**
    - A list of one or more product keys to enable on the Tableau nodes 

##### Example
`ansible-playbook tableau-cluster-upgrade.yml -i inventory.example -e @secrets.yml`

### [`tableau-uninstall.yml`](tableau-uninstall.yml)

##### Example
`ansible-playbook tableau-uninstall.yml -i inventory.example -e @secrets.yml`

### [`windows_update.yml`](windows-update.yml)

##### Example
`ansible-playbook windows-update.yml -i inventory.example -e @secrets.yml`
