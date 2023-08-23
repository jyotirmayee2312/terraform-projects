
# Project Documentation: AWS Jenkins Launch with Terraform

## Introduction

The **AWS Jenkins Launch with Terraform** project demonstrates the automated deployment of a Jenkins instance on Amazon Web Services (AWS) using Terraform. This project combines the power of Infrastructure as Code (IaC) and automation to streamline the setup of Jenkins in a cloud environment.

## Project Script Explanation

```hcl
# Terraform Backend Configuration
terraform {
  backend "s3" {
    bucket = "jyotirmayee-parida-firstbucket"
    key    = "terraform.tfstate"
    region = "ap-south-1"  # Your desired region
    encrypt = true
  }
}
```

Explanation: This block configures the backend where Terraform will store its state file. The state file contains the current state of your infrastructure. In this example, we're using an S3 bucket to store the state file securely. Replace the bucket name and region with your own settings.

```hcl
# AWS Provider Configuration
provider "aws" {
  region = "ap-south-1"  # Mumbai region
}
```

Explanation: Here, we're configuring the AWS provider that Terraform will use to manage AWS resources. The `region` parameter specifies the AWS region where the resources will be deployed.

```hcl
# RSA Key Generation
resource "tls_private_key" "rsa" {
  algorithm = "RSA"
  rsa_bits  = 4096
}
```

Explanation: This block generates an RSA private key using the TLS provider. The private key is used for secure communication and authentication.

```hcl
# Saving Private Key Locally
resource "local_file" "TF-key" {
  content  = tls_private_key.rsa.private_key_pem
  filename = "tfkey"
}
```

Explanation: This resource saves the generated private key to a local file named "tfkey". This private key will be used for secure access to the EC2 instance.

```hcl
# Creating AWS Key Pair
resource "aws_key_pair" "TF_key" {
  key_name   = "TF_key"
  public_key = tls_private_key.rsa.public_key_openssh
}
```

Explanation: This resource creates an AWS Key Pair named "TF_key" and associates it with the generated public key. This key pair will be used for secure remote access to the EC2 instance.

```hcl
# AWS Security Group
resource "aws_security_group" "example_sg" {
  name_prefix = "example-sg-"
  # Ingress and Egress rules
}
```

Explanation: This block defines an AWS security group named "example_sg". Ingress rules control incoming traffic (SSH, HTTP, HTTPS), while the default egress rule allows all outbound traffic.

```hcl
# AWS EC2 Instance
resource "aws_instance" "example_instance" {
  # Instance details: AMI, instance type, key pair, security groups
  # Connection details for SSH access

  # Provisioner block for software installation
}
```

Explanation: This resource defines the AWS EC2 instance. You'll need to provide details such as the Amazon Machine Image (AMI), instance type, key pair, and security groups. Connection details for SSH access are also included.

```hcl
# Output Public IP
output "instance_ip" {
  value = aws_instance.example_instance.public_ip
}
```

Explanation: This block creates an output variable that displays the public IP address of the EC2 instance after it's provisioned. This makes it easy to access Jenkins through a web browser.

## Usage

1. Clone the repository or copy the provided script into a `.tf` file.
2. Customize the script as needed (e.g., AMI ID, instance type, etc.).
3. Run `terraform init` to initialize the Terraform configuration.
4. Run `terraform apply` to create the resources according to the script.
5. Once the instance is provisioned, the public IP can be accessed to use Jenkins.

## Conclusion

The **AWS Jenkins Launch with Terraform** project showcases the synergy of Infrastructure as Code and automation. By using the provided script, you can effortlessly deploy Jenkins on AWS, streamlining your development and deployment workflows. Customize and extend the script to suit your requirements and further enhance your cloud provisioning skills.

