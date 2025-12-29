- A **subnet** is a **logical subdivision of an IP network** that divides a larger network into smaller, manageable networks to **improve routing efficiency, security, and IP utilization**.
- Assume in aws you created VPC with 65000 ip addresses.
- Then we are separating 20k ip adress for finance team, 30k ip's for IT department, 10k for free users.
- If incase free user tried to access malicious network , then only those subnet will get affected. remaining will be fine

<img width="1083" height="582" alt="image" src="https://github.com/user-attachments/assets/f5c3458a-d63e-43b0-85ee-29881ca099d6" />

**Advantages**

- Security
- Privacy
- Isolation

**Two types**

- Private subnet -> does not have access to the internet
- Public subnet -> have access to the internet

---

- In the above line no 3, i told right 20k ip address for finance subnet, 30k ip for IT etc., so how we can achieve that ? By using CIDR range.
- While creating a subnet we need to mention CIDR range

<img width="1065" height="516" alt="image" src="https://github.com/user-attachments/assets/a1ec2519-3b4a-42e8-ac06-c2db9ca7682e" />
