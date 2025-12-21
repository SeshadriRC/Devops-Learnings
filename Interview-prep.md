

## AWS

1. How will you handle a situation where your application, running on an EC2 instance, experiences high traffic or a spike during business hours? (3:01 - 3:21)

The candidate would configure an autoscaling group and load balancers. During peak hours, the autoscaling group would scale up EC2 machines based on CPU and RAM usage, and during off-peak hours, it would scale down to save costs. The load balancer would distribute the load among the increased EC2 instances and provide health checks.

2. What kind of troubleshooting approach will you consider if your EC2 instance crashes unexpectedly? (5:06 - 5:14)

First, the candidate would check the AWS console for health checks. If health checks pass, they would log into the server to check RAM and CPU usage. If high, they would try restarting services. If unable to connect to the EC2, they would reboot it. To prevent future crashes, they would configure CPU and RAM usage alerts to take proactive action.

3. If your EC2 application needs to connect to the internet to download something (e.g., images) but is unable to, what might be the possible causes? (7:01 - 7:19)

The possible cause is that the EC2 is in a private subnet with misconfiguration of the NAT Gateway or NAT instance. Specifically, the route table of the private subnet might not have an entry for NAT, or the NAT Gateway itself might be provisioned in a private subnet instead of a public one.

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
