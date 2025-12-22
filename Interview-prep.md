

- [AWS](#AWS)

****************************************************************************************************************************************************************
## AWS
****************************************************************************************************************************************************************
1. How will you handle a situation where your application, running on an EC2 instance, experiences high traffic or a spike during business hours?

Your answer is **mostly correct and interview-ready**, but it can be **strengthened technically and strategically**. I’ll first correct/improve it, then add **extra inputs** that interviewers like to hear from a DevOps engineer.

---

## Improved & Corrected Answer (Interview-Grade)

> To handle high traffic or sudden spikes on an application running on EC2, I would place the instances behind an **Application Load Balancer (ALB)** and configure an **Auto Scaling Group (ASG)**.
>
> The Auto Scaling Group would scale **out** during peak traffic and scale **in** during off-peak hours based on metrics like **CPU utilization, memory usage (via CloudWatch custom metrics), request count per target, or latency**.
>
> The Load Balancer would **distribute traffic evenly**, perform **health checks**, and ensure only healthy instances receive traffic. This setup ensures **high availability, scalability, and cost optimization**.

---

## What You Did Right ✅

* ✔ Mentioned **Auto Scaling Group**
* ✔ Mentioned **Load Balancer**
* ✔ Scaling up during peak and down during off-peak
* ✔ Cost optimization awareness

---

## Corrections & Enhancements ⚙️

### 1️⃣ **CPU & RAM clarification**

* AWS ASG **natively supports CPU**, but **RAM is NOT default**
* Memory-based scaling requires:

  * CloudWatch **custom metrics**
  * CloudWatch Agent on EC2

💡 Interview-friendly correction:

> “Scaling can be based on CPU or memory using CloudWatch custom metrics.”

---

### 2️⃣ Use the Right Load Balancer

* Prefer **Application Load Balancer (ALB)** for:

  * HTTP/HTTPS traffic
  * Path-based & host-based routing
* Use **NLB** only for TCP/UDP or extreme performance needs

---

### 3️⃣ Scaling Strategy (Advanced Touch)

Mention **scaling policies**:

* Target Tracking (preferred)
* Step Scaling
* Scheduled Scaling (business hours)

Example:

> “For predictable traffic, I’d also configure scheduled scaling during known business hours.”

---

## Additional Inputs (What Interviewers Love 💯)

You can add **any 2–3 of these** depending on seniority level.

### 🔹 High Availability

* Deploy EC2 instances across **multiple Availability Zones**
* ASG automatically replaces unhealthy instances

---

### 🔹 Caching for Traffic Spikes

* Use **CloudFront** for static content
* Use **ElastiCache (Redis)** for frequently accessed data

---

### 🔹 Application-Level Optimization

* Enable **connection pooling**
* Optimize database queries
* Use **read replicas** for RDS

---

### 🔹 Monitoring & Alerting

* CloudWatch Alarms
* ALB metrics (RequestCount, TargetResponseTime)
* Enable **Auto Scaling notifications**

---

### 🔹 Graceful Handling of Sudden Spikes

* Enable **instance warm-up**
* Use **launch templates**
* Pre-baked AMIs to reduce boot time

---

## Final Polished Answer (You Can Say This Directly)

> I would handle high traffic by placing the application behind an Application Load Balancer and configuring an Auto Scaling Group across multiple availability zones.
>
> The ASG would scale based on metrics like CPU utilization, request count, or memory using CloudWatch custom metrics. During peak hours, it would automatically scale out, and during off-peak hours, it would scale in to optimize costs.
>
> The load balancer would distribute traffic, perform health checks, and route traffic only to healthy instances. I would also enable monitoring, alerts, and optionally caching using CloudFront or Redis to handle sudden spikes efficiently.

---

If you want, I can:

* 🔥 Make this **shorter (2-minute answer)**
* 🔥 Convert it into **STAR format**
* 🔥 Ask you **follow-up interview questions** on this topic

****************************************************************************************************************************************************************

2. What kind of troubleshooting approach will you consider if your EC2 instance crashes unexpectedly? 

Good question. Your answer has the **right intent**, but for an interview it needs to be **more structured, accurate, and slightly more AWS-aware**. I’ll first correct it, then give you a **clean interview-ready version**, and finally add **extra points** you can mention based on seniority.

---

## What Needs Correction / Improvement ❗

1. **“If health checks pass, log in”**

   * If the instance *crashed*, **health checks usually fail**.
   * AWS has **two health checks**:

     * **System status check** (AWS hardware/network)
     * **Instance status check** (OS-level issues)

2. **Reboot is not always the first action**

   * Reboot only helps for **temporary kernel or memory lockups**
   * For OS-level corruption, you need **recovery steps**

3. **RAM monitoring is not default**

   * CPU is default
   * RAM requires **CloudWatch Agent + custom metrics**

4. Missing key DevOps troubleshooting steps:

   * Logs
   * Root volume check
   * Auto Scaling behavior
   * Preventive actions

---

## Improved & Corrected Answer (Interview-Grade)

> If an EC2 instance crashes unexpectedly, I would follow a structured troubleshooting approach.
>
> First, I would check the **EC2 system and instance status checks** in the AWS console to determine whether the issue is due to AWS infrastructure or an OS/application-level problem.
>
> If the instance is reachable, I would log in via SSH and check **CPU, memory usage (via CloudWatch custom metrics), disk space, and running services**, and review **application and system logs** to identify the root cause.
>
> If the instance is **unreachable**, I would attempt a **reboot**. If that doesn’t help, I would stop the instance, detach the root volume, attach it to another EC2 instance, and inspect logs or fix configuration issues.
>
> In Auto Scaling environments, I would also check whether the instance was terminated due to failed health checks.
>
> To prevent future crashes, I would configure **CloudWatch alarms, log monitoring, proper scaling policies, and resource limits**, and ensure alerts are in place for proactive action.

---

## Key Troubleshooting Flow (Simple to Remember 🧠)

### 1️⃣ Check AWS-Level Health

* System Status Check ❌ → AWS hardware/network issue
* Instance Status Check ❌ → OS/kernel issue

### 2️⃣ Instance Reachable?

**Yes**

* Check:

  * CPU (`top`, `htop`)
  * Memory (`free -m`)
  * Disk (`df -h`)
  * Logs (`/var/log/messages`, app logs)
* Restart affected services

**No**

* Reboot instance
* If still fails:

  * Stop instance
  * Detach root volume
  * Attach to recovery instance
  * Fix issues / retrieve logs

---

## Preventive Actions (Very Important for DevOps Interviews)

Mention **at least 3** of these:

* CloudWatch alarms (CPU, memory, disk)
* Auto Scaling with health checks
* Log monitoring (CloudWatch Logs)
* Proper instance sizing
* Resource limits and ulimits
* Application-level health checks
* Pre-baked AMIs
* Scheduled maintenance windows

---

## Short & Crisp Version (If Interviewer Wants a Quick Answer)

> I would first check EC2 system and instance status checks to identify whether the issue is infrastructure or OS-level. If the instance is reachable, I’d inspect CPU, memory, disk usage, and logs. If unreachable, I’d try a reboot or recover the root volume by attaching it to another instance. Finally, I’d implement CloudWatch alarms, log monitoring, and scaling policies to prevent future crashes.

---

If you want, next I can:

* 🔥 Ask **follow-up interview questions** from this topic
* 🔥 Convert this into **L2 / L3 DevOps level answers**
* 🔥 Add **real production scenarios** you can quote in interviews


****************************************************************************************************************************************************************
3. If your EC2 application needs to connect to the internet to download something (e.g., images) but is unable to, what might be the possible causes? 

Your answer is **directionally correct**, but for an interview it is **too narrow**. You focused only on NAT, while interviewers expect you to think across **networking, routing, security, and OS-level causes**.

I’ll first **correct and enhance your answer**, then give you a **complete troubleshooting checklist**, and finally a **polished interview-ready version**.

---

## What You Got Right ✅

* ✔ Correctly identified **private subnet**
* ✔ Mentioned **NAT Gateway / NAT instance**
* ✔ Correctly pointed out **route table misconfiguration**
* ✔ Correctly stated **NAT Gateway must be in a public subnet**

---

## What’s Missing ❗

You should also mention:

* Internet Gateway (IGW)
* Security Groups
* NACLs
* DNS resolution
* Public vs private IP behavior
* OS-level routing / proxy

---

## Corrected & Expanded Answer (Interview-Grade)

> If an EC2 instance is unable to access the internet, one common reason is that it is running in a **private subnet** without proper outbound internet access.
>
> In such cases, the private subnet’s **route table must have a default route (0.0.0.0/0) pointing to a NAT Gateway or NAT instance**. The NAT Gateway itself must be deployed in a **public subnet** with a route to an **Internet Gateway (IGW)**.
>
> Other possible causes include **missing or misconfigured Internet Gateway**, restrictive **security group or NACL outbound rules**, **DNS resolution issues**, or **OS-level network misconfigurations**.
>
> I would systematically verify routing, security rules, and connectivity from the instance to identify the root cause.

---

## Complete Troubleshooting Checklist (DevOps-Level)

### 1️⃣ Subnet Type

* Private subnet → needs NAT
* Public subnet → needs IGW + public IP

---

### 2️⃣ Route Tables

Private subnet:

```text
0.0.0.0/0 → NAT Gateway
```

Public subnet:

```text
0.0.0.0/0 → Internet Gateway
```

---

### 3️⃣ NAT Gateway / NAT Instance

* Must be in a **public subnet**
* Must have:

  * Elastic IP
  * Route to IGW
* If NAT instance:

  * Source/Destination check disabled

---

### 4️⃣ Internet Gateway

* Attached to the VPC
* Public subnet route points to it

---

### 5️⃣ Security Groups

* Outbound rules allow:

  * HTTP (80)
  * HTTPS (443)
  * DNS (53)

(Default SG allows all outbound, but interviewers want you to mention it.)

---

### 6️⃣ Network ACLs

* Outbound ephemeral ports (1024–65535) allowed
* Return traffic allowed

---

### 7️⃣ DNS Resolution

* `enableDnsSupport` = true
* `enableDnsHostnames` = true
* `/etc/resolv.conf` correctly configured

---

### 8️⃣ OS-Level Checks

* Proxy misconfiguration
* Incorrect default gateway
* Firewall (iptables / firewalld)

---

## Short & Strong Interview Answer (Recommended)

> If an EC2 instance cannot access the internet, I would first verify whether it is in a private subnet. For private subnets, outbound traffic must go through a NAT Gateway placed in a public subnet, and the private subnet’s route table must point 0.0.0.0/0 to the NAT.
>
> I would also check that the VPC has an Internet Gateway attached, security groups and NACLs allow outbound traffic, DNS resolution is enabled, and there are no OS-level network or proxy issues.

---

## One-Line Version (If Interviewer Rushes)

> Internet access issues are usually caused by missing NAT routing, NAT not being in a public subnet, lack of IGW, restrictive security rules, or DNS misconfiguration.

---

If you want, next I can:

* 🔥 Give **diagram-based explanation** (interview favorite)
* 🔥 Ask **follow-up scenario questions**
* 🔥 Convert this into **AWS CLI validation steps**


****************************************************************************************************************************************************************

4. If you have accidentally deleted data from an S3 bucket, or someone else has, and you need to restore it, how will you do this? (8:40 - 8:49)

If versioning is enabled on the S3 bucket, the data can be recovered from older versions. If versioning is not enabled, the candidate is unsure if recovery is possible without AWS support or AWS backup.

5. How will you connect your on-prem data center with your AWS cloud? (10:00 - 10:07)
Considering a scenario where you have two VPCs, with a front-end application on an EC2 instance in one VPC and a back-end service on an EC2 instance in another VPC, and they cannot communicate, what issues might be present and how can they be fixed? (10:47 - 11:24)
If you have an application running on an EC2 machine that needs to fetch objects (like icons) stored in an AWS S3 bucket, but it's not able to, how can you enable communication between the EC2 application and the S3 bucket? (12:53 - 13:16)
Can you explain the architecture of Kubernetes briefly? (14:13 - 14:15)
How will you upgrade your EKS cluster, including its nodes? (15:49 - 16:01)
If you have an application running on an AKS pod with multiple nodes, and you want to ensure that 10 pods (e.g., for Prometheus or Filebeat) are scheduled across all nodes for data scraping, how will you ensure this distribution of pods to the nodes? (16:40 - 17:12)
If you have an application running on a Kubernetes pod, and you want to access its details or the application itself from a browser by hitting a domain like ring.com, and this domain should show the application, how will you set this up? (18:29 - 18:58)
If you need to create 10 EC2 machines with the same configuration using Terraform, what will you use? (21:23 - 21:36)
If a VPC was created manually, and you need to create an EC2 machine using Terraform, but you want to use the existing VPC, how will you ensure this? (22:10 - 22:32)
If a VPC created with Terraform was manually deleted, and you run terraform apply again, what will happen? Will the resource be provisioned again, and how will your pipeline behave? (24:09 - 24:29)
If you have a Terraform module for VPC and another for EC2 instances, and you need to provision both together, passing the output (like VPC ID) from the first module to the second, how will you set this up? (25:09 - 25:48)
Can you explain the stages you have written in your Jenkins pipeline to deploy your application or any kind of pipeline you have written? (27:04 - 27:12)
If you are running a Linux script (e.g., for user creation) and get a "permission denied" error, what steps will you take to troubleshoot this? (27:52 - 28:05)
If your Linux server is performing very slowly, how will you troubleshoot this slow performance, and what commands will you use? 



A: If versioning is enabled on the S3 bucket, the data can be recovered from older versions. If versioning is not enabled, the candidate is unsure if recovery is possible without AWS support or AWS backup. (8:56-9:43)
Q: How will you connect your on-premise data center with your AWS cloud? (10:04-10:07)

A: The candidate would use VPN Site-to-Site connection or Direct Connect, which are two services provided by AWS for this purpose. (10:13-10:38)
Q: You have two VPCs with EC2 instances. One has the front end, and the other has the backend for the same application. They need to communicate, but they are not. What issues can be there, and how can you fix this? (10:47-11:24)

A: To establish connectivity between EC2s in different VPCs, even within the same AWS account, VPC peering needs to be enabled. One VPC initiates the request, and the other accepts it. Additionally, route table rules must be added in both VPCs, specifying the CIDR ranges of the peered VPCs. (11:31-12:42)
Q: Your application on an EC2 machine needs to fetch objects (icons) from an AWS S3 bucket but cannot. How can you enable communication between the EC2 application and the S3 bucket? (12:53-13:17)

A: First, create an IAM role with appropriate S3 permissions (read, write, or full access). Then, attach this IAM role to the EC2 instance from the AWS console. (13:23-14:05)
Kubernetes

Q: Can you explain the architecture of Kubernetes in brief? (14:13-14:15)

A: Kubernetes consists of a master node and worker nodes. The master node has processes like the API server, scheduler, etcd, and controller. Worker nodes have components like kube-proxy, which manages networking on each node. (14:23-15:23)
Q: How will you upgrade your EKS cluster, including your nodes? (15:49-16:01)

A: Before upgrading, manage ongoing pods by using drain or cordon commands to remove pods from nodes. Then, upgrade the node. After the upgrade, use uncordon to schedule pods back onto the node. (16:01-16:32)
Q: You have 10 pods, and you want them all to be scheduled over all the nodes running (e.g., for Prometheus or Filebeat). How will you ensure this distribution of pods to the nodes? (16:40-17:12)

A: To schedule one pod per node, use a DaemonSet. Alternatively, for distributing pods equally, Pod Anti-Affinity can be used in the deployment file to ensure only one pod is scheduled on a particular node. (17:19-18:08)
Q: You have an application running on a Kubernetes pod, and you want to access it from a browser by hitting "ringi.com". How will you set this up? (18:29-18:58)

A: Along with deployment files, you need to add Service and Ingress files. The Service of type Load Balancer will create an actual load balancer on AWS that routes requests to the Ingress controller. The Ingress controller, sitting at the cluster's entry point, reads rules from the Ingress file and routes requests to the destined pods. Finally, in Route 53, the domain name "ringi.com" is configured to point to this load balancer. (18:58-21:06)
Terraform

Q: You have to create 10 EC2 machines with the same configuration. What will you use in Terraform? (21:32-21:36)

A: Use the count variable inside the resource block for the EC2 machine, setting count equal to the number of instances desired. (21:43-22:07)
Q: You have a VPC created manually, and you need to create an EC2 machine using Terraform within this existing VPC. How will you ensure this? (22:13-22:32)

A: Two ways: either directly copy the VPC ID from the console and provide it in the EC2 resource block, or use a data block in Terraform. The data block can fetch details of existing resources by filtering on information like the VPC name or tags, and then extract the VPC ID. (22:39-23:42)
Q: You have a VPC created with Terraform, but someone manually deleted that resource. If you run terraform apply again, what will happen? Will the resource be provisioned again? (24:09-24:29)

A: Yes, if terraform apply is run again, Terraform will compare the configuration file with the actual infrastructure on AWS. Since the VPC is missing, it will detect the discrepancy and attempt to provision it again with the same name and configuration. (24:36-25:03)
Q: You have one module for VPC and another for EC2 instances. You need to provision both together, passing the output of the first module (VPC ID) to the second module (EC2) so it can utilize it. How will you set this up? (25:08-25:48)

A: Define output blocks in the first module (VPC) to output the VPC ID. In the second module (EC2), reference this output as a normal variable using the syntax module.output_variable_name. Additionally, a depends_on block can be added in the EC2 module to ensure it's created only after the VPC. (25:56-27:00)
Jenkins

Q: Can you explain the stages you have written in your Jenkins pipeline to deploy your application or any kind of pipeline you have written? (27:04-27:12)
A: For a simple Java application, the stages would typically include: Checkout (code), Build, Test (if test cases are written), and Deployment. The specific stages can vary based on the application type and setup. (27:16-27:44)
Linux

Q: You are trying to run a script (e.g., for user creation) and are getting a "permission denied" error. What steps will you take to troubleshoot this? (27:53-28:05)

A: The user running the script might not have execute permission, or the script's permissions themselves might not include executable rights (e.g., 777 permissions where 1 is for execute). The solution is to change the mode of the file using chmod +x filename to add executable permission on top of existing permissions. (28:11-29:11)
Q: Your Linux server is performing very slowly. How will you troubleshoot this slow performance, and what commands will you use? (29:20-29:31)

A: The candidate would log into the server and check RAM, CPU, and disk usages.
For RAM: free -mh to see total, used, free, and cache memory. (30:11-30:20)
For Disk: df -h to check disk usage, especially for the root partition (should be below 80%). (30:30-30:50)
To identify processes consuming high CPU/RAM: htop command. This provides an interactive UI where processes can be sorted by CPU or memory percentage. If a process is identified, it can be killed (sudo kill process_id) or its service restarted to free up resources. (30:52-31:39)
Proactive measures include setting up daily/hourly cron jobs for CPU, RAM, and disk checks, and configuring SMTP alerts if usage goes high to take action before customer complaints. (31:41-32:11)
