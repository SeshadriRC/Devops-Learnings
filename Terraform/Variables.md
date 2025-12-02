[Practicals](https://github.com/SeshadriRC/Devops/blob/main/100-days-of-Devops/Day98%3A%20Launch%20EC2%20in%20Private%20VPC%20Subnet%20Using%20Terraform.md)

**variables.tf** is the file where you **declare** variables in Terraform.

**terraform.tfvars** is where you **give values** to those variables.

Both work together.

---

# 🧩 **1️⃣ variables.tf → DECLARATION of variables**

This file tells Terraform:

* What variables exist
* (Optionally) default values
* (Optionally) descriptions
* (Optionally) type constraints

Example:

```hcl
variable "KKE_VPC_CIDR" {
  description = "CIDR block for the VPC"
  type        = string
}

variable "KKE_SUBNET_CIDR" {
  description = "CIDR block for the subnet"
  type        = string
}
```

This only **declares** the variables.
There are **no actual values yet**.

---

# 🧩 **2️⃣ terraform.tfvars → ASSIGNMENT of variable values**

This file provides actual values for the variables defined above.

Example:

```hcl
KKE_VPC_CIDR   = "10.0.0.0/16"
KKE_SUBNET_CIDR = "10.0.1.0/24"
```

Terraform reads terraform.tfvars automatically.

---

# 🆚 Difference in one line

| File                 | Purpose                                                    |
| -------------------- | ---------------------------------------------------------- |
| **variables.tf**     | Defines variables (structure, type, description, defaults) |
| **terraform.tfvars** | Supplies values for those variables                        |

---

# 🎯 Simple Analogy

Think of Terraform variables like a form:

* **variables.tf** = The blank form telling what fields exist
* **terraform.tfvars** = The form filled with actual answers

Terraform reads both and knows what to deploy.

---

# 🧪 Quick Example: Putting them together

### variables.tf

```hcl
variable "instance_type" {
  description = "Type of EC2 instance"
  type        = string
}
```

### terraform.tfvars

```hcl
instance_type = "t2.micro"
```

### main.tf

```hcl
resource "aws_instance" "example" {
  ami           = "ami-12345"
  instance_type = var.instance_type
}
```

---

If you want, I can also explain:

* `default` values in variables.tf
* `auto.tfvars`
* Best folder structure for Terraform projects

Just tell me!

---

**terraform.tfvars** is a file used in Terraform to **provide values for variables** defined in your variables.tf file.

It is the **default variable input file** that Terraform automatically loads — meaning you do **not** need to pass `-var` or `-var-file` explicitly.

---

# ✅ **Why terraform.tfvars is used**

When you define variables in Terraform, like:

```hcl
variable "KKE_VPC_CIDR" {}
variable "KKE_SUBNET_CIDR" {}
```

Terraform expects you to supply values.

Instead of passing them manually each time:

```
terraform apply -var "KKE_VPC_CIDR=10.0.0.0/16"
```

You can put them into a **terraform.tfvars** file:

```hcl
KKE_VPC_CIDR   = "10.0.0.0/16"
KKE_SUBNET_CIDR = "10.0.1.0/24"
```

Terraform automatically reads this file.

---

# ✅ **What Terraform automatically loads**

Terraform loads:

* **terraform.tfvars**
* **terraform.tfvars.json**
* **any file ending in .auto.tfvars**
* **any file ending in .auto.tfvars.json**

So without specifying anything, Terraform will include variable values from these files.

---

# 👍 **Example**

### variables.tf

```hcl
variable "environment" {}
variable "region" {}
```

### terraform.tfvars

```hcl
environment = "prod"
region      = "us-east-1"
```

Now run:

```
terraform apply
```

Terraform automatically uses these values.

---

# 💡 **Why use terraform.tfvars**

* Keeps variable values separate from code
* Easy to switch environment values
* Cleaner terraform commands
* Works like a config file for Terraform
* Supports multiple .tfvars for dev, qa, prod

---

# 📝 **Tip**

You can also create environment-specific variable files:

```
dev.tfvars
qa.tfvars
prod.tfvars
```

Then apply specifically:

```
terraform apply -var-file="prod.tfvars"
```

---

If you'd like, I can explain:

✅ terraform.tfvars vs variables.tf
✅ auto.tfvars
✅ Best practices for DevOps teams

Just tell me!
