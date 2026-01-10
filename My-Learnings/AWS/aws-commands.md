
- [SSH](#ssh)
- [VPC](#vpc)
- [Elastic=ip](#elastic-ip)


# SSH

- How it works (simple)

   - Public key → stored on the EC2 instance
   - Private key (mykey.pem) → stays with you
   - SSH verifies they match

```
ssh -i <private-key> ubuntu@<public-ip>

Eg: ssh -i /downloads/mykey.pem ubuntu@<public-ip>

```

# VPC

```
aws ec2 describe-vpcs --query "Vpcs[].VpcId" --output table

aws ec2 describe-vpcs \
  --query "Vpcs[].{VpcId:VpcId, Name:Tags[?Key=='Name'].Value | [0]}" \
  --output table
```

# Elastic IP

```
aws ec2 describe-addresses --output table
```
