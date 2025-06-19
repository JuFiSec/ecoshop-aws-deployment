# PHASE 3: SERVER DEPLOYMENT

## 3.1 Bastion Host Creation

[cite_start]A Bastion host was deployed in a public subnet to enable secure SSH access to private instances.

* [cite_start]**Name:** `ecosop-bastion` 
* [cite_start]**AMI:** Amazon Linux 2 
* [cite_start]**Instance type:** `t3.micro` 
* [cite_start]**Key pair:** [Create new key or use existing] 
* **Network settings:**
    * [cite_start]**VPC:** `ecosop-vpc` 
    * [cite_start]**Subnet:** `ecosop-public-1a` 
    * [cite_start]**Public IP:** Enable 
    * [cite_start]**Security group:** `SG-Bastion` 

[cite_start]![Bastion Instance Running](images/instances-running.png)