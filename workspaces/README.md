### **What is a Terraform Workspace?**

In Terraform, a **workspace** is a mechanism for managing multiple **state files** within the same configuration directory. Workspaces are typically used to manage different environments (e.g., `dev`, `uat`, `prod`) or multiple instances of the same configuration.

Each workspace has its **own state file**, which means resources in different workspaces are completely isolated. This allows you to share the same Terraform configuration while keeping environments or instances independent.

---

### **Key Features of Workspaces**
1. **Isolated State Files**  
   Each workspace maintains its own `.tfstate` file, avoiding conflicts between environments.

2. **Single Configuration for Multiple Environments**  
   You don't need to duplicate configuration files; instead, use variables to parameterize the configuration.

3. **Default Workspace**  
   Terraform starts with a default workspace named `default`.

---

### **When to Use Workspaces**
- Managing **multiple environments** (e.g., `dev`, `test`, `prod`) within the same configuration.
- Handling **multi-tenancy**, where different teams or clients need isolated instances of the same infrastructure.

> **Note:** Workspaces are not ideal for large-scale environment management. For complex setups, consider using separate directories or modules with different backends.

---

### **Common Workspace Commands**

| Command                                | Description                                                                                   |
|----------------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform workspace list`             | Lists all available workspaces in the current configuration.                                  |
| `terraform workspace show`             | Displays the name of the current workspace.                                                  |
| `terraform workspace new <name>`       | Creates a new workspace with the specified name.                                             |
| `terraform workspace select <name>`    | Switches to an existing workspace.                                                           |
| `terraform workspace delete <name>`    | Deletes an existing workspace (state files are lost).                                        |

---

### **How to Use Workspaces**

#### **1. Initialize the Configuration**
Run the following command to initialize your Terraform configuration:
```bash
terraform init
```

---

#### **2. List Existing Workspaces**
To check available workspaces:
```bash
terraform workspace list
```
Output:
```
* default
  dev
  prod
```
The `*` indicates the current workspace.

---

#### **3. Create a New Workspace**
Create a workspace named `dev`:
```bash
terraform workspace new dev
```

---

#### **4. Switch to a Workspace**
Switch to the `prod` workspace:
```bash
terraform workspace select prod
```

---

#### **5. Use Workspaces in Configuration**
To differentiate resources between workspaces, use the **`terraform.workspace`** variable. For example:

**`main.tf`:**
```hcl
resource "aws_s3_bucket" "example" {
  bucket = "my-bucket-${terraform.workspace}"
  acl    = "private"
}

output "bucket_name" {
  value = aws_s3_bucket.example.bucket
}
```

If you run this configuration in the `dev` workspace, the bucket name will be `my-bucket-dev`. In the `prod` workspace, it will be `my-bucket-prod`.

---

#### **6. Apply Configuration in a Workspace**
Run `terraform plan` and `terraform apply` for a specific workspace:
```bash
terraform plan
terraform apply
```
The changes will be applied to the state file associated with the active workspace.

---

#### **7. Delete a Workspace**
To delete a workspace (except `default`):
```bash
terraform workspace delete dev
```

---

### **Best Practices with Workspaces**
1. **Use Remote State for Collaboration**  
   Store state files in a backend like S3 or Terraform Cloud to ensure team collaboration.

2. **Parameterize with Variables**  
   Combine workspaces with variables to manage environment-specific settings:
   **`variables.tf`**:
   ```hcl
   variable "region" {
     default = {
       dev  = "us-east-1"
       prod = "us-west-2"
     }
   }

   variable "environment" {
     default = terraform.workspace
   }
   ```

   Use it in resources:
   ```hcl
   provider "aws" {
     region = var.region[var.environment]
   }
   ```

3. **Limit Workspace Usage**  
   Use workspaces for simple cases. For complex setups (e.g., separate teams managing environments), 
   consider different directories or repositories.
---
