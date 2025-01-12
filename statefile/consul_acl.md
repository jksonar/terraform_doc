To create a Consul token for Terraform's backend that does not expire, you need to generate a token with an appropriate policy and set the TTL (Time-to-Live) to `0`, which indicates no expiration.

Here’s how you can do it:

---

### **1. Create a Policy for Terraform Backend**
You need a policy that grants the necessary permissions for Terraform's Consul backend. For example:

```hcl
# terraform-policy.hcl
key_prefix "terraform/" {
  policy = "write"
}
```

This policy gives write access to all keys under the `terraform/` namespace.

---

### **2. Apply the Policy in Consul**
Use the `consul acl policy create` command to add this policy to your Consul cluster:

```bash
consul acl policy create \
  -name "terraform-policy" \
  -rules @terraform-policy.hcl
```

---

### **3. Create a Token with No Expiration**
Now, generate a token associated with the policy you just created. Set its TTL to `0` to make it non-expiring:

```bash
consul acl token create \
  -description "Terraform Backend Token" \
  -policy-name "terraform-policy" \
  -ttl "0"
```

The output will contain a token that you can use in your Terraform configuration.

---

### **4. Update Terraform Backend Configuration**
In your `terraform` backend block, update the configuration to use the generated token:

```hcl
terraform {
  backend "consul" {
    address   = "127.0.0.1:8500" # Update with your Consul address
    path      = "terraform/state"
    token     = "your-generated-token"
  }
}
```

---

### **5. Secure the Token**
Ensure the token is securely stored, as it grants access to the backend state. You can use environment variables or a secure secrets management system.

For example, using an environment variable:

```bash
export CONSUL_HTTP_TOKEN="your-generated-token"
```

Update your Terraform backend configuration to reference the environment variable:

```hcl
terraform {
  backend "consul" {
    address = "127.0.0.1:8500"
    path    = "terraform/state"
  }
}
```

---

### **6. Verify the Token**
To confirm that the token works, test with the following command:

```bash
consul acl token read -id "your-generated-token-id"
```

This ensures that the token is valid and has the expected permissions.

Let me know if you encounter any issues!