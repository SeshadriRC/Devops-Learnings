```
bob@iac-server ~/terraform via 💠 default ➜  pwd
/home/bob/terraform

bob@iac-server ~/terraform via 💠 default ➜  ls
README.MD  provider.tf

bob@iac-server ~/terraform via 💠 default ➜  cat README.MD
# WELCOME TO KKE TERRAFORM LABS! 🚀

This is your Visual Studio Code editor where you will be able to open and update files on the "iac-server" node.

To open a new terminal:

MAC:

⌃ + `

Windows:

Ctrl + Shift + `

To copy text to clipboard:

MAC:

Command + C

Windows:

Ctrl + C

To Paste copied text to terminal:

MAC:

Shift + Command + V

Windows:

Shift + Insert

To open a new file:

MAC:

Command + O

Windows:

Ctrl + O

bob@iac-server ~/terraform via 💠 default ➜  pwd
/home/bob/terraform

bob@iac-server ~/terraform via 💠 default ➜  ls
README.MD  provider.tf

bob@iac-server ~/terraform via 💠 default ➜  ls
README.MD  provider.tf

bob@iac-server ~/terraform via 💠 default ➜  vi main.tf

bob@iac-server ~/terraform via 💠 default ➜  ls
README.MD  main.tf  provider.tf

bob@iac-server ~/terraform via 💠 default ➜  ls -a
.  ..  README.MD  main.tf  provider.tf

bob@iac-server ~/terraform via 💠 default ➜  terraform init
Initializing the backend...
╷
│ Error: Terraform encountered problems during initialisation, including problems
│ with the configuration, described below.
│ 
│ The Terraform configuration must be valid before initialization so that
│ Terraform can determine which modules and providers need to be installed.
│ 
│ 
╵
╷
│ Error: Missing newline after argument
│ 
│   on provider.tf line 6, in terraform:
│    3:     aws = {
│    4:       source = "hashicorp/aws"
│    5:       version = "5.91.0"
│    6:     }pw
│ 
│ An argument definition must end with a newline.
╵

bob@iac-server ~/terraform via 💠 default ✖ terraform init
Initializing the backend...
Initializing provider plugins...
- Finding hashicorp/aws versions matching "5.91.0"...
- Installing hashicorp/aws v5.91.0...
- Installed hashicorp/aws v5.91.0 (signed by HashiCorp)
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

bob@iac-server ~/terraform via 💠 default ➜  ls -a
.  ..  .terraform  .terraform.lock.hcl  README.MD  main.tf  provider.tf

bob@iac-server ~/terraform via 💠 default ➜  terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_vpc.xfusion_vpc will be created
  + resource "aws_vpc" "xfusion_vpc" {
      + arn                                  = (known after apply)
      + cidr_block                           = "10.0.0.0/16"
      + default_network_acl_id               = (known after apply)
      + default_route_table_id               = (known after apply)
      + default_security_group_id            = (known after apply)
      + dhcp_options_id                      = (known after apply)
      + enable_dns_hostnames                 = (known after apply)
      + enable_dns_support                   = true
      + enable_network_address_usage_metrics = (known after apply)
      + id                                   = (known after apply)
      + instance_tenancy                     = "default"
      + ipv6_association_id                  = (known after apply)
      + ipv6_cidr_block                      = (known after apply)
      + ipv6_cidr_block_network_border_group = (known after apply)
      + main_route_table_id                  = (known after apply)
      + owner_id                             = (known after apply)
      + tags                                 = {
          + "Name" = "xfusion-vpc"
        }
      + tags_all                             = {
          + "Name" = "xfusion-vpc"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.

bob@iac-server ~/terraform via 💠 default ➜  terraform validate
Success! The configuration is valid.


bob@iac-server ~/terraform via 💠 default ➜  ls -a
.  ..  .terraform  .terraform.lock.hcl  README.MD  main.tf  provider.tf

bob@iac-server ~/terraform via 💠 default ➜  terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_vpc.xfusion_vpc will be created
  + resource "aws_vpc" "xfusion_vpc" {
      + arn                                  = (known after apply)
      + cidr_block                           = "10.0.0.0/16"
      + default_network_acl_id               = (known after apply)
      + default_route_table_id               = (known after apply)
      + default_security_group_id            = (known after apply)
      + dhcp_options_id                      = (known after apply)
      + enable_dns_hostnames                 = (known after apply)
      + enable_dns_support                   = true
      + enable_network_address_usage_metrics = (known after apply)
      + id                                   = (known after apply)
      + instance_tenancy                     = "default"
      + ipv6_association_id                  = (known after apply)
      + ipv6_cidr_block                      = (known after apply)
      + ipv6_cidr_block_network_border_group = (known after apply)
      + main_route_table_id                  = (known after apply)
      + owner_id                             = (known after apply)
      + tags                                 = {
          + "Name" = "xfusion-vpc"
        }
      + tags_all                             = {
          + "Name" = "xfusion-vpc"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_vpc.xfusion_vpc: Creating...
aws_vpc.xfusion_vpc: Creation complete after 1s [id=vpc-67b497a979f5f9748]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

bob@iac-server ~/terraform via 💠 default ➜  terraform state list
aws_vpc.xfusion_vpc

bob@iac-server ~/terraform via 💠 default ➜  terraform state show aws_vpc.xfusion_vpc
# aws_vpc.xfusion_vpc:
resource "aws_vpc" "xfusion_vpc" {
    arn                                  = "arn:aws:ec2:us-east-1:000000000000:vpc/vpc-67b497a979f5f9748"
    assign_generated_ipv6_cidr_block     = false
    cidr_block                           = "10.0.0.0/16"
    default_network_acl_id               = "acl-53fcb00c21b2f8a0b"
    default_route_table_id               = "rtb-e699912c329aa4fd9"
    default_security_group_id            = "sg-fc0c695aaad8f34af"
    dhcp_options_id                      = "default"
    enable_dns_hostnames                 = false
    enable_dns_support                   = true
    enable_network_address_usage_metrics = false
    id                                   = "vpc-67b497a979f5f9748"
    instance_tenancy                     = "default"
    ipv6_association_id                  = null
    ipv6_cidr_block                      = null
    ipv6_cidr_block_network_border_group = null
    ipv6_ipam_pool_id                    = null
    ipv6_netmask_length                  = 0
    main_route_table_id                  = "rtb-e699912c329aa4fd9"
    owner_id                             = "000000000000"
    tags                                 = {
        "Name" = "xfusion-vpc"
    }
    tags_all                             = {
        "Name" = "xfusion-vpc"
    }
}

bob@iac-server ~/terraform via 💠 default ➜  
```


Here you go — a clean explanation of the commonly used Terraform commands and flags.

---

# ✅ **1. terraform init**

Initializes the working directory for Terraform.

```
terraform init
```

What it does:

* Downloads required providers (AWS, Kubernetes, Vault, etc.)
* Sets up the `.terraform` folder
* Makes Terraform ready to run

---

# ✅ **2. terraform plan**

Shows what Terraform **WILL DO** without actually doing it.

```
terraform plan
```

Terraform will show:

* What resources it will create
* What resources will change
* What resources will be destroyed

This is **read-only** — no actual changes happen.

---

# ✅ **3. terraform apply**

Applies the plan and makes real changes.

```
terraform apply
```

Terraform stops and asks:

```
Do you want to perform these actions? (yes/no)
```

---

# ✅ **4. terraform apply -auto-approve**

Runs `apply` **without manual approval**.

```
terraform apply -auto-approve
```

Used for automation (CI/CD).
⚠️ Risky for production — no confirmation.

---

# ✅ **5. terraform destroy**

Destroys **all resources** created by Terraform.

```
terraform destroy
```

Will ask for confirmation.
Used to delete test environments or avoid cost.

---

# ✅ **6. terraform destroy -auto-approve**

Destroys everything WITHOUT asking.

```
terraform destroy -auto-approve
```

⚠️ Very dangerous. Use only in automation or temporary setups.

---

# ✅ **7. terraform fmt**

Formats your `.tf` files properly.

```
terraform fmt
```

Fixes spacing, alignment, indentation.

---

# ✅ **8. terraform validate**

Checks if your Terraform syntax is correct.

```
terraform validate
```

Useful before committing code.

---

# Want examples?

If you want, I can prepare:

✔ Perfect workflow for AWS
✔ CI/CD pipeline for Terraform
✔ Example folder structure (prod, dev, modules)

Just tell me!
