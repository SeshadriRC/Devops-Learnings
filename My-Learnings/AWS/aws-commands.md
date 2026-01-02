
- [VPC](#vpc)


# VPC

```
aws ec2 describe-vpcs --query "Vpcs[].VpcId" --output table

aws ec2 describe-vpcs \
  --query "Vpcs[].{VpcId:VpcId, Name:Tags[?Key=='Name'].Value | [0]}" \
  --output table
```
