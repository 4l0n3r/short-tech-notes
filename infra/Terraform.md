# TERRAFORM

## Provider Block

- Declares cloud provider information.
- Multiple provider blocks can be created using the alias concept for deploying in different regions.
- Can use input values but cannot reference resources.

## Terraform Block

- Configures some behaviors of Terraform itself.
- Cannot use input values and cannot reference resources.

## Provision Infrastructure

- Using AWS Console.
- Coding in Python, Bash.
- Terraform:
  - Multi providers.
- Ansible using YAML.

## Terraform State

- **Current state**: Represents the actual infrastructure.
- **Code**: Represents the desired state.

### Resource Example

```hcl
resource "local_file" "pet" {
  filename = "path"
  content  = "I love pets"
}
```

- Terraform follows an immutable infrastructure approach (`-/+` destroying and creating new resources).

## Terraform Commands

- `terraform init`: Downloads plugins for the provider.
- `terraform validate`: Validates configuration files.
- `terraform fmt`: Formats configuration files.
- `terraform plan -out file`: Creates an execution plan.
- `terraform apply file`: Applies the execution plan.
- `terraform show`: Displays the current state.
- `terraform show -json`: Displays the state in JSON format.
- `terraform output`: Displays output values.
- `terraform refresh`: Updates the state file without changing resources.
- `terraform state list`: Lists all resources.
- `terraform state show resource_address`: Shows resource details.
- `terraform state mv old_address new_address`: Moves resources.
- `terraform state pull`: Pulls the state file.
- `terraform state rm resource_address`: Removes a resource.

## Variables

### Types

- **Primitive Types**:
  - `string`
  - `integer`
  - `bool`
- **Complex Types**:
  - `list`
  - `tuple` (different types)
  - `set`
  - `map`
  - `object` (different types)

### Usage

- Type constraints can be used to check the type during the `plan` command.

### Input Methods

- Command-line arguments.
- Environment variables.
- `terraform.tfvars` or `*.auto.tfvars`.
- `-var` or `-var-file` options.

## Resource Attributes

- Implicit dependency: One resource's attribute depends on another.
- Explicit dependency: Use `depends_on`.

## Terraform State

- **Metadata**: Tracks dependencies while deleting dependent resources.
- **Performance**: Avoids fetching the state of every resource from providers.
- **Collaboration**: Stores all resource information securely.
- **Notes**:
  - Includes sensitive information like initial database passwords.
  - Should not be edited manually; use Terraform commands.

## Lifecycle

- `create_before_destroy`: Ensures new resource creation before destroying the old one.
- `prevent_destroy`: Prevents resource destruction.
- `ignore_changes`: Ignores specific attribute changes.

## Data Sources

- Used to read existing information.

### Example

```hcl
data "aws_ami" "example" {
  most_recent = true
  owners      = ["self"]
}
```

## Count and For-Each

### Count

- Used for creating multiple resources.

```hcl
filename = var.filename[count.index]
count    = length(var.filenames)
```

### For-Each

- Used for `set` and `map` types.

```hcl
for_each = toset(var.filenames)
filename = each.value
```

## Lookup

- Retrieves a value from a map.

```hcl
lookup(map, key)
```

## Provider Configuration

- Specify `region`, `access_key`, and `secret_key`.
- Can use `.aws/credentials` file or environment variables.

## Policies

- Use `<<EOF` to define inline policies.

```hcl
policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": []
}
EOF
```

## Remote State

### Configuration

```hcl
terraform {
  backend "s3" {
    bucket = "example-bucket"
    key    = "terraform.tfstate"
    region = "us-east-1"
    dynamodb_table = "terraform-lock"
  }
}
```

## Provisioners

### File Uploads

```hcl
provisioner "file" {
  source      = "local_path"
  destination = "remote_path"
}
```

### SSH Connection

```hcl
connection {
  user        = var.INSTANCE_USERNAME
  private_key = file(var.PATH_TO_PRIVATE_KEY)
}
```

### Remote Execution

```hcl
provisioner "remote-exec" {
  inline = ["command-1", "command-2"]
}
```

## Outputs

```hcl
output "instance_ip" {
  value = aws_instance.example.public_ip
}
```

## Modules

- Reusable configurations.
- Sources can be local or remote (e.g., Git).

```hcl
module "example" {
  source = "path/to/module"
}
```

## Debugging

- Set log levels:

```bash
export TF_LOG=TRACE
export TF_LOG_PATH=/path/to/logfile
```

## Import

- Import existing resources:

```bash
terraform import resource_type.resource_name resource_id
```

---

# Terragrunt

## Overview

- A wrapper around Terraform to make code DRY.

## Use Cases

- Sharing `provider.tf` and `backend.tf` across environments.
- Managing common variables.
- Handling dependencies effectively.

## Setup

- Root folder contains a `terragrunt.hcl` file.
- Subfolders include configuration:

```hcl
include "root" {
  path   = find_in_parent_folders()
  expose = true
}
```

---

# Software Provisioning

## Methods

1. **Creating AMI with Packer**.
2. **Using standard AMI and installing software with Ansible**.

## File Uploads

```hcl
provisioner "file" {
  source      = "local_path"
  destination = "remote_path"
}
```

## SSH Key Pairs

```hcl
resource "aws_key_pair" "example" {
  key_name   = "mykey"
  public_key = file(var.PATH_TO_PUBLIC_KEY)
}
```

## Remote State Management

- **Backup**:

```hcl
terraform {
  backend "s3" {
    bucket = "example-bucket"
    key    = "terraform.tfstate"
    region = "us-east-1"
  }
}
```

- **Restore**:

```hcl
data "terraform_remote_state" "example" {
  backend = "s3"
  config = {
    bucket     = "example-bucket"
    key        = "terraform.tfstate"
    region     = var.AWS_REGION
    access_key = "value"
    secret_key = "value"
  }
}
```

## Commands

- `terraform show`
- `terraform graph`
- `terraform refresh`
