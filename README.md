# Workspaces using Terraform

This repository demonstrates how to use Terraform workspaces to manage multiple AWS environments using a single Terraform configuration. It provisions an EC2 instance with environment-specific values for `dev`, `qa`, `uat`, and `prod`.

## Overview

The project includes:

- A Terraform configuration to create an AWS EC2 instance
- Environment-specific variable files for different workspaces
- A remote S3 backend for storing Terraform state
- Output values for instance details such as public/private IP and DNS

## Project Structure

- `main.tf` - EC2 resource definition
- `variables.tf` - Input variables used by the configuration
- `providers.tf` - AWS provider configuration
- `backend.tf` - Remote S3 backend configuration
- `output.tf` - Outputs for the EC2 instance
- `dev.tfvars` - Variables for development environment
- `qa.tfvars` - Variables for QA environment
- `uat.tfvars` - Variables for UAT environment
- `prod.tfvars` - Variables for production environment

## Prerequisites

Before you run this project, make sure you have:

- Terraform installed
- AWS CLI configured with valid credentials
- A valid AWS VPC subnet ID for each environment
- An S3 bucket created for the Terraform backend state
- Permissions to create EC2 instances in the selected AWS region

## AWS Configuration

The provider is configured for the `ap-south-1` region:

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

The Terraform state is stored in an S3 backend:

```hcl
terraform {
  backend "s3" {
    bucket = "tf-backend-08"
    key    = "compute/Day-04/Workspace/terraform.tfstate"
    region = "ap-south-1"
  }
}
```

## Initialize Terraform

```bash
terraform init
```

## Create and Select a Workspace

Create or switch to an environment workspace:

```bash
terraform workspace new dev
terraform workspace select dev
```

You can repeat the same command for other environments like `qa`, `uat`, and `prod`.

## Apply the Configuration

For example, to deploy the development environment:

```bash
terraform plan -var-file=dev.tfvars
terraform apply -var-file=dev.tfvars
```

Similarly, for other environments:

```bash
terraform plan -var-file=qa.tfvars
terraform apply -var-file=qa.tfvars
```

```bash
terraform plan -var-file=uat.tfvars
terraform apply -var-file=uat.tfvars
```

```bash
terraform plan -var-file=prod.tfvars
terraform apply -var-file=prod.tfvars
```

## Destroy the Infrastructure

To remove the deployed EC2 instance:

```bash
terraform destroy -var-file=dev.tfvars
```

Replace the environment file as needed for the targeted workspace.

## Output Values

The configuration exposes output values for the EC2 instance:

- Public IP
- Private IP
- Public DNS
- Private DNS
- Instance ID

These can be viewed using:

```bash
terraform output
```

## Notes

This project is intended for learning and hands-on practice with Terraform workspaces and environment isolation in AWS. The same Terraform code is reused across environments with different input values.

## License

This project is provided for educational purposes.
