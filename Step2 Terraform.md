# Multiple Providers

You can use multiple providers in one single terraform project. For example,


1. Create a providers.tf file in the root directory of your Terraform project.
2. In the providers.tf file, define the AWS and Azure providers. For example:


```
provider "aws" {
  region = "us-east-1"
}

provider "azurerm" {
  subscription_id = "your-azure-subscription-id"
  client_id = "your-azure-client-id"
  client_secret = "your-azure-client-secret"
  tenant_id = "your-azure-tenant-id"
}
```

3. In your other Terraform configuration files, you can then use the aws and azurerm providers to create resources in AWS and Azure, respectively,

```
resource "aws_instance" "example" {
  ami = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
}

resource "azurerm_virtual_machine" "example" {
  name = "example-vm"
  location = "eastus"
  size = "Standard_A1"
}
```

# Multiple Region Implementation in Terraform

You can make use of `alias` keyword to implement multi region infrastructure setup in
terraform.

```
provider "aws" {
  alias = "us-east-1"
  region = "us-east-1"
}

provider "aws" {
  alias = "us-west-2"
  region = "us-west-2"
}

resource "aws_instance" "example" {
  ami = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
  provider = "aws.us-east-1"
}

resource "aws_instance" "example2" {
  ami = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
  provider = "aws.us-west-2"
}
```

# Variables

Input and output variables in Terraform are essential for parameterizing and sharing values within your Terraform configurations and modules. They allow you to make your configurations more dynamic, reusable, and flexible.

## Input Variables

Input variables are used to parameterize your Terraform configurations. They allow you to pass values into your modules or configurations from the outside. Input variables can be defined within a module or at the root level of your configuration. Here's how you define an input variable:

```hcl
variable "example_var" {
  description = "An example input variable"
  type        = string
  default     = "default_value"
}
```

In this example:

- `variable` is used to declare an input variable named `example_var`.
- `description` provides a human-readable description of the variable.
- `type` specifies the data type of the variable (e.g., `string`, `number`, `list`, `map`, etc.).
- `default` provides a default value for the variable, which is optional.

You can then use the input variable within your module or configuration like this:

```hcl
resource "example_resource" "example" {
  name = var.example_var
  # other resource configurations
}
```

You reference the input variable using `var.example_var`.

## Output Variables

Output variables allow you to expose values from your module or configuration, making them available for use in other parts of your Terraform setup. Here's how you define an output variable:

```hcl
output "example_output" {
  description = "An example output variable"
  value       = resource.example_resource.example.id
}
```

In this example:

- `output` is used to declare an output variable named `example_output`.
- `description` provides a description of the output variable.
- `value` specifies the value that you want to expose as an output variable. This value can be a resource attribute, a computed value, or any other expression.

You can reference output variables in the root module or in other modules by using the syntax `module.module_name.output_name`, where `module_name` is the name of the module containing the output variable.

For example, if you have an output variable named `example_output` in a module called `example_module`, you can access it in the root module like this:

```hcl
output "root_output" {
  value = module.example_module.example_output
}
```

This allows you to share data and values between different parts of your Terraform configuration and create more modular and maintainable infrastructure-as-code setups.

# Variables Demo

```hcl

# Define an input variable for the EC2 instance type
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

# Define an input variable for the EC2 instance AMI ID
variable "ami_id" {
  description = "EC2 AMI ID"
  type        = string
}

# Configure the AWS provider using the input variables
provider "aws" {
  region      = "us-east-1"
}

# Create an EC2 instance using the input variables
resource "aws_instance" "example_instance" {
  ami           = var.ami_id
  instance_type = var.instance_type
}

# Define an output variable to expose the public IP address of the EC2 instance
output "public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.example_instance.public_ip
}

```

