**Load Balancer in AWS VPC — short and clear** 👇

> **A Load Balancer is a managed AWS service that distributes incoming traffic across multiple targets (EC2, containers, IPs) inside a VPC.**

### Key points:

* Lives **inside a VPC**
* Placed in **public or private subnets**
* Improves:

  * **Availability**
  * **Scalability**
  * **Fault tolerance**
* Receives traffic on **one endpoint** and forwards it to healthy targets

### Common types:

* **ALB** – HTTP/HTTPS (Layer 7)
* **NLB** – TCP/UDP (Layer 4)
* **CLB** – Legacy

### One-line interview answer:

> “A load balancer in AWS distributes incoming traffic across multiple resources in a VPC to ensure high availability and reliability.”

If you want, I can also explain **ALB vs NLB in one table** or **where LB sits in public vs private subnet**.
