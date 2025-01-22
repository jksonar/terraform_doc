# 1. What is terraform module and how to use it?

In **Terraform**, a **module** is a container for multiple resources that are used together.
A module is a way to organize and reuse your Terraform code by grouping related infrastructure components into a single,
reusable unit. This helps in managing complexity, maintaining consistency, and improving the scalability of your infrastructure code.

---

## 1. **Key Points about Modules**

1. **Every Terraform configuration is a Module**

   - The root module is the directory where you run Terraform commands.
   - Additional modules can be nested inside the root module.

2. **Reusable**

   - Modules can be used to define reusable and standard patterns for creating infrastructure.

3. **Composability**

   - You can combine multiple smaller modules to form a larger infrastructure configuration.

4. **Encapsulation**
   - A module hides the complexity of resource definitions, exposing only the necessary variables and outputs.

---

## 2. **Components of a Module**

A module typically consists of:

1. **`main.tf`**
   - Defines the resources and logic for the module.
2. **`variables.tf`**
   - Defines the input variables required by the module.
3. **`outputs.tf`**
   - Specifies the output values that are returned by the module.
4. **Optional Files**
   - `README.md`: Documentation for the module.
   - `providers.tf`: Specifies providers for the module.
   - Other `.tf` files for organizing additional configurations.

---

## 3. **Why Use Modules?**

- **Simplify Complex Infrastructure**  
  Breaking large configurations into smaller, reusable modules makes it easier to manage and understand.
- **Encourage Code Reuse**  
  Modules can be used across different environments or projects, reducing redundancy.

- **Improve Collaboration**  
  Teams can share modules as standardized building blocks.

- **Centralize Management**  
  Updates to a module can propagate across all its usages.

---

# 2. **How to Create and Use Modules**

#### **1. Create a Module**

- Create a directory for the module:

  ```bash
  my-module/
  ├── main.tf
  ├── variables.tf
  ├── outputs.tf
  ```

- **Example**: Module to create an AWS EC2 instance.
  **`main.tf`**

  ```hcl
  resource "aws_instance" "example" {
    ami           = var.ami
    instance_type = var.instance_type
    tags = {
      Name = var.instance_name
    }
  }
  ```

  **`variables.tf`**

  ```hcl
  variable "ami" {
    description = "The AMI to use for the instance"
    type        = string
  }

  variable "instance_type" {
    description = "The type of instance to use"
    type        = string
    default     = "t2.micro"
  }

  variable "instance_name" {
    description = "The name of the instance"
    type        = string
  }
  ```

  **`outputs.tf`**

  ```hcl
  output "instance_id" {
    value = aws_instance.example.id
  }

  output "public_ip" {
    value = aws_instance.example.public_ip
  }
  ```

---

#### **2. Use a Module**

To use the module, reference it in another Terraform configuration:

```hcl
module "ec2_instance" {
  source         = "./my-module" # Path to the module directory
  ami            = "ami-12345678"
  instance_type  = "t2.micro"
  instance_name  = "example-instance"
}
```

- Run the usual Terraform commands:
  ```bash
  terraform init
  terraform plan
  terraform apply
  ```

---

### **Sources for Modules**

Modules can be sourced from different locations:

1. **Local Path**

   ```hcl
   source = "./path/to/module"
   ```

2. **Terraform Registry**

   ```hcl
   source = "terraform-aws-modules/vpc/aws"
   ```

3. **Git Repository**

   ```hcl
   source = "git::https://github.com/example/repo.git"
   ```

4. **HTTP URL**

   ```hcl
   source = "https://example.com/modules/vpc.zip"
   ```

5. **S3 Bucket**
   ```hcl
   source = "s3::https://my-bucket.s3.amazonaws.com/modules/module.zip"
   ```

---

# 3. **Best Practices for Modules**

1. **Use Variables Judiciously**  
   Define variables for inputs to make the module flexible.

2. **Output Only What’s Necessary**  
   Expose only essential information through outputs.

3. **Document Modules**  
   Include a `README.md` file to explain the module’s purpose, variables, and outputs.

4. **Version Control**  
   Store modules in version-controlled systems (e.g., Git) and use specific versions.

5. **Encapsulate Complexity**  
   Hide resource details, exposing only what users need.

---

### **Example Terraform Registry Module**

The Terraform Registry (https://registry.terraform.io/) hosts public modules.  
Example: AWS VPC Module:

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "3.5.0"

  name = "my-vpc"
  cidr = "10.0.0.0/16"
  azs  = ["us-east-1a", "us-east-1b"]
}
```

# 4. how to use module to setup different environments like dev, uat and prod?

Setting up **different environments** (like `dev`, `uat`, and `prod`) using Terraform modules is a best practice.
It allows you to reuse the same module for multiple environments while customizing configurations for each.
Here's a step-by-step guide:

---

### **1. Create the Module**

The module should define reusable resources that can be parameterized for different environments.

#### Example: A Module for an AWS VPC

**Module Structure:**

```plaintext
terraform-modules/
├── vpc/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── README.md
```

**`main.tf`:**

```hcl
resource "aws_vpc" "main" {
  cidr_block           = var.cidr_block
  enable_dns_support   = true
  enable_dns_hostnames = true
  tags = {
    Name = var.environment
  }
}

resource "aws_subnet" "subnet" {
  count                  = var.subnet_count
  vpc_id                 = aws_vpc.main.id
  cidr_block             = element(var.subnet_cidrs, count.index)
  map_public_ip_on_launch = true
  availability_zone      = element(var.availability_zones, count.index)
  tags = {
    Name = "${var.environment}-subnet-${count.index + 1}"
  }
}
```

**`variables.tf`:**

```hcl
variable "cidr_block" {
  description = "The CIDR block for the VPC"
  type        = string
}

variable "subnet_count" {
  description = "The number of subnets to create"
  type        = number
  default     = 2
}

variable "subnet_cidrs" {
  description = "List of CIDR blocks for the subnets"
  type        = list(string)
}

variable "availability_zones" {
  description = "List of availability zones"
  type        = list(string)
}

variable "environment" {
  description = "The name of the environment (e.g., dev, uat, prod)"
  type        = string
}
```

**`outputs.tf`:**

```hcl
output "vpc_id" {
  value = aws_vpc.main.id
}

output "subnet_ids" {
  value = aws_subnet.subnet[*].id
}
```

---

### **2. Define Environment Configurations**

Create a directory for each environment (`dev`, `uat`, `prod`) and provide specific variables for each.

#### Example Directory Structure:

```plaintext
environments/
├── dev/
│   ├── main.tf
│   ├── terraform.tfvars
├── uat/
│   ├── main.tf
│   ├── terraform.tfvars
├── prod/
│   ├── main.tf
│   ├── terraform.tfvars
```

#### **Environment-Specific Configurations**

**`main.tf`:** (Common for all environments)

```hcl
module "vpc" {
  source              = "../../terraform-modules/vpc"
  cidr_block          = var.cidr_block
  subnet_count        = var.subnet_count
  subnet_cidrs        = var.subnet_cidrs
  availability_zones  = var.availability_zones
  environment         = var.environment
}
```

**`terraform.tfvars`:** (Environment-Specific)

- **dev/terraform.tfvars**

  ```hcl
  cidr_block          = "10.0.0.0/16"
  subnet_count        = 2
  subnet_cidrs        = ["10.0.1.0/24", "10.0.2.0/24"]
  availability_zones  = ["us-east-1a", "us-east-1b"]
  environment         = "dev"
  ```

- **uat/terraform.tfvars**

  ```hcl
  cidr_block          = "10.1.0.0/16"
  subnet_count        = 3
  subnet_cidrs        = ["10.1.1.0/24", "10.1.2.0/24", "10.1.3.0/24"]
  availability_zones  = ["us-east-1a", "us-east-1b", "us-east-1c"]
  environment         = "uat"
  ```

- **prod/terraform.tfvars**
  ```hcl
  cidr_block          = "10.2.0.0/16"
  subnet_count        = 4
  subnet_cidrs        = ["10.2.1.0/24", "10.2.2.0/24", "10.2.3.0/24", "10.2.4.0/24"]
  availability_zones  = ["us-east-1a", "us-east-1b", "us-east-1c", "us-east-1d"]
  environment         = "prod"
  ```

---

### **3. Deploy Environment-Specific Infrastructure**

#### Navigate to the Environment Directory

For example, for the `dev` environment:

```bash
cd environments/dev
```

#### Initialize Terraform

```bash
terraform init
```

#### Preview Changes

```bash
terraform plan
```

#### Apply Changes

```bash
terraform apply
```

#### Repeat for Other Environments

Navigate to `uat` or `prod` directories and repeat the steps.

---

### **4. Best Practices for Managing Environments**

1. **Use a Backend for State Files**  
   Store state files in a remote backend (e.g., AWS S3 with state locking via DynamoDB) to avoid conflicts and ensure team collaboration.

2. **Use Version Control**  
   Maintain your module and environment configurations in a version-controlled system like Git.

3. **Avoid Hardcoding Values**  
   Keep all environment-specific values in `terraform.tfvars` files.

4. **Name Resources Dynamically**  
   Include the environment name in resource tags or names for easy identification.

5. **Automate with CI/CD**  
   Automate environment deployments using CI/CD pipelines for consistency.

---
