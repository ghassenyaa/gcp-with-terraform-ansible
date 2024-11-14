# GCP VM Deployment with Terraform and Ansible

This guide explains how to set up a virtual machine on Google Cloud Platform (GCP) using Terraform and configure it with Ansible.

## Overview

1. **Terraform** is used to create a GCP VM instance and set up an SSH firewall rule.
2. **Ansible** is used to configure the VM by installing required packages and setting up services.

## Prerequisites

- **Terraform** installed
- **Ansible** installed
- GCP project with billing enabled
- SSH key pair (usually located at `~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub`)
- Google Cloud SDK configured to authenticate with your GCP account

## Step 1: Terraform Setup

1. **Update the Terraform file** (`main.tf`):

   - In the `provider` block, replace `"your-project-id"` with your actual GCP project ID.
   - In the `metadata` section of the `google_compute_instance` resource, replace `"your-username"` with your username.

2. **Initialize Terraform**:

   - Run the following command to initialize Terraform and download the required provider plugins:
     ```bash
     terraform init
     ```

3. **Apply the Terraform configuration**:

   - Run the following command to create the VM instance. Review the proposed changes, and type `yes` to confirm and proceed:
     ```bash
     terraform apply
     ```

4. **Copy the Instance IP**:

   - After the process completes, Terraform will output the IP address of the created VM instance. Note this IP address, as it will be used in the Ansible setup.

## Step 2: Ansible Setup

1. **Navigate to the Ansible folder**:
   ```bash
   cd path/to/ansible

2. **Update the Inventory File (`hosts`)**

 . Replace `${instance_ip}` with the IP address output by Terraform.
 . Replace `"your-username"` with your local username.

Example `hosts` file:
```ini
[gcp_instances]
terraform-instance ansible_host=<instance_ip> ansible_user=your-username ansible_ssh_private_key_file=~/.ssh/id_rsa



3. **Run the Ansible Playbook**
 
   - Execute the playbook to configure the VM:
     ```bash
     ansible-playbook -i hosts playbook.yml
     ```


This command will use the hosts inventory file to define the target VM and execute the tasks specified in the playbook.yml file to install and configure the necessary software.

## Verification

After running the Ansible playbook, you can verify that:

1. NGINX is installed and running on the VM instance.
2. Additional packages specified in playbook.yml are installed.
3. To check NGINX, open your browser and navigate to the instance's IP address (http://<instance_ip>). You should see the default NGINX welcome page if the setup was successful.


## Cleaning Up

To delete the resources created by Terraform, run the following command in the Terraform directory:

```bash
terraform destroy
```

You will be prompted to confirm the destruction of the resources. Type yes to remove all created resources.


## Troubleshooting

1. SSH Connection Errors: Ensure your SSH key is correctly specified in the hosts file, and the VM has SSH access enabled. You may also need to confirm the firewall rules allow SSH connections.

2. Permission Errors: Ensure your Google Cloud account has the required permissions to create resources in the specified project.

## Project Structure

The directory structure is as follows:

1. terraform/: Contains the Terraform configuration to create a GCP VM and firewall rule.
2. ansible/: Contains the Ansible inventory file (hosts) and playbook (playbook.yml) to configure the VM.


Here’s an example of the folder structure:

```bash

gcp-with-terraform-ansible/
├── ansible/
│   ├── inventory.yml
│   └── ansible.yml
└── terraform/
    ├── main.tf
    └── vm_instance.tf
```

##  Additional Resources

This guide should help you set up and manage your GCP VM instance using Terraform and Ansible. For more information, refer to the official documentation:

1. Terraform Documentation
2. Ansible Documentation

