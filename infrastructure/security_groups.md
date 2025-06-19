# PHASE 2: SECURITY CONFIGURATION (Security Groups)

[cite_start]Four security groups were created to control inbound and outbound traffic for different resources, ensuring proper segmentation and protection.

## 2.1 Security Groups to Create

* **SG-Web (Load Balancer):**
    * [cite_start]**Name:** `SG-Web-ALB` 
    * [cite_start]**Description:** Security group for Application Load Balancer 
    * [cite_start]**VPC:** `ecosop-vpc` 
    * **Inbound rules:**
        * [cite_start]HTTP (80): `0.0.0.0/0` 
        * [cite_start]HTTPS (443): `0.0.0.0/0` 
    * [cite_start]**Outbound rules:** All allowed 

* **SG-App (Application Servers):**
    * [cite_start]**Name:** `SG-App-Servers` 
    * [cite_start]**Description:** Security group for application servers 
    * **Inbound rules:**
        * [cite_start]Port 8080: Source `SG-Web-ALB` 
        * [cite_start]SSH (22): Source `SG-Bastion` 
        * [cite_start]HTTP (80): Source `SG-Web-ALB` 
    * [cite_start]**Outbound rules:** All allowed 

* **SG-DB (Database):**
    * [cite_start]**Name:** `SG-Database` 
    * [cite_start]**Description:** Security group for RDS database 
    * **Inbound rules:**
        * [cite_start]MySQL (3306): Source `SG-App-Servers` 
    * [cite_start]**Outbound rules:** All allowed 

* **SG-Bastion (Administration):**
    * [cite_start]**Name:** `SG-Bastion` 
    * [cite_start]**Description:** Security group for Bastion server 
    * **Inbound rules:**
        * [cite_start]SSH (22): `[YOUR_PUBLIC_IP]/32` 
    * [cite_start]**Outbound rules:** All allowed 

[cite_start]![Security Groups Configuration](images/sg-config.png)