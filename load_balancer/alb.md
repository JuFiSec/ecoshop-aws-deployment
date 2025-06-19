# Application Load Balancer

## STEP 5.2: Create Application Load Balancer

[cite_start]An Application Load Balancer (ALB) was deployed to distribute incoming web traffic among the application servers, improving fault tolerance and scalability.

* [cite_start]**Name:** `ecoshop-alb` 
* [cite_start]**Scheme:** Internet-facing 
* [cite_start]**IP address type:** IPv4 
* **Network mapping:**
    * [cite_start]**VPC:** `ecosop-vpc` 
    * **Mappings:**
        * [cite_start]`us-east-1a` <span class="math-inline">\\rightarrow</span> `ecosop-public-1a` 
        * [cite_start]`us-east-1b` <span class="math-inline">\\rightarrow</span> `ecosop-public-1b` 
* [cite_start]**Security groups:** `SG-Web-ALB` 
* **Listeners and routing:**
    * [cite_start]**Protocol:** HTTP 
    * [cite_start]**Port:** 80 
    * [cite_start]**Default action:** Forward to <span class="math-inline">\\rightarrow</span> `ecoshop-tg` 

[cite_start]![Load Balancer Active State](images/alb-active.png)