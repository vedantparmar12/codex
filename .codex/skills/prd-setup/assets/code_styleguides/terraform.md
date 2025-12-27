# Terraform (HCL) Style Guide Summary

This document summarizes key rules and best practices for writing Terraform configurations using HashiCorp Configuration Language (HCL).

## 1. File Structure
- **File Extension:** `.tf` for Terraform files
- **File Organization:**
  - `main.tf` - Main resource definitions
  - `variables.tf` - Input variable declarations
  - `outputs.tf` - Output value definitions
  - `versions.tf` - Terraform and provider version constraints
  - `terraform.tfvars` - Variable value assignments (never commit sensitive data)

## 2. Formatting
- **Indentation:** 2 spaces per level
- **Use `terraform fmt`:** Always run before committing
- **Line Length:** Keep under 120 characters
- **Blank Lines:** Use blank lines to separate logical groups

```hcl
resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = "t2.micro"

  tags = {
    Name        = "web-server"
    Environment = var.environment
  }
}
```

## 3. Naming Conventions
- **Resources:** `snake_case` (e.g., `aws_security_group.web_server`)
- **Variables:** `snake_case` (e.g., `instance_type`)
- **Outputs:** `snake_case` (e.g., `vpc_id`)
- **Locals:** `snake_case` (e.g., `common_tags`)
- **Files:** `snake_case.tf` (e.g., `security_groups.tf`)

## 4. Variables
- **Always Include Description:** Every variable should have a description
- **Use Type Constraints:** Specify variable types explicitly
- **Default Values:** Provide sensible defaults when appropriate
- **Validation:** Use validation blocks for complex requirements

```hcl
variable "instance_type" {
  description = "EC2 instance type for web servers"
  type        = string
  default     = "t2.micro"

  validation {
    condition     = contains(["t2.micro", "t2.small", "t2.medium"], var.instance_type)
    error_message = "Instance type must be t2.micro, t2.small, or t2.medium."
  }
}
```

## 5. Resources
- **Unique Names:** Each resource should have a unique, descriptive name
- **Use `for_each` over `count`:** When creating multiple similar resources
- **Tags:** Always tag resources with at least Name and Environment
- **Lifecycle Rules:** Use lifecycle blocks to prevent accidental destruction

```hcl
resource "aws_s3_bucket" "data" {
  for_each = toset(var.bucket_names)

  bucket = each.value

  lifecycle {
    prevent_destroy = true
  }

  tags = merge(
    local.common_tags,
    {
      Name = each.value
    }
  )
}
```

## 6. Modules
- **Single Purpose:** Each module should have one clear purpose
- **README:** Include README.md with usage examples
- **Version Pinning:** Pin module versions in production
- **Input/Output Documentation:** Document all variables and outputs

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 3.0"

  name = var.vpc_name
  cidr = var.vpc_cidr
}
```

## 7. State Management
- **Remote Backend:** Always use remote state for team environments
- **State Locking:** Enable state locking (e.g., DynamoDB for S3 backend)
- **Sensitive Data:** Mark sensitive outputs appropriately

```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}
```

## 8. Best Practices
- **Version Constraints:** Pin provider and module versions
- **Data Sources:** Use data sources to reference existing resources
- **Locals:** Use local values for repeated expressions
- **Comments:** Use `#` for single-line comments, document complex logic
- **Secrets:** Never hardcode secrets, use secret management systems
- **Workspaces:** Use workspaces for environment separation when appropriate

```hcl
terraform {
  required_version = ">= 1.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}
```

## 9. Output Values
- **Description:** Always include descriptions
- **Sensitive:** Mark outputs containing secrets as sensitive

```hcl
output "vpc_id" {
  description = "ID of the VPC"
  value       = aws_vpc.main.id
}

output "database_password" {
  description = "Database administrator password"
  value       = aws_db_instance.main.password
  sensitive   = true
}
```

**BE CONSISTENT.** Follow the HashiCorp style conventions and use `terraform fmt` regularly.

*Source: [Terraform Style Guide](https://www.terraform.io/docs/language/syntax/style.html)*
