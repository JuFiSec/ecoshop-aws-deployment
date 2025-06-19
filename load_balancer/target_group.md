# PHASE 5: LOAD BALANCER

## STEP 5.1: Create Target Group

[cite_start]A target group was configured to route ALB traffic to the application servers.

* [cite_start]**Target type:** Instances 
* [cite_start]**Target group name:** `ecoshop-tg` 
* [cite_start]**Protocol:** HTTP 
* [cite_start]**Port:** 80 
* [cite_start]**VPC:** `ecosop-vpc` 
* **Health checks:**
    * [cite_start]**Health check protocol:** HTTP 
    * [cite_start]**Health check path:** `/index.php` 
* [cite_start]**Registered targets:** `ecosop-app-server-1` and `ecosop-app-server-2` 

[cite_start]![Healthy Target Group](images/tg-healthy.png)