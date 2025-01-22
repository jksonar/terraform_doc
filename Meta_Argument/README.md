In Terraform, **meta-arguments** are special arguments that can be used with resources, modules, and data blocks to modify or control their behavior. They are not tied to a specific resource type but are common features of the Terraform language.

### Common Meta-Arguments

1. **depends_on**

   - Ensures that a resource depends on another resource, even if Terraform does not automatically detect the dependency.
   - **Example**:

     ```hcl
     resource "aws_s3_bucket" "example" {
       bucket = "my-example-bucket"
     }

     resource "aws_s3_bucket_object" "example" {
       bucket = aws_s3_bucket.example.id
       key    = "example-object"
       source = "example.txt"

       depends_on = [aws_s3_bucket.example]
     }
     ```

2. **count**

   - Creates multiple instances of a resource or module.
   - **Example**:
     ```hcl
     resource "aws_instance" "example" {
       count         = 3
       ami           = "ami-12345678"
       instance_type = "t2.micro"
     }
     ```
     This creates 3 instances of the AWS EC2 resource.

3. **for_each**

   - Similar to `count`, but allows creating resources for each item in a map or set.
   - **Example**:
     ```hcl
     resource "aws_instance" "example" {
       for_each      = toset(["dev", "prod", "test"])
       ami           = "ami-12345678"
       instance_type = "t2.micro"
       tags = {
         Name = each.key
       }
     }
     ```
     This creates one instance per environment (`dev`, `prod`, `test`).

4. **provider**

   - Specifies which provider configuration to use if multiple configurations are defined.
   - **Example**:

     ```hcl
     provider "aws" {
       alias  = "us_east"
       region = "us-east-1"
     }

     provider "aws" {
       alias  = "us_west"
       region = "us-west-2"
     }

     resource "aws_instance" "example" {
       provider      = aws.us_west
       ami           = "ami-12345678"
       instance_type = "t2.micro"
     }
     ```

5. **lifecycle**

   - Customizes the behavior of a resource's lifecycle, such as preventing updates or controlling deletion.
   - **Example**:

     ```hcl
     resource "aws_s3_bucket" "example" {
       bucket = "my-example-bucket"

       lifecycle {
         prevent_destroy = true
       }
     }
     ```

     This prevents the S3 bucket from being destroyed, even with `terraform destroy`.

6. **iterator**

   - Allows defining a named iterator for `for_each`.
   - **Example**:

     ```hcl
     resource "aws_instance" "example" {
       for_each = {
         dev  = "ami-12345678"
         prod = "ami-87654321"
       }

       ami           = each.value
       instance_type = "t2.micro"
       tags = {
         Name = each.key
       }
     }
     ```

### Summary

Meta-arguments give you control over how resources and modules behave in Terraform. They enable features like dynamic resource
creation, lifecycle management, and fine-grained control of dependencies and providers.
