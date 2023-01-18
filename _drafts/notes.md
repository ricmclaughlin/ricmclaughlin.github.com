You want to scale up an AWS Site-to-Site VPN connection throughput, established between your on-premises data and AWS Cloud, beyond a single IPsec tunnel's maximum limit of 1.25 Gbps. What should you do?
Answer: Transit gateway

|            | Lambda     | Batch                                   |
|------------|------------|-----------------------------------------|
| Time limit | 15 minutes | no time limit                           |
| Runtime    | limited    | any runtime                             |
| Disk space | limited    | unlimited (EBS)                         |
| Compute    | Serverless | Serverless or EC2 (on-demand, Spot) |

|                  | Backup/Restore             | Pilot Light   | Warm Standby   | Multi-site    |
|------------------|----------------------------|---------------|----------------|---------------|
| RPO/RTO          | hours                      | minutes       | minutes        | near 0        |
| Approach         | backups; IaC               | live data     | active-passive | active-active |
| Restoration      |                            |               |                |               |
| EC2              |                            |               |                |               |
| Elastic Recovery |                            |               |                |               |
| EBS              | manual                     |               |                |               |
| RDS              |                            |               |                |               |
| Aurora           | automatic                  |               |                |               |
| Redshift         | snapshot cross-region copy |               |                |               |
| EFS/S3           | manual                     |               |                |               |
| Route53          |                            | health checks |                |               |
|                  |                            |               |                |               |