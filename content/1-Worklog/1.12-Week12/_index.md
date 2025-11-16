---
title: "Week 12 Worklog"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.12. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 12 Objectives:

* Review the project end-to-end and fix any bugs prior to mentor presentation.
* Deploy backend, database, and frontend into AWS (staging and production as appropriate) with repeatable CI/CD pipelines.
* Prepare demo, runbooks, and monitoring so mentors can review a stable, observable system.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 1   | - Code review and bug triage: run static analysis, prioritize critical/major bugs, create fix plan                                                                                                     | 11/24/2025 | 11/24/2025      |                                         |
| 2   | - Fix high-priority backend issues: unit & integration tests, database query optimizations, handle error paths and edge cases                                                                              | 11/25/2025 | 11/25/2025      |                                         |
| 3   | - Prepare deployment artifacts: containerize services, build artifacts, verify infra-as-code templates (CloudFormation/CDK), and configure CI pipelines for staging deploys                          | 11/26/2025 | 11/26/2025      |                                         |
| 4   | - Deploy backend & database to AWS staging (or production if ready): configure Secrets Manager/SSM, load balancer / ALB, autoscaling, and backups; validate connections and migrations             | 11/27/2025 | 11/27/2025      |                                         |
| 5   | - Deploy frontend (S3 + CloudFront or hosting), run end-to-end smoke tests, run performance and load checks, prepare demo script and present rehearsal to team                                       | 11/28/2025 | 11/28/2025      |                                         |
### Week 12 Achievements:

* Completed code review and resolved critical/major bugs blocking presentation.

* Backend and database deployed to AWS staging (and production where approved)
  * CI/CD pipelines validated for automated builds and deploys to staging
  * Database migrations applied and backups configured

* Frontend deployed and configured behind CDN (S3 + CloudFront) with routing and TLS

* Monitoring and observability configured
  * Application logs centralized, metrics and basic dashboards created, and alerts configured for critical failures

* Performed end-to-end smoke tests and a rehearsal demo for mentors
  * Prepared demo script, runbook, and rollback steps

* Project is presentation-ready with documented deployment steps and testing results.
