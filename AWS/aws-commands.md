
- [EC2](#ec2)
- [Cloudwatch-alarms](#cloudwatch-alarms)
- [Sns-topics](#Sns-topics)

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

# Sns-topics

- List sns-topics

```
aws sns list-topics
```
