# Evidence Index

This page maps the repository screenshots to the configuration or validation point they support.

## Insecure baseline evidence

| File | What it shows |
|---|---|
| `01-insecure-ec2-launch-configuration.png` | Public EC2 launch configuration |
| `02-insecure-ec2-open-security-group.png` | SSH and HTTP exposed broadly |
| `03-insecure-nodejs-installation.jpeg` | Node.js installed on baseline instance |
| `04-insecure-public-application.jpeg` | SecureCart baseline page reachable publicly |
| `05-insecure-rds-engine-selection.png` | MySQL RDS selection |
| `06-insecure-rds-weak-credentials.png` | Self-managed weak credential demonstration |
| `07-insecure-rds-public-access.png` | RDS Public access set to Yes |
| `08-insecure-global-ingress-rules.jpeg` | SSH/HTTP/MySQL rules from `0.0.0.0/0` |
| `09-insecure-s3-bucket-creation.jpeg` | Insecure S3 bucket creation |
| `10-insecure-s3-public-access-enabled.jpeg` | Block Public Access disabled |
| `11-insecure-s3-object-uploaded.jpeg` | Test object uploaded |
| `12-insecure-s3-public-read-policy.png` | Public `s3:GetObject` bucket policy |
| `13-insecure-s3-anonymous-access.jpeg` | AWS logo opened anonymously in a private browser |

## Secure build and validation evidence

| File | What it shows |
|---|---|
| `01-vpc-create.png` | Custom VPC creation |
| `02-public-subnet-1.png` | First public subnet |
| `03-public-subnet-2.png` | Second public subnet |
| `04-private-subnet-1.png` | First private subnet |
| `05-private-subnet-2.png` | Second private subnet |
| `06-public-subnet-1-auto-public-ip.png` | Public IPv4 auto-assignment on public subnet 1 |
| `07-public-subnet-2-auto-public-ip.png` | Public IPv4 auto-assignment on public subnet 2 |
| `08-internet-gateway-create.png` | Internet Gateway creation |
| `09-internet-gateway-attach-vpc.png` | IGW attachment to SecureCart VPC |
| `10-elastic-ip-allocation.png` | Elastic IP allocation |
| `11-nat-gateway-create.png` | NAT Gateway in public subnet |
| `12-public-route-table-create.png` | Public route table creation |
| `13-public-route-igw.png` | Public `0.0.0.0/0` route to IGW |
| `14-public-route-table-subnet-association.png` | Public subnet associations |
| `15-private-route-table-create.png` | Private route table creation |
| `16-private-route-nat.png` | Private `0.0.0.0/0` route to NAT |
| `17-private-route-table-subnet-association.png` | Private subnet associations |
| `18-private-ec2-launch.png` | Application EC2 launch |
| `19-private-ec2-networking.png` | EC2 in private subnet with public IP disabled |
| `20-target-group-create.png` | ALB target group settings |
| `21-target-group-register-app.png` | Application instance registered |
| `22-alb-security-group.png` | ALB HTTP ingress from internet |
| `23-alb-basic-configuration.png` | Internet-facing ALB basics |
| `24-alb-network-mapping.png` | ALB mapped to both public subnets |
| `25-alb-listener-routing.png` | HTTP listener forwarding to target group |
| `26-app-sg-allow-alb.png` | App HTTP source restricted to ALB SG |
| `27-bastion-security-group.png` | Bastion SSH restricted to trusted `/32` |
| `28-app-sg-allow-bastion.png` | App SSH source restricted to bastion SG |
| `29-ssh-bastion-to-private-ec2.jpeg` | Successful SSH hop to private EC2 |
| `30-nodejs-install-private-ec2.jpeg` | Node.js installed on private EC2 |
| `31-node-app-files.jpeg` | Minimal test page/server created |
| `32-application-access-through-alb.png` | Application reached using ALB DNS |
| `33-rds-db-subnet-group.png` | Private DB subnet group |
| `34-rds-security-group-app-only.png` | RDS `3306` allowed from app SG |
| `35-rds-engine-and-template.png` | Secure RDS MySQL selection |
| `36-rds-credentials.png` | RDS credential configuration |
| `37-rds-instance-class-storage.png` | RDS class and storage |
| `38-rds-private-connectivity.png` | Public access set to No |
| `39-rds-security-group-selection.png` | RDS SG attached |
| `40-rds-encryption-status.png` | RDS encryption enabled |
| `41-s3-secure-bucket-create.png` | Secure S3 bucket creation |
| `42-s3-block-public-access.png` | Block Public Access enabled |
| `43-s3-default-encryption.png` | SSE-S3 enabled |
| `44-s3-anonymous-access-denied.png` | Anonymous object request denied |
| `45-waf-app-and-resource-selection.png` | WAF scope and ALB resource selection |
| `46-waf-name.png` | Web ACL name |
| `47-waf-managed-rules.png` | AWS managed rule groups |
| `48-waf-alb-association.png` | WAF associated with SecureCart ALB |
| `49-waf-dashboard-summary.png` | WAF request summary |
| `50-waf-sqli-blocked.png` | SQLi-pattern requests blocked |
| `51-healthy-target.png` | Target group healthy status |
| `52-active-alb.png` | ALB active status and listener |
| `53-vpc-resource-map.png` | Final VPC topology in AWS console |
| `54-cost-explorer.png` | Cost Explorer with service categories and `$0.00` displayed |
| `55-billing-summary.png` | Billing summary with eight active service categories and credits banner |
