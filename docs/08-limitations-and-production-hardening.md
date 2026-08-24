# Limitations and Production Hardening

The secure version is significantly stronger than the insecure baseline, but it should not be described as a complete production implementation. This document records the main gaps intentionally.

## 1. HTTP instead of HTTPS

The ALB listener used HTTP on port `80`.

### Production improvement

- issue a certificate using AWS Certificate Manager;
- add an HTTPS `443` listener;
- redirect HTTP `80` to HTTPS; and
- apply an appropriate TLS security policy.

## 2. Single application EC2 instance

Only one application target was used.

### Production improvement

Use an Auto Scaling Group across private subnets in at least two Availability Zones and define health-based replacement.

## 3. Zonal NAT Gateway

The lab used one NAT Gateway in `public-subnet-1`, while both private subnets use it for outbound connectivity.

### Production improvement

Deploy a NAT Gateway per Availability Zone and route each private subnet to the NAT Gateway in the same AZ. This improves fault isolation and avoids cross-AZ dependency for egress.

## 4. RDS was not Multi-AZ

The DB subnet group spans two Availability Zones, but the database instance itself was configured with Multi-AZ disabled.

### Production improvement

Use Multi-AZ deployment for high availability and failover requirements.

## 5. Bastion host administration

The lab used a public bastion host, and the project workflow copied a PEM key onto the bastion before connecting to the private instance.

### Production improvement

Prefer AWS Systems Manager Session Manager or EC2 Instance Connect Endpoint so administration can occur without a public bastion and without copying private keys to intermediate hosts.

## 6. Self-managed RDS master password

The database master credential was self-managed.

### Production improvement

Use AWS Secrets Manager for secret storage, controlled retrieval, and rotation. Application workloads should use a dedicated database account with only the permissions required by the application rather than the master user.

## 7. Application-to-database functionality was not validated

Network authorization from EC2 to RDS was configured, but the project did not capture an application-level SQL transaction proving read/write functionality.

### Production improvement

Create a minimal database schema and capture a controlled query from the application tier using a non-master database account.

## 8. S3 application integration was not validated

The secure bucket was proven private and encrypted, but there was no captured EC2 IAM role and S3 object retrieval test.

### Production improvement

Attach a least-privilege instance role allowing access only to the required bucket prefix, then validate object retrieval from the application without static AWS credentials.

## 9. Monitoring and detection scope

The project validated WAF activity but did not build a complete monitoring stack.

### Production improvement

Add:

- CloudTrail;
- VPC Flow Logs;
- GuardDuty;
- AWS Config;
- CloudWatch log retention and alarms;
- centralized log storage; and
- alerting / incident-response workflows.

## 10. Infrastructure as Code

The lab was built through the AWS Management Console.

### Production improvement

Recreate the secure architecture with Terraform, AWS CloudFormation, or AWS CDK and use code review plus automated policy checks before deployment.
