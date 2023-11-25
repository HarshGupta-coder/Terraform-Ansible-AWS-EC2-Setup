# Terraform-Ansible-AWS-EC2-Setup
A comprehensive guide and code repository demonstrating the automated provisioning and configuration of an AWS EC2 instance with Nginx using Terraform for infrastructure management and Ansible for configuration. Simplify your infrastructure deployment process with this easy-to-follow project.

## Provisioning Infrastructure using Terraform and Ansible

### Overview

This project demonstrates the provisioning and configuration of infrastructure resources on AWS using Terraform for resource creation and Ansible for configuration management. The goal is to create an EC2 instance and install Nginx on it.

### Prerequisites

- Ubuntu machine with Terraform and Ansible installed
- AWS account with appropriate permissions
- AWS CLI configured on the local machine

### Steps

1. **AWS Key Pair**

   - Login to the AWS console.
   - Generate a key pair and download it in PEM format.

2. **Terraform Configuration**

   - Write the `main.tf` file with the following specifications:
  
     ```hcl
     locals {
       vpc_id           = "vpc-090d44ff9674390bf"
       subnet_id        = "subnet-00783f62f7d584e6b"
       ssh_user         = "ubuntu"
       key_name         = "ec2-terra-key"
       private_key_path = "~/Downloads/ec2-terra-key.pem"
     }

     provider "aws" {
       region = "us-east-1"
     }

     resource "aws_security_group" "nginx" {
       # ... (security group configuration)
     }

     resource "aws_instance" "nginx" {
       # ... (instance configuration)
     }

     output "nginx_ip" {
       value = aws_instance.nginx.public_ip
     }
     ```

3. **Ansible Configuration**

   - Write the `nginx.yaml` Ansible playbook to install Nginx on the Ubuntu instance:

     ```yaml
     ---
     - name: Configure EC2 instances
       hosts: all
       become: true

       tasks:
         - name: Update package cache
           apt:
             update_cache: yes

         - name: Install nginx
           apt:
             name: nginx
             state: present
     ```

   - Create an `ansible.cfg` file with the following content:

     ```ini
     [defaults]
     host_key_checking = False
     ```

4. **Execution**

   - Run the following commands in the terminal:

     ```bash
     terraform init
     terraform apply
     ```

   - After Terraform applies successfully, check the public IP of the EC2 instance.

   - Run the Ansible playbook using the following command:

     ```bash
     ansible-playbook -i <EC2_PUBLIC_IP>, --private-key ~/Downloads/ec2-terra-key.pem nginx.yaml
     ```

5. **Verification**

   - Access the Nginx welcome page by navigating to the EC2 instance's public IP in a web browser.
     
  ![image](https://github.com/HarshGupta-coder/Terraform-Ansible-AWS-EC2-Setup/assets/54001485/36c607af-669a-454a-87e0-ee8b848266f5)

### Outputs

- The public IP of the provisioned EC2 instance is available as an output.

```bash
Nginx Public IP: <EC2_PUBLIC_IP>
```

### Note

- Ensure that the specified key pair, VPC ID, subnet ID, and AMI ID are correct for your AWS environment.
- Verify that the security group allows SSH (port 22) and HTTP (port 80) traffic.

### Clean-Up

- Run the following command to destroy the provisioned resources:

  ```bash
  terraform destroy
  ```

---

Feel free to customize the provided Terraform and Ansible scripts according to your specific requirements. For more detailed information, refer to the official documentation of [Terraform](https://www.terraform.io/docs/index.html) and [Ansible](https://docs.ansible.com/).
