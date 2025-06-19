# Comprehensive AWS Deployment Report - ECOSHOP

**Student:** FIENI DANNIE INNOCENT JUNIOR
**Date:** June 13, 2025

This report details the complete deployment process of a robust and scalable infrastructure on Amazon Web Services (AWS) for the Ecoshop application. It covers the setup of the network infrastructure, security group configuration, deployment of application servers, RDS database, and Load Balancer implementation, as well as testing and a critical analysis of the architecture.

---

## Table of Contents

1.  [**PHASE 1: NETWORK INFRASTRUCTURE**](infrastructure/vpc.md)
    * [1.1 Main VPC Creation](infrastructure/vpc.md#11-main-vpc-creation)
    * [1.2 Subnet Creation](infrastructure/vpc.md#12-subnet-creation-6-subnets-across-2-az)
    * [1.3 Gateway Configuration](infrastructure/vpc.md#13-gateway-configuration)
    * [1.4 Route Table Configuration](infrastructure/vpc.md#14-route-table-configuration)
2.  [**PHASE 2: SECURITY CONFIGURATION (Security Groups)**](infrastructure/security_groups.md)
    * [2.1 Security Groups to Create](infrastructure/security_groups.md#21-security-groups-to-create)
3.  [**PHASE 3: SERVER DEPLOYMENT**](servers/bastion_host.md)
    * [3.1 Bastion Host Creation](servers/bastion_host.md#31-bastion-host-creation)
    * [3.2 Application Server Creation](servers/app_servers.md#32-application-server-creation)
    * [3.3 Connectivity Tests](servers/app_servers.md#33-connectivity-tests)
4.  [**PHASE 4: RDS DATABASE**](database/rds.md)
    * [STEP 4.1: Create DB Subnet Group](database/rds.md#step-41-create-db-subnet-group)
    * [STEP 4.2: Create RDS Instance](database/rds.md#step-42-create-rds-instance)
5.  [**PHASE 5: LOAD BALANCER**](load_balancer/target_group.md)
    * [STEP 5.1: Create Target Group](load_balancer/target_group.md#step-51-create-target-group)
    * [STEP 5.2: Create Application Load Balancer](load_balancer/alb.md#step-52-create-application-load-balancer)
6.  [**PHASE 6: TESTS AND VALIDATION**](tests_and_validation.md)
    * [TEST 1: Wait for everything to be operational](tests_and_validation.md#test-1-wait-for-everything-to-be-operational)
    * [TEST 2: Bastion Connectivity Test](tests_and_validation.md#test-2-bastion-connectivity-test)
    * [TEST 3: Application Test via ALB](tests_and_validation.md#test-3-application-test-via-alb)
    * [TEST 4: Database Test](tests_and_validation.md#test-4-database-test)
7.  [**DOCUMENTED ARCHITECTURE**](#documented-architecture)
    * [Deployed Architecture Diagram](architecture/ip_addressing_plan.md#deployed-architecture-diagram)
    * [Detailed IP Addressing Plan](architecture/ip_addressing_plan.md#detailed-ip-addressing-plan)
    * [Specific Assigned IP Addresses](architecture/ip_addressing_plan.md#specific-assigned-ip-addresses)
    * [Network Flow Matrix](architecture/ip_addressing_plan.md#network-flow-matrix)
8.  [**DEPLOYMENT LOG**](#deployment-log)
    * [Issues Encountered and Solutions](#issues-encountered-and-solutions)
9.  [**CRITICAL ANALYSIS**](#critical-analysis)
    * [Security](#security)
    * [High Availability](#high-availability)
    * [Performance and Cost](#performance-and-cost)
    * [Additional Recommendations](#additional-recommendations)

---

## DEPLOYMENT LOG

### Issues Encountered and Solutions

| Issue                               | Solution Applied                                           | Time    |
| :---------------------------------- | :--------------------------------------------------------- | :------ |
| User Data not executing             | Checking logs in `/var/log/cloud-init-output.log`          | 10 min  |
| Unhealthy targets in ALB            | Modifying health check path to `/index.php`                | 5 min   |
| No internet access from app servers | Verifying route to NAT Gateway in the private table        | 15 min  |
| RDS connection failed               | Opening port 3306 in `SGDatabase` from `SGApp-Servers`     | 8 min   |
| SSH to app servers failed           | Configuring SSH agent forwarding on the bastion            | 12 min  |

---

## CRITICAL ANALYSIS

### Security

[cite_start]The infrastructure adheres to basic best practices (private subnets for sensitive servers, segmented security groups). However, further improvements can be made:

* [cite_start]Adding AWS WAF in front of the Load Balancer would help block common web attacks (e.g., SQLI, XSS).
* [cite_start]Secrets Manager would be preferable for managing passwords, especially database credentials, instead of hardcoding them.
* [cite_start]Activating CloudTrail and VPC Flow Logs would enhance visibility and auditability of actions performed in the environment.

### High Availability

* [cite_start]Using an Auto Scaling Group would automate the addition or removal of EC2 instances based on load.
* [cite_start]For RDS, the Multi-AZ option has been enabled, which is a good point. [cite_start]However, one or more read replicas could be added to optimize read operations.

### Performance and Cost

Several optimizations are possible:

* [cite_start]Replacing some instances with Spot Instances for development or testing environments.
* [cite_start]Migrating EBS storage from GP2 to GP3 would save up to 20% without performance loss.
* [cite_start]Implementing CloudFront as a CDN for static files would reduce latency for end-users.

### Additional Recommendations

* [cite_start]Encryption of data at rest and in transit.
* [cite_start]Disabling root SSH login.
* [cite_start]Implementing HTTPS security headers on the application.