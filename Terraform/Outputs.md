
[Practicals](https://github.com/SeshadriRC/Devops/blob/main/100-days-of-Devops/Day98%3A%20Launch%20EC2%20in%20Private%20VPC%20Subnet%20Using%20Terraform.md)

**outputs.tf** is the Terraform file used to **display useful information after your infrastructure is created**.

Think of it as **“final results”** that Terraform prints after `terraform apply`.

---

# 🎯 **Purpose of outputs.tf**

After provisioning AWS resources, you may want to see:

* VPC ID
* Subnet ID
* EC2 private IP
* Security group ID
* RDS endpoint

These are shown using **output blocks**.

---

# 📄 **Example outputs.tf**

For your case:

### outputs.tf

```hcl
output "KKE_vpc_name" {
  value = aws_vpc.datacenter_priv_vpc.tags.Name
}

output "KKE_subnet_name" {
  value = aws_subnet.datacenter_priv_subnet.tags.Name
}

output "KKE_ec2_private" {
  value = aws_instance.datacenter_priv_ec2.private_ip
}
```

---

# 💡 **Why do we use outputs.tf?**

### ✔ To print important values after apply

Example output after deployment:

```
KKE_vpc_name = "datacenter-priv-vpc"
KKE_subnet_name = "datacenter-priv-subnet"
KKE_ec2_private = "10.0.1.25"
```

### ✔ To pass values between Terraform modules

Outputs act like **return values** from a module.

### ✔ To use in other scripts or CI/CD pipelines

Example: a pipeline may need to know the EC2 IP before running Ansible.

---

# 🧩 **Summary: variables.tf vs terraform.tfvars vs outputs.tf**

| File                 | Purpose                                    |
| -------------------- | ------------------------------------------ |
| **variables.tf**     | Declares variables                         |
| **terraform.tfvars** | Assigns values to variables                |
| **outputs.tf**       | Shows important output values after deploy |

---

If you want, I can write **complete main.tf, variables.tf, terraform.tfvars, outputs.tf** for the task you shared.
