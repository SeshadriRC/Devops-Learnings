
- [EC2](#ec2)
- [Cloudwatch-alarms](#cloudwatch-alarms)
- [Configure](#configure)
- [Security-group](#Security-group)
- [Sns-topics](#Sns-topics)
- [sts](#sts)

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

# sts

```
aws sts get-caller-identity
```



