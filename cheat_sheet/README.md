## **Terraform CLI Commands Cheat Sheet**

### **1. Basics**

| Command                       | Description                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform --help`            | Lists all available commands and options for Terraform.                                       |
| `terraform version`           | Displays the current version of Terraform installed.                                         |
| `terraform force-unlock <lock-id>` | Release a stuck lock																	|
| `terraform console`		| Try Terraform expressions at an interactive prompt							|
| `terraform -install-autocomplete` | Setup tab
auto-completion, requires logging back in |

---

### **2. Working with Configurations**

| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform init`                  | Initializes a Terraform configuration directory. Installs provider plugins and prepares the environment. |
| `terraform init –plugin-dir=PATH` | force plugin installation from a dir  |
| `terraform init –get-plugins=false` | skip plugin installation  |
| `terraform init –backend=false` | skip backend configuration |
| `terraform init -upgrade`         | Upgrade modules/providers to the latest allowed versions  |
| `terraform init –migrate-state –force-copy` | update backend configuratio |
| `terraform get`                   | Only download and install modules                         |
| `terraform get -update=true`  | Download and update modules in the “root” module     |
| `terraform validate`              | Validates the configuration files for syntax errors.                                         |
| `terraform validate -backend=false` | Validate code skip backend validation.                                         |
| `terraform fmt`                   | Formats the configuration files to the canonical style.                                       |
| `terraform fmt -check `           | Check whether the configuration is formatted correctly, return non-zero exit code if not      |                                |
| `terraform providers`             | Lists the providers required for the configuration.                                           |
| `terraform graph`                 | Generates a dependency graph of Terraform resources.                                         |
| `terraform graph \| dot -Tpng > graph.png` | Generate a visual graph of Terraform resources |

---

### **3. Execution Commands**

| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform plan`                  | Generates an execution plan showing what changes will be made without applying them.         |
| `terraform plan -output=<file>`   | Write the plan to a file to apply it later                                                    |
| `terraform plan -target <resource>`| Create a plan for a specific module or resource                                              |
| `terraform plan -replace <resource>`| Force the plan to replace a specific resource                                                |
| `terraform plan -var '<key>=<value>'`| Set a value for one of the input variables                                                 |
| `terraform plan -refresh-only` | Inspect resource drift without updating the state file                                           |
| `terraform apply`                 | Applies the changes required to reach the desired state defined in the configuration files.  |
| `terraform apply -lock=true` | Lock the state file so it can’t be modified by any other Terraform apply or modification action (possible only where backend allows  locking)  |
| `terraform apply var my_region_variable=us-east-1` | Pass a variable via command-line while applying a configuration |
| `terraform apply <file>`  | Create or update infrastructure using a plan file                                                     |
| `terraform apply -target <resource>`| Create or update a specific resource                                                        |
| `terraform apply -replace <resource>`| Force the replacement of a specific resource                                               |
| `terraform apply -auto-approve`   | Applies the changes without prompting for confirmation.                                       |
| `terraform apply --parallelism=5 ` | Number of
simultaneous resource operations |                                       |
| `terraform apply refresh=false `   | Do not reconcile state
file with real-world resources(helpful with large
complex deployments for saving deployment time) |                                      |
| `terraform destroy`               | Removes all infrastructure managed by the configuration.                                     |
| `terraform destroy -target <resource>`| Destroy a specific resource                                                               |
| `terraform destroy -auto-approve` | Destroys resources without confirmation prompt.                                               |

---

### **4. State Management**

| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform state`            | Show the current state in human-readable form                                                   |
| `terraform state <file>`            | Show a human-readable state or plan file                                                |
| `terraform show -json <file>`            | Show a state or plan file in JSON format                                                |
| `terraform state list`            | Lists all resources in the current state.                                                    |
| `terraform state show <resource>` | Shows details of a specific resource in the state.                                           |
| `terraform state rm <resource>`   | Removes a resource from the state file without destroying it.                                |
| `terraform state mv <source> <dest> ` | Moves a resource in the state file.                                                          |
| `terraform state pull `           | Pull current state and output to stdout                                                      |
| `terraform state push `           | push current state and output to stdout                                                      |
| `terraform state replace-provider <from> <to>` | Replace provider for resources in the state                                     |

---

### **5. Resource Management**

| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform import <address> <id>` | Imports existing infrastructure into Terraform's state.                                       |
| `terraform output`                | Displays the output values defined in the configuration.                                      |
| `terraform output -json`			| Show all output values in JSON format															|
| `terraform output <name>`			| Show a specific output value															|
| `terraform output -raw <name>`    | Show a specific output value without quotes												|
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
| `terraform workspace delete <name>`| Delete an existing workspace                                                                |

---

### **8. Modules**

| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform get`                   | Downloads and updates modules defined in the configuration.                                  |
| `terraform init -upgrade`         | Upgrades provider plugins and modules.                                                      |

---

### **9. Terraform Cloud / Remote Authentication**

| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform login`                 | Log in to Terraform Cloud                                                                     |
| `terraform login <hostname>`      | Log in to a different host                                                                    |
| `terraform logout`                | Log out of Terraform Cloud                                                                    |
| `terraform logout <hostname>`      | Log out of a different host                                                                  |

### **10 Referencing Named Values**
| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `<RESOURCE_TYPE>.<NAME>`		|	Reference to a managed resource																	|
| `var.<NAME>`		| Reference to an input variable																			|
| `local.<NAME>` 	| Reference to a local value	|
| `module.<MODULE NAME>` | Reference to a child module					|
| `data.<DATA TYPE>.<NAME>`  | Reference to a data source		|
				
### **11. Lifecycle Meta-Argument Attributes**
| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `create_before_destroy` | Create the new resource before destroying the old one |
| `prevent_destroy` | Prevent Terraform from destroying the resource |
|`ignore_changes` | Ignore changes to specific resource attributes |

### **12. Resource Meta-Arguments**
| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
|`depends_on` | Explicitly specify resource dependencies |
|`count` | Create multiple instances of a resource |
|`for_each` | Create an instance of a resource for each element in a map or set |
|`provider` | Specify a provider configuration block to use for this resource |
|`lifecycle` | Configure the behavior of a resource over its lifetime |

### **13. Splat Expressions**
| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
|`<RESOURCE_TYPE>.<NAME>[*].<ATTRIBUTE>` | Return a list of values for the given attribute of all instances of a resource  |

## **14. Conditional Expressions**
| Command                           | Description                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------|
| `condition ? true : false ` | If condition is true, return true, otherwise return false  |
### **Tips:**

- Use `terraform plan` frequently to preview changes before applying them.
- Store state files securely (e.g., in a remote backend like S3 with state locking enabled).
- Always version-control your configuration files.
- Use workspaces for managing multiple environments (e.g., dev, test, prod).
