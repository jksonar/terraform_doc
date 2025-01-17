Here’s a cheat sheet for **Terraform CLI commands** along with brief descriptions to help you study:  

---

## **Terraform CLI Commands Cheat Sheet**

### **1. Basics**

| Command                       | Description                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform --help`            | Lists all available commands and options for Terraform.                                       |
| `terraform version`           | Displays the current version of Terraform installed.                                         |

---

### **2. Working with Configurations**

| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform init`                  | Initializes a Terraform configuration directory. Installs provider plugins and prepares the environment. |
| `terraform init -upgrade`         | Upgrade modules/providers to the latest allowed versions  |
| `terraform get`                   | Only download and install modules                         |
| `terraform validate`              | Validates the configuration files for syntax errors.                                         |
| `terraform fmt`                   | Formats the configuration files to the canonical style.                                       |
| `terraform providers`             | Lists the providers required for the configuration.                                           |
| `terraform graph`                 | Generates a dependency graph of Terraform resources.                                         |

---

### **3. Execution Commands**

| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform plan`                  | Generates an execution plan showing what changes will be made without applying them.         |
| `terraform apply`                 | Applies the changes required to reach the desired state defined in the configuration files.  |
| `terraform apply -auto-approve`   | Applies the changes without prompting for confirmation.                                       |
| `terraform destroy`               | Removes all infrastructure managed by the configuration.                                     |
| `terraform destroy -auto-approve` | Destroys resources without confirmation prompt.                                               |

---

### **4. State Management**

| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform state list`            | Lists all resources in the current state.                                                    |
| `terraform state show <resource>` | Shows details of a specific resource in the state.                                           |
| `terraform state rm <resource>`   | Removes a resource from the state file without destroying it.                                |
| `terraform state mv`              | Moves a resource in the state file.                                                          |

---

### **5. Resource Management**

| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform import <address> <id>` | Imports existing infrastructure into Terraform's state.                                       |
| `terraform output`                | Displays the output values defined in the configuration.                                      |
| `terraform refresh`               | Updates the state file with the real-world state of resources.                               |

---

### **6. Debugging and Logging**

| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform taint <resource>`      | Marks a resource for recreation during the next `terraform apply`.                           |
| `terraform untaint <resource>`    | Removes the taint from a resource.                                                           |
| `TF_LOG=<level> terraform <cmd>`  | Sets the logging level (`TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`) for detailed logs.         |

---

### **7. Workspaces**

| Command                            | Description                                                                                  |
|------------------------------------|----------------------------------------------------------------------------------------------|
| `terraform workspace list`         | Lists all workspaces in the current configuration.                                           |
| `terraform workspace show`         | Shows the name of the current workspace.                                                    |
| `terraform workspace new <name>`   | Creates a new workspace.                                                                    |
| `terraform workspace select <name>`| Switches to the specified workspace.                                                        |

---

### **8. Modules**

| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform get`                   | Downloads and updates modules defined in the configuration.                                  |
| `terraform init -upgrade`         | Upgrades provider plugins and modules.                                                      |

---

### **Tips:**

- Use `terraform plan` frequently to preview changes before applying them.
- Store state files securely (e.g., in a remote backend like S3 with state locking enabled).
- Always version-control your configuration files.
- Use workspaces for managing multiple environments (e.g., dev, test, prod).
