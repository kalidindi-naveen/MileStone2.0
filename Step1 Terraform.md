## Why Terraform
- Code can be distributed across multiple files.
- Declare variable is easy
- dry run iseasy & verbose
- Terraform modules
- Terraform workspaces
- Terraform can be used for AWS, Azure, GCP, ...etc

## Terraform 
- Terraform code can vary from Platform to Platform but Logic is Same
- Code Readability is Easy

## Changes Made in GUI ??
- Terraform plan 
- If Changes required then Add them in Terraform Scripts Else These changes will removed Automatically after apply
- Terraform apply
  
# Overview of steps

Create a directory for your Terraform project and create a Terraform configuration file (usually named `main.tf`) in that directory. In this file, you define the AWS provider and resources you want to create. Here's a basic example:

```hcl
   provider "aws" {
     region = "ap-south-1"  # Set your desired AWS region
   }

   resource "aws_instance" "example" {
     ami           = "ami-0a5ac53f63249fba0"  # Specify an appropriate AMI ID
     instance_type = "t2.micro"
   }
```

## Initialize Terraform**

In your terminal, navigate to the directory containing your Terraform configuration files and run:

```
terraform init
```

This command initializes the Terraform working directory, downloading any necessary provider plugins.

## Change Format of Code
Rewrite Terraform configuration files to a canonical format and style.
```
terraform fmt
```
## Validate Terraform Script's

```
terraform validate
```
## Dry Run
Run the following command to see what we are able to create in AWS 
```
terraform plan
```
## Apply the Configuration

Run the following command to create the AWS resources defined in your Terraform configuration:

```
terraform apply
```

Terraform will display a plan of the changes it's going to make. Review the plan and type "yes" when prompted to apply it.

## Verify Resources

After Terraform completes the provisioning process, you can verify the resources created in the AWS Management Console or by using AWS CLI commands.

## Destroy Resources

If you want to remove the resources created by Terraform, you can use the following command:

```
terraform destroy
```

Be cautious when using `terraform destroy` as it will delete resources as specified in your Terraform configuration.