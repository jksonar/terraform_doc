Terraform provides a wide variety of **built-in functions** to handle and manipulate data effectively. These functions are grouped into several categories based on their purpose. Below is the comprehensive list of Terraform's built-in functions as of Terraform 1.x.

---

### **1. Numeric Functions**

- **`abs`**: Absolute value of a number.
- **`ceil`**: Rounds up to the nearest integer.
- **`floor`**: Rounds down to the nearest integer.
- **`log`**: Natural logarithm of a number.
- **`max`**: Maximum of a list of numbers.
- **`min`**: Minimum of a list of numbers.
- **`parseint`**: Converts a string to an integer.
- **`pow`**: Raises a number to the power of another.
- **`signum`**: Returns 1 if positive, -1 if negative, and 0 if zero.

---

### **2. String Functions**

- **`chomp`**: Removes trailing newlines.
- **`chunklist`**: Splits a string into chunks of a specified size.
- **`format`**: Formats a string with placeholders.
- **`formatlist`**: Applies a format to a list of items.
- **`indent`**: Adds indentation to each line of a string.
- **`join`**: Concatenates a list of strings with a delimiter.
- **`lower`**: Converts a string to lowercase.
- **`replace`**: Replaces parts of a string.
- **`regex`**: Extracts parts of a string using a regular expression.
- **`regexall`**: Finds all matches of a regular expression.
- **`split`**: Splits a string into a list.
- **`strrev`**: Reverses a string.
- **`substr`**: Extracts a substring.
- **`title`**: Capitalizes the first letter of each word.
- **`trimspace`**: Removes leading and trailing whitespace.
- **`trimprefix`**: Removes a prefix from a string.
- **`trimsuffix`**: Removes a suffix from a string.
- **`upper`**: Converts a string to uppercase.

---

### **3. Collection Functions**

- **`alltrue`**: Checks if all elements in a list are `true`.
- **`anytrue`**: Checks if any element in a list is `true`.
- **`chunklist`**: Splits a list into chunks of a specified size.
- **`coalesce`**: Returns the first non-`null` value.
- **`coalescelist`**: Returns the first non-empty list.
- **`compact`**: Removes `null` values from a list.
- **`concat`**: Concatenates lists together.
- **`contains`**: Checks if a list contains a specific element.
- **`distinct`**: Removes duplicates from a list.
- **`element`**: Gets an element from a list by index.
- **`flatten`**: Flattens nested lists into a single list.
- **`index`**: Finds the index of an element in a list.
- **`keys`**: Returns the keys of a map.
- **`length`**: Returns the length of a list, map, or string.
- **`lookup`**: Gets a value from a map by key with a default.
- **`merge`**: Merges multiple maps.
- **`range`**: Generates a sequence of numbers.
- **`reverse`**: Reverses a list.
- **`setproduct`**: Generates all combinations of elements from lists.
- **`slice`**: Extracts a sublist from a list.
- **`sort`**: Sorts a list of strings.
- **`sum`**: Sums a list of numbers.
- **`transpose`**: Transposes a map of lists.
- **`values`**: Returns the values of a map.
- **`zipmap`**: Creates a map from two lists (keys and values).

---

### **4. Encoding and Decoding Functions**

- **`base64decode`**: Decodes a base64 string.
- **`base64encode`**: Encodes a string in base64.
- **`base64gzip`**: Compresses and base64-encodes a string.
- **`csvdecode`**: Decodes a CSV string into a list of maps.
- **`jsondecode`**: Decodes a JSON string into a data structure.
- **`jsonencode`**: Encodes a value as a JSON string.
- **`urlencode`**: Encodes a string as a URL query parameter.
- **`yamldecode`**: Decodes a YAML string into a data structure.
- **`yamlencode`**: Encodes a value as a YAML string.

---

### **5. Filesystem Functions**

- **`file`**: Reads the contents of a file as a string.
- **`filebase64`**: Reads the contents of a file as base64.
- **`filebase64sha256`**: Returns the SHA-256 checksum of a file in base64.
- **`filebase64sha512`**: Returns the SHA-512 checksum of a file in base64.
- **`filemd5`**: Returns the MD5 checksum of a file.
- **`filesha1`**: Returns the SHA-1 checksum of a file.
- **`filesha256`**: Returns the SHA-256 checksum of a file.
- **`filesha512`**: Returns the SHA-512 checksum of a file.
- **`templatefile`**: Renders a template file with variables.

---

### **6. Cryptographic Functions**

- **`md5`**: Computes the MD5 hash of a string.
- **`sha1`**: Computes the SHA-1 hash of a string.
- **`sha256`**: Computes the SHA-256 hash of a string.
- **`sha512`**: Computes the SHA-512 hash of a string.

---

### **7. IP Network Functions**

- **`cidrhost`**: Computes an IP address in a CIDR block.
- **`cidrnetmask`**: Gets the subnet mask of a CIDR block.
- **`cidrsubnet`**: Calculates a subnet within a CIDR block.
- **`cidrsize`**: Computes the size of a CIDR block.

---

### **8. Date and Time Functions**

- **`formatdate`**: Formats a timestamp.
- **`timeadd`**: Adds a duration to a timestamp.
- **`timestamp`**: Returns the current time.

---

### **9. Hash and ID Functions**

- **`uuid`**: Generates a UUID.
- **`uuidv5`**: Generates a UUIDv5 based on a namespace and name.

---

### **10. Dynamic and Type Conversion Functions**

- **`can`**: Tests if an expression is valid without error.
- **`default`**: Returns a default value if a variable is `null`.
- **`tolist`**: Converts a value to a list.
- **`tomap`**: Converts a value to a map.
- **`tonumber`**: Converts a value to a number.
- **`tostring`**: Converts a value to a string.
- **`try`**: Tries multiple expressions and returns the first that doesn't error.

---

### **11. Specialized Functions**

- **`abs`**: Absolute value (also listed in numeric functions).
- **`join`**: Joins elements (also listed in string functions).
- **`lookup`**: Lookup from a map (also listed in collection functions).

---

These functions cover nearly all common scenarios for data manipulation, making Terraform highly flexible and powerful for Infrastructure as Code (IaC).
