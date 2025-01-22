# **1. Terraform data types**

In Terraform, data types define the kind of values that can be assigned to variables or used in expressions.
Understanding data types is essential for working with variables, outputs, and module inputs.
Below are the primary Terraform data types with examples.

---

## **Primitive Data Types**

1. **String**

   - Represents text.
   - **Example**:
     ```hcl
     variable "example_string" {
       type    = string
       default = "Hello, Terraform!"
     }
     ```

2. **Number**

   - Represents integers or floating-point numbers.
   - **Example**:
     ```hcl
     variable "example_number" {
       type    = number
       default = 42
     }
     ```

3. **Bool**
   - Represents boolean values (`true` or `false`).
   - **Example**:
     ```hcl
     variable "example_bool" {
       type    = bool
       default = true
     }
     ```

---

## **Complex Data Types**

1. **List**

   - An ordered collection of values of the same type.
   - **Example**:

     ```hcl
     variable "example_list" {
       type    = list(string)
       default = ["apple", "banana", "cherry"]
     }
     ```

     Accessing elements:

     ```hcl
     output "first_item" {
       value = var.example_list[0] # Output: "apple"
     }
     ```

2. **Set**

   - An unordered collection of unique values of the same type.
   - **Example**:
     ```hcl
     variable "example_set" {
       type    = set(string)
       default = ["apple", "banana", "apple"] # Duplicate "apple" is ignored
     }
     ```

3. **Map**

   - A collection of key-value pairs, where keys are strings, and values can be of the same type.
   - **Example**:

     ```hcl
     variable "example_map" {
       type = map(string)
       default = {
         region = "us-east-1"
         env    = "production"
       }
     }
     ```

     Accessing values:

     ```hcl
     output "region" {
       value = var.example_map["region"] # Output: "us-east-1"
     }
     ```

4. **Object**

   - A more complex structure, similar to a map, but with explicitly defined key types and value types.
   - **Example**:
     ```hcl
     variable "example_object" {
       type = object({
         name    = string
         count   = number
         enabled = bool
       })
       default = {
         name    = "example"
         count   = 5
         enabled = true
       }
     }
     ```

5. **Tuple**

   - An ordered collection of values of different types.
   - **Example**:

     ```hcl
     variable "example_tuple" {
       type    = tuple([string, number, bool])
       default = ["example", 5, true]
     }
     ```

     Accessing elements:

     ```hcl
     output "first_item" {
       value = var.example_tuple[0] # Output: "example"
     }
     ```

---

## **Dynamic Types**

1. **Any**
   - Accepts any type of value.
   - **Example**:
     ```hcl
     variable "example_any" {
       type    = any
       default = "This can be any type"
     }
     ```

---

## **Type Constraints with Complex Types**

You can combine primitive and complex types for stricter validation.

### Example: List of Objects

```hcl
variable "example_list_of_objects" {
  type = list(object({
    name    = string
    age     = number
    enabled = bool
  }))
  default = [
    { name = "Alice", age = 30, enabled = true },
    { name = "Bob", age = 25, enabled = false }
  ]
}
```

### Example: Map of Lists

```hcl
variable "example_map_of_lists" {
  type = map(list(string))
  default = {
    team_a = ["Alice", "Bob"]
    team_b = ["Charlie", "David"]
  }
}
```

---

## **Usage in Modules**

Variables with explicit types are especially useful in module inputs to ensure compatibility and validate user-provided values.

```hcl
module "example" {
  source = "./example-module"

  example_string = "Hello"
  example_number = 42
  example_list   = ["a", "b", "c"]
}
```

## By defining types, Terraform ensures that the input matches the expected structure, reducing runtime errors.

---

# **2. how to apply validation rules**

In Terraform, **validation rules** allow you to enforce specific conditions on variable inputs. This ensures that the provided values meet your requirements, improving configuration robustness and preventing errors.

Validation rules are defined using the `validation` block within a variable declaration. This block consists of:

- A `condition` (an expression that evaluates to `true` or `false`).
- An `error_message` (a custom message displayed when the condition is not met).

---

### **Syntax**

```hcl
variable "variable_name" {
  type        = <type>
  description = <description>
  default     = <default_value>

  validation {
    condition     = <expression>
    error_message = "Custom error message if the condition fails"
  }
}
```

---

### **Examples**

#### 1. **String Validation**

Ensure the string meets specific conditions (e.g., starts with "env").

```hcl
variable "environment" {
  type = string
  description = "The environment name"

  validation {
    condition     = can(regex("^env-", var.environment))
    error_message = "The environment must start with 'env-'."
  }
}
```

#### 2. **Number Validation**

Restrict a number to a specific range.

```hcl
variable "instance_count" {
  type        = number
  description = "Number of instances to launch"
  default     = 1

  validation {
    condition     = var.instance_count >= 1 && var.instance_count <= 10
    error_message = "The instance count must be between 1 and 10."
  }
}
```

#### 3. **List Validation**

Validate the allowed values in a list.

```hcl
variable "allowed_regions" {
  type = list(string)
  default = ["us-east-1", "us-west-1"]

  validation {
    condition     = alltrue([for region in var.allowed_regions : contains(["us-east-1", "us-west-1"], region)])
    error_message = "All regions must be 'us-east-1' or 'us-west-1'."
  }
}
```

#### 4. **Set Validation**

Ensure a set contains only unique elements and specific values.

```hcl
variable "availability_zones" {
  type = set(string)

  validation {
    condition     = alltrue([for az in var.availability_zones : az =~ "^us-east-[1-3]$"])
    error_message = "All availability zones must be in the format 'us-east-1', 'us-east-2', etc."
  }
}
```

#### 5. **Map Validation**

Ensure keys or values in a map adhere to rules.

```hcl
variable "tag_map" {
  type = map(string)

  validation {
    condition     = alltrue([for k, v in var.tag_map : length(k) > 0 && length(v) > 0])
    error_message = "All keys and values in the tag map must be non-empty strings."
  }
}
```

#### 6. **Dynamic Validation with `can()`**

Validate inputs using dynamic expressions that won't error out.

```hcl
variable "cidr_block" {
  type = string

  validation {
    condition     = can(cidrnetmask(var.cidr_block))
    error_message = "The provided CIDR block is invalid."
  }
}
```

---

### **Best Practices for Validation**

1. **Provide Clear Error Messages**: Error messages should help users understand what went wrong and how to fix it.
2. **Combine Conditions**: Use logical operators like `&&` and `||` to combine multiple rules.
3. **Test Validation Rules**: Test your configurations with different inputs to ensure validation works as expected.
4. **Avoid Over-Validation**: Don't over-complicate validations; keep them simple and focused on critical requirements.

---

### **Example of Multiple Validations**

```hcl
variable "instance_type" {
  type = string

  validation {
    condition     = var.instance_type != ""
    error_message = "The instance type cannot be empty."
  }

  validation {
    condition     = contains(["t2.micro", "t2.small", "t3.medium"], var.instance_type)
    error_message = "Allowed instance types are: t2.micro, t2.small, t3.medium."
  }
}
```

With these validation rules, Terraform ensures your input values meet your specific requirements before proceeding with the plan or apply phase.

---

# **3. examples of validation**

Here are several **useful and practical validation examples** to implement in your Terraform configurations. These cover a variety of scenarios to ensure inputs meet your expectations.

---

### **1. Validate Environment Name**

Ensure the environment name is one of the allowed values.

```hcl
variable "environment" {
  type = string

  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "The environment must be one of: 'dev', 'staging', or 'prod'."
  }
}
```

---

### **2. Validate Resource Tags**

Ensure all tags have non-empty keys and values.

```hcl
variable "tags" {
  type = map(string)

  validation {
    condition     = alltrue([for k, v in var.tags : length(k) > 0 && length(v) > 0])
    error_message = "All tag keys and values must be non-empty."
  }
}
```

---

### **3. Validate IP Address Format**

Ensure the input is a valid IPv4 address.

```hcl
variable "ip_address" {
  type = string

  validation {
    condition     = can(regex("^([0-9]{1,3}\\.){3}[0-9]{1,3}$", var.ip_address))
    error_message = "The IP address must be in valid IPv4 format, e.g., '192.168.0.1'."
  }
}
```

---

### **4. Validate Port Number**

Ensure the port number is within the valid range (1–65535).

```hcl
variable "port_number" {
  type = number

  validation {
    condition     = var.port_number >= 1 && var.port_number <= 65535
    error_message = "The port number must be between 1 and 65535."
  }
}
```

---

### **5. Validate CIDR Block**

Ensure the input is a valid CIDR block.

```hcl
variable "cidr_block" {
  type = string

  validation {
    condition     = can(cidrnetmask(var.cidr_block))
    error_message = "The provided value must be a valid CIDR block."
  }
}
```

---

### **6. Validate List of Allowed Regions**

Ensure the regions specified in a list are within allowed values.

```hcl
variable "regions" {
  type = list(string)

  validation {
    condition     = alltrue([for region in var.regions : contains(["us-east-1", "us-west-1", "eu-west-1"], region)])
    error_message = "All regions must be one of: 'us-east-1', 'us-west-1', or 'eu-west-1'."
  }
}
```

---

### **7. Validate Minimum Password Length**

Ensure the password meets a minimum length requirement.

```hcl
variable "password" {
  type = string

  validation {
    condition     = length(var.password) >= 8
    error_message = "The password must be at least 8 characters long."
  }
}
```

---

### **8. Validate Instance Types**

Ensure the selected instance type is valid.

```hcl
variable "instance_type" {
  type = string

  validation {
    condition     = contains(["t2.micro", "t2.small", "t3.medium"], var.instance_type)
    error_message = "The instance type must be one of: 't2.micro', 't2.small', or 't3.medium'."
  }
}
```

---

### **9. Validate SSH Key Format**

Ensure the SSH public key is in valid OpenSSH format.

```hcl
variable "ssh_key" {
  type = string

  validation {
    condition     = can(regex("^ssh-(rsa|ed25519)\\s+[A-Za-z0-9+/=]+\\s.*$", var.ssh_key))
    error_message = "The SSH key must be in valid OpenSSH format."
  }
}
```

---

### **10. Validate Non-Empty List**

Ensure the provided list is not empty.

```hcl
variable "subnets" {
  type = list(string)

  validation {
    condition     = length(var.subnets) > 0
    error_message = "You must provide at least one subnet."
  }
}
```

---

### **11. Validate Numeric Threshold**

Ensure the numeric value falls within a specific range.

```hcl
variable "cpu_threshold" {
  type = number

  validation {
    condition     = var.cpu_threshold >= 50 && var.cpu_threshold <= 100
    error_message = "The CPU threshold must be between 50 and 100."
  }
}
```

---

### **12. Validate Bucket Name**

Ensure the S3 bucket name follows AWS naming conventions.

```hcl
variable "bucket_name" {
  type = string

  validation {
    condition     = can(regex("^[a-z0-9.-]{3,63}$", var.bucket_name))
    error_message = "The bucket name must be between 3 and 63 characters and can contain only lowercase letters, numbers, dots, and hyphens."
  }
}
```

---

### **13. Validate Domain Name**

Ensure the input is a valid domain name format.

```hcl
variable "domain_name" {
  type = string

  validation {
    condition     = can(regex("^[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$", var.domain_name))
    error_message = "The domain name must be valid, e.g., 'example.com'."
  }
}
```

---

### **14. Validate Timezone**

Ensure the timezone is from a predefined set of values.

```hcl
variable "timezone" {
  type = string

  validation {
    condition     = contains(["UTC", "PST", "EST"], var.timezone)
    error_message = "The timezone must be one of: 'UTC', 'PST', or 'EST'."
  }
}
```

---

These examples demonstrate how validation can enforce rules for inputs, ensuring that configurations are predictable, robust, and error-free. You can adapt these rules based on your specific use cases.
