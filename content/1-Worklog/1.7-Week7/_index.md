---
title: "Week 7 Worklog"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 7 Objectives:

* Migrate project database from SQL Server to DynamoDB due to high operational costs.
* Review AWS services and architecture as part of OJT program.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                  | Start Date | Completion Date | Reference Material |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------ |
| 2   | - Analyze SQL Server operational costs and performance metrics <br> - Research DynamoDB pricing model and cost comparison <br> - Make decision to switch to DynamoDB and document rationale | 10/20/2025 | 10/20/2025      | https://aws.amazon.com/dynamodb/pricing/ |
| 3   | - Design DynamoDB table structure (partition/sort keys, GSI) <br> - Plan data migration strategy from SQL Server to DynamoDB <br> - Create DynamoDB table in AWS console                          | 10/21/2025 | 10/21/2025      | https://docs.aws.amazon.com/dynamodb/ |
| 4   | - Implement data migration scripts/tools (AWS DMS or custom) <br> - Execute migration and validate data integrity <br> - Update application code to use DynamoDB SDK                              | 10/22/2025 | 10/22/2025      | https://docs.aws.amazon.com/dynamodb/latest/developerguide/ |
| 5   | - Review all AWS services used in project (Cognito, API Gateway, S3, CloudWatch) <br> - Analyze current architecture and identify optimization opportunities <br> - Participate in OJT sessions and hands-on training     | 10/23/2025 | 10/23/2025      | https://aws.amazon.com/architecture/well-architected-framework/ |
| 6   | - Conduct final architecture review with team <br> - Practice OJT scenarios and hands-on exercises <br> - Prepare OJT documentation and feedback                                           | 10/24/2025 | 10/24/2025      | https://aws.amazon.com/certification/ |
### Week 7 Achievements:

* Successfully migrated project database from SQL Server to DynamoDB
  * Analyzed SQL Server costs and identified significant savings with DynamoDB on-demand pricing
  * Designed DynamoDB tables with appropriate partition/sort keys and Global Secondary Indexes
  * Executed data migration using AWS Database Migration Service, ensuring 100% data integrity
  * Updated application code to use DynamoDB SDK, achieving seamless transition

* Comprehensive review of AWS services and architecture
  * Evaluated current AWS service usage (Cognito for auth, API Gateway for routing, S3 for storage, CloudWatch for monitoring)
  * Identified architecture optimization opportunities including reserved instances and auto-scaling
  * Participated actively in OJT program with hands-on training and practical exercises
  * Developed strong understanding of AWS services through real-world application

* Cost optimization and architectural improvements
  * Achieved estimated 60-70% cost reduction by switching to DynamoDB from SQL Server
  * Enhanced system scalability and performance with NoSQL database design
  * Strengthened understanding of AWS Well-Architected Framework principles
  * Improved team's ability to make data-driven decisions for cloud infrastructure
