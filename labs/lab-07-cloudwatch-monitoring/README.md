# Lab 07 — Amazon CloudWatch Monitoring & Alerting

## Objective

The objective of this lab was to implement and validate operational monitoring for the private EC2 workload built earlier in Project Aegis.

Amazon CloudWatch was used to monitor the instance's CPU utilization and detect sustained high-CPU conditions. A CloudWatch alarm was configured to publish notifications through Amazon SNS when the CPU utilization exceeded the defined threshold.

The monitoring pipeline was then validated through a controlled CPU stress test performed through AWS Systems Manager Session Manager.

## Architecture

The monitoring and alerting workflow implemented in this lab was:

Private EC2 Instance
→ Amazon CloudWatch CPUUtilization Metric
→ CloudWatch Alarm
→ Amazon SNS Topic
→ Email Notification

The EC2 instance remained in a private subnet and was administered through AWS Systems Manager Session Manager without requiring a public IPv4 address or inbound SSH access.

## CloudWatch Alarm Configuration

A CloudWatch alarm named `ProjectAegis-High-CPU-Alarm` was created to monitor the `CPUUtilization` metric of the private EC2 instance.

Configuration:

- Namespace: `AWS/EC2`
- Metric: `CPUUtilization`
- Statistic: Average
- Period: 5 minutes
- Threshold type: Static
- Alarm condition: CPU utilization greater than 70%
- Datapoints to alarm: 1 out of 1
- Missing data treatment: Treat missing data as missing
- Alarm action: Publish a notification to Amazon SNS

The 70% threshold was selected for controlled validation. The instance normally operated at approximately 0.2% CPU utilization, providing a clear difference between normal behavior and the intentionally generated high-CPU condition.

## Amazon SNS Alerting

An Amazon SNS standard topic named `ProjectAegis-CloudWatch-Alerts` was created as the notification channel for the CloudWatch alarm.

A confirmed email subscription was attached to the SNS topic. Before performing the EC2 stress test, SNS delivery was independently validated by publishing a test message and confirming successful email delivery.

This verified the notification path independently before testing the complete monitoring pipeline.

## Controlled Validation

### Baseline

Before generating load, the private EC2 instance was accessed through AWS Systems Manager Session Manager.

The following commands were used to establish the baseline:

```bash
uptime
nproc

## Troubleshooting

During initial configuration, the CloudWatch alarm displayed a warning indicating that the configured SNS topic could not be found.

Investigation showed that the referenced SNS topic was no longer present in the SNS console. Because the CloudWatch alarm itself remained functional but its notification dependency was unavailable, the issue was isolated to the alerting layer.

The notification workflow was repaired by:

1. Creating a new SNS standard topic: `ProjectAegis-CloudWatch-Alerts`
2. Creating a new email subscription
3. Confirming the email subscription
4. Updating `ProjectAegis-High-CPU-Alarm` to use the new SNS topic
5. Independently testing SNS by publishing a test message
6. Confirming successful email delivery
7. Repeating the CloudWatch CPU stress test

After remediation, the CloudWatch alarm successfully transitioned to `ALARM` and delivered an email notification through SNS.

This demonstrated the importance of validating dependencies between monitoring and notification components rather than assuming that a healthy alarm guarantees successful alert delivery.

## Lessons Learned

### CloudWatch vs. CloudTrail

CloudTrail and CloudWatch serve different purposes within the Aegis environment.

- CloudTrail provides visibility into AWS API activity and supports auditing and investigation.
- CloudWatch provides operational monitoring through metrics and alarms.

In Project Aegis, CloudTrail was used for security auditing and incident reconstruction, while CloudWatch was used to monitor EC2 performance and detect abnormal CPU utilization.

### Monitoring vs. Alerting

A metric by itself only provides visibility into system behavior.

A CloudWatch alarm adds automated evaluation of that metric, while SNS provides a delivery mechanism for notifications.

The resulting workflow is:

`Metric → Alarm → Notification`

### Validation

The lab demonstrated both detection and recovery:

`OK → ALARM → OK`

This confirmed that the monitoring control responded to a simulated abnormal condition and recognized when the workload returned to normal.

### Secure Administration

The CPU test was performed through Systems Manager Session Manager rather than exposing SSH access to the public internet.

This preserved the private-network architecture established earlier in Project Aegis while still allowing controlled administrative access.
