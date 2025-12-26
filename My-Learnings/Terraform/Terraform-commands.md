Here’s a **clean, interview-ready list of important Terraform commands**, grouped by **day-to-day usage**, **state management**, and **troubleshooting**. These are exactly what you’ll use in real projects and labs.

---

## 🔹 Core Terraform Workflow (Most Important)

### 1️⃣ Initialize Terraform

```bash
terraform init
```

* Downloads providers & modules
* Sets up backend
* Must run **once per directory**

Eg:

```
bob@iac-server ~/terraform via 💠 default ➜  terraform init
Initializing the backend...
Initializing provider plugins...
- Finding latest version of hashicorp/aws...
- Installing hashicorp/aws v6.27.0...
- Installed hashicorp/aws v6.27.0 (signed by HashiCorp)
Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

```

---

### 2️⃣ Validate Configuration

```bash
terraform validate
```

* Syntax check
* No AWS calls
* Very useful in CI/CD

Eg:

```
bob@iac-server ~/terraform via 💠 default ➜  terraform validate
Success! The configuration is valid.

```

---

### 3️⃣ Format Code

```bash
terraform fmt
```

* Auto-formats `.tf` files
* Industry standard

Eg:
```
bob@iac-server ~/terraform via 💠 default ➜  cat main.tf
provider "aws" {
region = "us-east-1"
}

resource "aws_vpc" "datacenter_vpc" {
 cidr_block           = "192.168.0.0/24"
 enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "datacenter-vpc"
  }
}


bob@iac-server ~/terraform via 💠 default ➜  terraform fmt main.tf
main.tf

bob@iac-server ~/terraform via 💠 default ➜  cat main.tf
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "datacenter_vpc" {
  cidr_block           = "192.168.0.0/24"
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "datacenter-vpc"
  }
}

```
---

### 4️⃣ Create Execution Plan

```bash
terraform plan
```

* Shows what Terraform **will create/update/destroy**
* No changes applied

```
bob@iac-server ~/terraform via 💠 default ➜  terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_vpc.datacenter_vpc will be created
  + resource "aws_vpc" "datacenter_vpc" {
      + arn                                  = (known after apply)
      + cidr_block                           = "192.168.0.0/24"
      + default_network_acl_id               = (known after apply)
      + default_route_table_id               = (known after apply)
      + default_security_group_id            = (known after apply)
      + dhcp_options_id                      = (known after apply)
      + enable_dns_hostnames                 = true
      + enable_dns_support                   = true
      + enable_network_address_usage_metrics = (known after apply)
      + id                                   = (known after apply)
      + instance_tenancy                     = "default"
      + ipv6_association_id                  = (known after apply)
      + ipv6_cidr_block                      = (known after apply)
      + ipv6_cidr_block_network_border_group = (known after apply)
      + main_route_table_id                  = (known after apply)
      + owner_id                             = (known after apply)
      + region                               = "us-east-1"
      + tags                                 = {
          + "Name" = "datacenter-vpc"
        }
      + tags_all                             = {
          + "Name" = "datacenter-vpc"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.

```

---

### 5️⃣ Apply Changes

```bash
terraform apply
```

* Creates/updates infrastructure
* Prompts for confirmation

Eg:

```
bob@iac-server ~/terraform via 💠 default ➜  terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_vpc.datacenter_vpc will be created
  + resource "aws_vpc" "datacenter_vpc" {
      + arn                                  = (known after apply)
      + cidr_block                           = "192.168.0.0/24"
      + default_network_acl_id               = (known after apply)
      + default_route_table_id               = (known after apply)
      + default_security_group_id            = (known after apply)
      + dhcp_options_id                      = (known after apply)
      + enable_dns_hostnames                 = true
      + enable_dns_support                   = true
      + enable_network_address_usage_metrics = (known after apply)
      + id                                   = (known after apply)
      + instance_tenancy                     = "default"
      + ipv6_association_id                  = (known after apply)
      + ipv6_cidr_block                      = (known after apply)
      + ipv6_cidr_block_network_border_group = (known after apply)
      + main_route_table_id                  = (known after apply)
      + owner_id                             = (known after apply)
      + region                               = "us-east-1"
      + tags                                 = {
          + "Name" = "datacenter-vpc"
        }
      + tags_all                             = {
          + "Name" = "datacenter-vpc"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_vpc.datacenter_vpc: Creating...
aws_vpc.datacenter_vpc: Still creating... [10s elapsed]
aws_vpc.datacenter_vpc: Creation complete after 12s [id=vpc-054c96749ae1b32a0]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

```

---

### 6️⃣ Destroy Infrastructure

```bash
terraform destroy
```

* Deletes all managed resources
* Dangerous in production ⚠️

---

## 🔹 State Management (Very Important for DevOps)

### 7️⃣ List Resources in State

```bash
terraform state list
```

Eg:

```
bob@iac-server ~/terraform via 💠 default ➜  terraform state list
aws_vpc.datacenter_vpc
```
---

### 8️⃣ Show Resource Details

```bash
terraform state show aws_vpc.datacenter_vpc
```

```
bob@iac-server ~/terraform via 💠 default ➜  terraform state show aws_vpc.datacenter_vpc
# aws_vpc.datacenter_vpc:
resource "aws_vpc" "datacenter_vpc" {
    arn                                  = "arn:aws:ec2:us-east-1:000000000000:vpc/vpc-054c96749ae1b32a0"
    assign_generated_ipv6_cidr_block     = false
    cidr_block                           = "192.168.0.0/24"
    default_network_acl_id               = "acl-b0856ebb8d39e0383"
    default_route_table_id               = "rtb-0a56a5c3d5f2814e5"
    default_security_group_id            = "sg-3b5e0ea4e94cd8799"
    dhcp_options_id                      = "default"
    enable_dns_hostnames                 = true
    enable_dns_support                   = true
    enable_network_address_usage_metrics = false
    id                                   = "vpc-054c96749ae1b32a0"
    instance_tenancy                     = "default"
    ipv6_association_id                  = null
    ipv6_cidr_block                      = null
    ipv6_cidr_block_network_border_group = null
    ipv6_ipam_pool_id                    = null
    ipv6_netmask_length                  = 0
    main_route_table_id                  = "rtb-0a56a5c3d5f2814e5"
    owner_id                             = "000000000000"
    region                               = "us-east-1"
    tags                                 = {
        "Name" = "datacenter-vpc"
    }
    tags_all                             = {
        "Name" = "datacenter-vpc"
    }
}
```
---

### 9️⃣ Remove Resource from State (Does NOT delete in AWS)

```bash
terraform state rm aws_vpc.datacenter_vpc
```

---

### 🔟 Import Existing Resource

```bash
terraform import aws_vpc.myvpc vpc-0abc123
```

---

## 🔹 Working with Outputs & Variables

### 1️⃣1️⃣ Show Output Values

```bash
terraform output
```

### 1️⃣2️⃣ Show Specific Output

```bash
terraform output vpc_id
```

### 1️⃣3️⃣ Pass Variables

```bash
terraform apply -var="region=us-east-1"
```

---

## 🔹 Debugging & Troubleshooting

### 1️⃣4️⃣ Refresh State from AWS

```bash
terraform refresh
```

*(Mostly deprecated but still asked in interviews)*

---

### 1️⃣5️⃣ Enable Debug Logs

```bash
export TF_LOG=DEBUG
terraform apply
```

---

### 1️⃣6️⃣ Reconfigure Backend

```bash
terraform init -reconfigure
```

Eg
```
bob@iac-server ~/terraform via 💠 default ➜  terraform init -reconfigure
Initializing the backend...
Initializing provider plugins...
- Reusing previous version of hashicorp/aws from the dependency lock file
- Using previously-installed hashicorp/aws v6.27.0

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

```

---

## 🔹 Workspace Management

### 1️⃣7️⃣ List Workspaces

```bash
terraform workspace list
```

### 1️⃣8️⃣ Create Workspace

```bash
terraform workspace new dev
```

### 1️⃣9️⃣ Switch Workspace

```bash
terraform workspace select prod
```

---

## 🔹 Module & Provider Related

### 2️⃣0️⃣ Download/Update Modules

```bash
terraform get
```

---

### 2️⃣1️⃣ Provider Lock Info

```bash
terraform providers lock
```

---

## 🔹 Real-World Interview Summary (Must Remember)

| Phase         | Command                     |
| ------------- | --------------------------- |
| Init          | `terraform init`            |
| Check         | `terraform validate`        |
| Preview       | `terraform plan`            |
| Execute       | `terraform apply`           |
| Cleanup       | `terraform destroy`         |
| State inspect | `terraform state list/show` |
| Import        | `terraform import`          |

---

