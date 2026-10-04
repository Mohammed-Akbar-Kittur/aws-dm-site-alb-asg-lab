# AWS-Based DM Site with ALB and Auto Scaling

<img width="1920" height="1280" alt="IMG-20261004-WA1832" src="https://github.com/user-attachments/assets/5e9dfbb5-5b21-4d99-8170-9bdab12a8196" />

> **Lab Status:** Practiced and decommissioned. Preserved for portfolio.

## Overview
Disaster Management site with fault-tolerant architecture using ALB + ASG to ensure availability during high traffic.

## Architecture
- VPC: 10.0.0.0/16
- Public: 10.0.1.0/24 (1a), 10.0.2.0/24 (1b) - ALB Nodes
- Private: 10.0.3.0/24, 10.0.4.0/24 - EC2 App Servers

## Key Configurations
- ALB: HTTP:80, Target Group /health, Cross-Zone ON
- SGs: ALB-SG allows 80 from 0.0.0.0/0, EC2-SG allows 80 from ALB-SG only
- ASG: Desired 3, Min 2, Max 6, Health check HTTP /health every 30s
- Scaling: CPU > 50% for 5 min -> +1, CPU < 30% -> -1

## Tech Stack
EC2, ALB, ASG, VPC, Route53, CloudWatch

## Validation
- Health check validation: `curl http://ALB-DNS/health` -> {"status":"healthy"}
- Checked target health: 3/3 instances InService in console
- Simulated AZ failure: Stopped all instances in 1a -> traffic auto-routed to 1b in 30 sec
- Scaling validation: Stress test `stress --cpu 2` -> New instance launched in 3 min

## Outcome
- Highly available across 2 AZs - no single point of failure
- Secure - EC2 in private subnets, no public IP, only via ALB
- Cost-optimized - scales in when traffic low, saved ~55% compute cost

---
## Author
**Mohammed Akbar Kittur**
DevOps Engineer | AWS | Linux | Terraform | Docker | Kubernetes
📍 Bangalore, Karnataka
🔗 [GitHub](https://github.com/Mohammed-Akbar-Kittur) | [LinkedIn](https://linkedin.com/in/mohammed-akbar-kittur)
