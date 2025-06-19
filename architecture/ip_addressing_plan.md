# DOCUMENTED ARCHITECTURE

## Deployed Architecture Diagram

![Deployed Architecture Diagram](images/ecoshop_architecture.mermaid)

## Detailed IP Addressing Plan

| Component         | CIDR            | Zone       | Type    | Available IPs | Reserved IPs | Usage                   |
| :---------------- | :-------------- | :--------- | :------ | :------------ | :----------- | :---------------------- |
| Main VPC          | `10.0.0.0/16`   | Multi-AZ   | Public  | 65,536        | 5 per subnet | Global Network          |
| Public 1A         | `10.0.1.0/24`   | `us-east-1a`| Public  | 251           | 5            | Web Tier, Bastion       |
| Public 1B         | `10.0.2.0/24`   | `us-east-1b`| Public  | 251           | 5            | Web Tier, ALB           |
| Private App 1A    | `10.0.10.0/24`  | `us-east-1a`| Private | 251           | 5            | App Tier, Server 1      |
| Private App 1B    | `10.0.20.0/24`  | `us-east-1b`| Private | 251           | 5            | App Tier, Server 2      |
| Private DB 1A     | `10.0.100.0/24` | `us-east-1a`| Private | 251           | 5            | Database                |
| Private DB 1B     | `10.0.200.0/24` | `us-east-1b`| Private | 251           | 5            | Database                |



## Specific Assigned IP Addresses

* **Bastion Host:** `10.0.1.x` (in `ecosop-public-1a`)
* **NAT Gateway:** `10.0.1.y` (in `ecosop-public-1a`)
* **App Server 1:** `10.0.10.90` (in `ecosop-private-app-1a`)
* **App Server 2:** `10.0.20.66` (in `ecosop-private-app-1b`)
* **RDS Instance:** `10.0.100.x` and `10.0.200.x` (Multi-AZ)

## Network Flow Matrix

| Source      | Destination   | Port      | Protocol    | Direction                 | Security Group                  |
| :---------- | :------------ | :-------- | :---------- | :------------------------ | :------------------------------ |
| Internet    | ALB           | 80, 443   | HTTP/HTTPS  | Inbound                   | `SGWeb-ALB`                     |
| ALB         | App Servers   | 80, 8080  | HTTP        | Outbound <span class="math-inline">\\rightarrow</span> Inbound | `SGWeb-ALB` <span class="math-inline">\\rightarrow</span> `SGApp-Servers` |
| Your IP     | Bastion       | 22        | SSH         | Inbound                   | `SGBastion`                     |
| Bastion     | App Servers   | 22        | SSH         | Outbound <span class="math-inline">\\rightarrow</span> Inbound | `SGBastion` <span class="math-inline">\\rightarrow</span> `SGApp-Servers` |
| App Servers | RDS           | 3306      | MySQL       | Outbound <span class="math-inline">\\rightarrow</span> Inbound | `SGApp-Servers` <span class="math-inline">\\rightarrow</span> `SGDatabase` |
| App Servers | Internet      | 80, 443   | HTTP/HTTPS  | Outbound via NAT          | `SGApp-Servers`                 |
| App Servers | Internet      | 53        | DNS         | Outbound via NAT          | `SGApp-Servers`                 |

