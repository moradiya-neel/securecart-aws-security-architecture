# Secure Networking: VPC, Subnets, Gateways and Routing

## Goal

The secure phase starts with the network because placement and routing determine which resources can be reached in the first place.

## 1. Custom VPC

A dedicated VPC was created with CIDR `10.0.0.0/16`.

![VPC create](../assets/evidence/secure/01-vpc-create.png)

The `/16` provides a large private address space that can be divided into smaller subnets for different tiers.

## 2. Public and private subnet design

| Name | AZ | CIDR | Role |
|---|---|---|---|
| `public-subnet-1` | `us-east-1a` | `10.0.1.0/24` | ALB, NAT Gateway, bastion |
| `public-subnet-2` | `us-east-1b` | `10.0.3.0/24` | ALB |
| `private-subnet-1` | `us-east-1a` | `10.0.2.0/24` | Application / private resources |
| `private-subnet-2` | `us-east-1b` | `10.0.4.0/24` | Private RDS subnet group / private resources |

![Public subnet 1](../assets/evidence/secure/02-public-subnet-1.png)

![Public subnet 2](../assets/evidence/secure/03-public-subnet-2.png)

![Private subnet 1](../assets/evidence/secure/04-private-subnet-1.png)

![Private subnet 2](../assets/evidence/secure/05-private-subnet-2.png)

Automatic public IPv4 assignment was enabled for the public subnets.

![Public subnet 1 public IP setting](../assets/evidence/secure/06-public-subnet-1-auto-public-ip.png)

![Public subnet 2 public IP setting](../assets/evidence/secure/07-public-subnet-2-auto-public-ip.png)

The private subnets were intentionally left without automatic public IP assignment.

## 3. Internet Gateway

The `securecart-igw` Internet Gateway was created and attached to the custom VPC.

![Internet Gateway creation](../assets/evidence/secure/08-internet-gateway-create.png)

![Internet Gateway attached](../assets/evidence/secure/09-internet-gateway-attach-vpc.png)

The Internet Gateway is not what makes every resource in the VPC public. A resource also needs the appropriate subnet route, addressing, and security rules.

## 4. NAT Gateway

An Elastic IP was allocated for the public NAT Gateway.

![Elastic IP allocation](../assets/evidence/secure/10-elastic-ip-allocation.png)

The NAT Gateway was deployed in `public-subnet-1`.

![NAT Gateway](../assets/evidence/secure/11-nat-gateway-create.png)

Its job is outbound connectivity for resources in private subnets. It does not create a path for unsolicited inbound internet connections to those private instances.

## 5. Public route table

A `public-rt` route table was created.

![Public route table](../assets/evidence/secure/12-public-route-table-create.png)

Its internet route sends `0.0.0.0/0` to the Internet Gateway.

![Public internet route](../assets/evidence/secure/13-public-route-igw.png)

Both public subnets were associated with `public-rt`.

![Public subnet associations](../assets/evidence/secure/14-public-route-table-subnet-association.png)

## 6. Private route table

A separate `private-rt` route table was created.

![Private route table](../assets/evidence/secure/15-private-route-table-create.png)

Its default route sends `0.0.0.0/0` to the NAT Gateway instead of directly to the Internet Gateway.

![Private NAT route](../assets/evidence/secure/16-private-route-nat.png)

Both private subnets were associated with the private route table.

![Private subnet associations](../assets/evidence/secure/17-private-route-table-subnet-association.png)

## Result

The networking layer now expresses the intended trust model:

- public-facing resources can live in the public subnets;
- application and database resources can live in private subnets;
- private EC2 can still reach external package repositories through NAT; and
- there is no direct internet route designed for inbound access to the private application or database tiers.

Final AWS VPC resource-map evidence is included later in the validation section.
