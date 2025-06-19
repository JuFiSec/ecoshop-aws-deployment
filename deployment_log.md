# DEPLOYMENT LOG

## Issues Encountered and Solutions

| Issue                               | Solution Applied                                           | Time    |
| :---------------------------------- | :--------------------------------------------------------- | :------ |
| User Data not executing             | Checking logs in `/var/log/cloud-init-output.log`          | 10 min  |
| Unhealthy targets in ALB            | Modifying health check path to `/index.php`                | 5 min   |
| No internet access from app servers | Verifying route to NAT Gateway in the private table        | 15 min  |
| RDS connection failed               | Opening port 3306 in `SGDatabase` from `SGApp-Servers`     | 8 min   |
| SSH to app servers failed           | Configuring SSH agent forwarding on the bastion            | 12 min  |

[cite_start][cite: 49]