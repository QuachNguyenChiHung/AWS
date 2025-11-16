---
title: "Week 9 Worklog"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 9 Objectives:

* Migrate project from RDS SQL Server to DynamoDB to reduce operational costs.
* Complete admin side features with basic CRUD operations for the clothes shop project.
* Learn basic Linux commands to manage EC2 Ubuntu instances.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                          | Start Date  | Completion Date | Reference Material                                                                 |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | --------------- | ---------------------------------------------------------------------------------- |
| 2   | - Analyze and plan migration from RDS SQL Server to DynamoDB <br> - Compare cost models and expected savings <br> - Document migration plan and rollback strategy                             | 11/03/2025  | 11/03/2025      | https://aws.amazon.com/dynamodb/pricing/                                           |
| 3   | - Design DynamoDB table schema (partition/sort keys, GSIs) <br> - Implement data migration scripts (AWS DMS or custom ETL) <br> - Start data migration and validate samples                  | 11/04/2025  | 11/04/2025      | https://docs.aws.amazon.com/dynamodb/                                              |
| 4   | - Complete migration and verify data integrity <br> - Update backend to use DynamoDB SDK / client <br> - Monitor performance and adjust capacity or indexes as needed                     | 11/05/2025  | 11/05/2025      | https://docs.aws.amazon.com/dms/latest/userguide/                                 |
| 5   | - Finish admin side features: implement full CRUD for products, categories, orders <br> - Write unit and integration tests for admin APIs <br> - Deploy admin changes to staging            | 11/06/2025  | 11/06/2025      | https://spring.io/guides/gs/accessing-data-jpa/                                    |
| 6   | - Learn and practice basic Linux commands on EC2 Ubuntu (ssh, systemctl, journalctl, apt, file permissions) <br> - Verify application runs correctly on instance and document commands   | 11/07/2025  | 11/07/2025      | https://linuxcommand.org/; https://help.ubuntu.com/community/UsingTheTerminal      |

### Week 9 Achievements:

* Successfully migrated project database from RDS SQL Server to DynamoDB
  * Completed cost analysis and confirmed significant operational savings
  * Designed DynamoDB schema and executed migration with data validation checks
  * Updated backend to use DynamoDB client; verified application functionality

* Completed admin side functionality with full CRUD
  * Implemented create/read/update/delete for products, categories, and orders
  * Added unit and integration tests to cover admin APIs
  * Deployed admin features to staging for verification

* Gained practical Linux skills for EC2 Ubuntu administration
  * Used SSH to access instances and managed services with `systemctl` and `journalctl`
  * Installed packages with `apt`, managed file permissions, and checked logs
  * Documented common commands to operate EC2 instances reliably

* Improved project cost-efficiency and deployment readiness
  * Transition to DynamoDB improved scalability and lowered expected costs
  * Admin improvements and Linux know-how increased team's ability to operate and maintain the system
