# azure-infrastructure-automation
documentation for Terraform modules todeploy a Standard Enterprise Landing Zone

Project Overview

This project demonstrates the architecture and deployment of a Warm Standby (Pilot Light) Disaster Recovery strategy for a mission-critical web application. It addresses the business need for high availability and minimal data loss during a regional Azure outage.
•	RTO (Recovery Time Objective): < 30 Minutes
•	RPO (Recovery Point Objective): < 5 Minutes

Architecture Diagram
<img width="1024" height="559" alt="1777175740572" src="https://github.com/user-attachments/assets/57b5345c-1434-4b43-b4ef-eced3adb9b61" />

Core Components:
•	Traffic Management: Azure Traffic Manager / Front Door for automated health-probe-based failover.
•	Compute: Azure App Services deployed in a primary region with a secondary "Pilot Light" instance in a paired region.
•	Data Persistence: Azure SQL Failover Groups to ensure asynchronous data replication.
•	Infrastructure: 100% defined via Terraform for rapid environment replication.

Key Features
•	Automated Failover: Configured endpoint monitoring to redirect 100% of traffic to the secondary region if the primary heartbeat fails.
•	IaC Resilience: Used Terraform variables to ensure the entire stack can be redeployed to a third "unplanned" region in under 15 minutes.
•	Security & Compliance: Implemented Azure Key Vault replication to ensure secrets and SSL certificates are available across regions during a failover.

Technical Implementation (The "How-To")
1.	Replication: Setup azurerm_sql_failover_group to manage database synchronization.
2.	Monitoring: Deployed Azure Monitor alerts to notify the Ops team via Webhooks when a failover event is triggered.
3.	Testing: Included a failover_test.sh script that simulates a regional outage by disabling the primary endpoint to verify Traffic Manager's response.

Business Impact
•	Risk Mitigation: Eliminated single-point-of-failure for the "PAyback" refund system.
•	Cost Efficiency: Utilized a "Pilot Light" model instead of "Multi-Site Active-Active," reducing idle compute costs by 60% while still meeting RTO requirements.
<img width="468" height="654" alt="image" src="https://github.com/user-attachments/assets/de13c810-29ff-4891-9bff-95c510428c4f" />
