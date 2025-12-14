
- [EC2](#ec2)
- [Cloudwatch-alarms](#cloudwatch-alarms)
- [Configure](#configure)
- [Security-group](#Security-group)
- [Sns-topics](#Sns-topics)
- [Subnets](#subnets)
- [sts](#sts)
- [VPC](#vpc)

# Configure

```
aws configure list
```

# EC2


- List EC2 instances

```
aws ec2 describe-instances \
  --query "Reservations[*].Instances[*].[InstanceId, Tags[?Key=='Name'].Value | [0]]" \
  --output table
```

# Cloudwatch-alarms

- List Alarms

```
aws cloudwatch describe-alarms \
  --query "MetricAlarms[*].[AlarmName, StateValue]" \
  --output table
```


# Security-group

- [Create Security group](https://github.com/SeshadriRC/Devops/blob/main/AWS/50-days/Day2%3A%20Create%20Security%20Group.md)
- List the security group

```
aws ec2 describe-security-groups \
  --query "SecurityGroups[*].[GroupId,GroupName]" \
  --output table
```


# Sns-topics

- List sns-topics

```
aws sns list-topics
```

# Subnets

- list subnet

```
aws ec2 describe-subnets \
  --filters Name=vpc-id,Values=vpc-0491678b962cfdb50 \
  --query "Subnets[].CidrBlock" \
  --output table
```

- [create subnet](https://github.com/SeshadriRC/Devops/blob/main/AWS/50-days/Day3:%20Create%20Subnet.md)

# sts

```
aws sts get-caller-identity
```

# VPC

```
aws ec2 describe-vpcs --query "Vpcs[].VpcId" --output table

aws ec2 describe-vpcs \
  --query "Vpcs[].{VpcId:VpcId, Name:Tags[?Key=='Name'].Value | [0]}" \
  --output table
```



