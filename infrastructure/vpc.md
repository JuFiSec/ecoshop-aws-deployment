# PHASE 1: NETWORK INFRASTRUCTURE

## 1.1 Main VPC Creation

[cite_start]A Virtual Private Cloud (VPC) was created to host the entire Ecoshop infrastructure, providing an isolated and secure network environment.

**VPC Configuration:**
* [cite_start]**Name:** `ecosop-vpc` 
* [cite_start]**IPv4 CIDR:** `10.0.0.0/16` 
* [cite_start]**Tenancy:** Default 
* [cite_start]**Enable DNS hostnames:** Enabled 
* [cite_start]**Enable DNS resolution:** Enabled 

[cite_start]![VPC Creation Screenshot](images/vpc-overview.png)

## 1.2 Subnet Creation (6 subnets across 2 AZs)

[cite_start]Six subnets were configured and distributed across two Availability Zones (`us-east-1a` and `us-east-1b`) to ensure high availability and segmentation of the infrastructure.

| Name                        | Type   | CIDR           | AZ           | Usage     |
| :-------------------------- | :----- | :------------- | :----------- | :-------- |
| `ecosop-public-1a`          | Public | `10.0.1.0/24`  | `us-east-1a` | Web Tier  |
| `ecosop-public-1b`          | Public | `10.0.2.0/24`  | `us-east-1b` | Web Tier  |
| `ecosop-private-app-1a`     | Private | `10.0.10.0/24` | `us-east-1a` | App Tier  |
| `ecosop-private-app-1b`     | Private | `10.0.20.0/24` | `us-east-1b` | App Tier  |
| `ecosop-private-db-1a`      | Private | `10.0.100.0/24`| `us-east-1a` | DB Tier   |
| `ecosop-private-db-1b`      | Private | `10.0.200.0/24`| `us-east-1b` | DB Tier   |

[cite_start]![List of Created Subnets](images/subnets-list.png) 

## 1.3 Gateway Configuration

### Internet Gateway (IGW):

[cite_start]An Internet Gateway was created and attached to the VPC to enable communication between the VPC and the Internet.

* [cite_start]**Name:** `ecosop-igw` 
* [cite_start]**Attached to VPC:** `ecosop-vpc` 

[cite_start]![Internet Gateway Attached](images/igw-attached.png) 

### NAT Gateway:

[cite_start]A NAT Gateway was deployed in a public subnet to allow instances in private subnets to access the Internet (e.g., for software updates) without exposing these instances directly to the web.

* [cite_start]**Name:** `ecosop-nat-gw` 
* [cite_start]**Subnet:** `ecosop-public-1a` 
* [cite_start]**Connectivity type:** Public 
* [cite_start]**Allocate Elastic IP:** Enabled 

[cite_start]![NAT Gateway](images/nat-gateway.png) 

## 1.4 Route Table Configuration

### Public Route Table:

[cite_start]A public route table was configured to direct Internet traffic to the Internet Gateway and was associated with the public subnets.

* [cite_start]**Name:** `ecosop-public-rt` 
* [cite_start]**VPC:** `ecosop-vpc` 
* **Routes:**
    * [cite_start]`10.0.0.0/16` $\rightarrow$ Local 
    * [cite_start]`0.0.0.0/0` $\rightarrow$ `ecosop-igw` 
* [cite_start]**Associations:** `ecosop-public-1a`, `ecosop-public-1b` 

[cite_start]![Public Route Table](images/public-rt.png) 

### Private Route Table:

[cite_start]A private route table was configured to route outbound traffic from private subnets through the NAT Gateway.

* [cite_start]**Name:** `ecosop-private-rt` 
* [cite_start]**VPC:** `ecosop-vpc` 
* **Routes:**
    * [cite_start]`10.0.0.0/16` $\rightarrow$ Local 
    * [cite_start]`0.0.0.0/0` $\rightarrow$ `ecosop-nat-gw` 
* [cite_start]**Associations:** All private subnets 

[cite_start]![Private Route Table](images/private-rt.png)
