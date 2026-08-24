# Security Comparison

## Before and after

| Control area | Insecure configuration | Secure configuration | Why the change matters |
|---|---|---|---|
| VPC | Default VPC | Dedicated `securecart-vpc` | Creates an intentional network boundary and address plan |
| Application server | Public EC2 | Private EC2 | Removes direct internet reachability |
| Public application path | User → EC2 | User → WAF → ALB → EC2 | Centralizes ingress and adds filtering |
| SSH | `0.0.0.0/0` | Trusted `/32` → bastion → app | Reduces the administrative attack surface |
| HTTP security group | Internet directly to EC2 | `alb-sg` → `app-sg` | Only the load balancer is authorized to reach app port 80 |
| RDS placement | Public | Private subnet group | Removes public database exposure |
| RDS SG | `3306` from `0.0.0.0/0` | `3306` from app SG | Restricts DB network access to the application tier |
| RDS encryption | Not used as verified baseline evidence | Enabled | Protects stored database data at rest |
| S3 public access | Disabled protections + public-read policy | Block Public Access enabled | Prevents anonymous object exposure |
| S3 encryption | Not used as verified baseline evidence | SSE-S3 | Encrypts new objects at rest |
| WAF | None | AWS managed rules | Adds application-layer request filtering |
| Private outbound access | Not separated | NAT Gateway | Allows egress without public EC2 addressing |

## Defense in depth

No single control is treated as sufficient.

For example, the secure application path has multiple independent checks:

1. the ALB is the public endpoint;
2. WAF evaluates requests at the application layer;
3. the EC2 instance is private;
4. `app-sg` accepts port `80` from `alb-sg`, not from the internet;
5. RDS is separately private; and
6. RDS accepts `3306` only from the application security group.

If one layer is bypassed or misconfigured, the remaining controls still limit what can be reached.

## Architectural lesson

The strongest improvement did not come from adding a security product. It came from reducing exposure by design.

The insecure version assumes resources can be public and rules will protect them. The secure version starts from the opposite assumption: resources remain private unless there is a clear reason for exposure.
