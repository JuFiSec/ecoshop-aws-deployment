# Application Servers and Connectivity Tests

## 3.2 Application Server Creation

[cite_start]Two application servers were launched in private subnets, configured to run a simple PHP application.

**App Server 1:**
* [cite_start]**Name:** `ecosop-app-server-1` 
* [cite_start]**AMI:** Amazon Linux 2 
* [cite_start]**Instance type:** `t3.micro` 
* [cite_start]**Subnet:** `ecosop-private-app-1a` 
* [cite_start]**Public IP:** Disable 
* [cite_start]**Security group:** `SG-App-Servers` 
* **User Data:**
    ```bash
    #!/bin/bash
    yum update -y
    yum install -y httpd php mysql
    systemctl start httpd
    systemctl enable httpd
    echo "<?php" > /var/www/html/index.php
    echo "echo 'EcoShop Application - Server:'. gethostname();" >> /var/www/html/index.php
    echo "?>" >> /var/www/html/index.php
    [cite_start]``` 

**App Server 2:**
* **Name:** `ecosop-app-server-2` 
* **AMI:** Amazon Linux 2 
* **Instance type:** `t3.small` 
* **Subnet:** `ecosop-private-app-1b` 
* **Public IP:** Disable 
* **Security group:** `SG-App-Servers` 
* **User Data:** (Same configuration as Server 1) 

![Application Server Instances Running](images/instances-running.png) 

## 3.3 Connectivity Tests

### Connecting to the Bastion:

SSH connection to the Bastion Host was tested and successful, confirming access to the private network.

```bash
chmod 400 /home/jufisec/ecosop-bastion.pem
ssh -i ecosop-bastion.pem ec2-user@54.247.7.116