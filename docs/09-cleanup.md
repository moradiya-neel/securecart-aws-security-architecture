# Resource Cleanup

The SecureCart environment was intended as a short-lived lab. Cleanup matters because services such as NAT Gateway, ALB, public IPv4 addresses, WAF, RDS, and EC2 can continue to incur charges while running.

## Recommended dependency order

1. **Terminate EC2 instances**
   - application EC2
   - bastion host

2. **Delete the Application Load Balancer**
   - `securecart-alb`

3. **Delete the target group**
   - `securecart-tg`

4. **Delete or disassociate the WAF web ACL resources**
   - `securecart-waf-acl`

5. **Delete the RDS database**
   - `securecart-db`
   - for this disposable lab, no final snapshot is required unless the data should be retained

6. **Delete the RDS DB subnet group**
   - `securecart-db-subnet-group`

7. **Empty and delete the S3 bucket**
   - `securecart-assets`

8. **Delete the NAT Gateway**
   - wait until deletion completes before continuing

9. **Release the Elastic IP**
   - release the address after it is no longer associated with the NAT Gateway

10. **Delete custom security groups**
    - remove references/dependencies first

11. **Delete custom route tables**
    - `public-rt`
    - `private-rt`

12. **Delete subnets**
    - `public-subnet-1`
    - `public-subnet-2`
    - `private-subnet-1`
    - `private-subnet-2`

13. **Detach and delete the Internet Gateway**
    - `securecart-igw`

14. **Delete the VPC**
    - `securecart-vpc`

## Final billing check

After cleanup, review Billing and Cost Explorer again after AWS billing data has had time to update. A zero-dollar display during the lab does not guarantee that every service has stopped or that all usage records have already posted.
