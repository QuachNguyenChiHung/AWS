---
title: "Week 11 Worklog"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 11 Objectives:

* Add credit card transaction capability to the project to enable user payments securely.
* Attend AWS Cloud Mastery Series (Bitexco Financial Tower) and apply DevOps/CI-CD and observability learnings to the project.
* Complete integration, testing, and deployment of payment flows and document the implementation.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Attend AWS Cloud Mastery Series at Bitexco Financial Tower (full day) <br> - Sessions covered: DevOps mindset, CI/CD (CodeCommit/CodeBuild/CodeDeploy/CodePipeline), IaC (CloudFormation/CDK), containers (ECR/ECS/EKS), observability (CloudWatch/X-Ray), and best practices. | 11/17/2025 | 11/17/2025      |                                         |
| 3   | - Design and integrate credit card payment flow (choose gateway / tokenization) <br> - Implement secure server-side payment endpoint and token handling <br> - Ensure sensitive data is not stored in cleartext | 11/18/2025 | 11/18/2025      |                                         |
| 4   | - Implement client-side payment UI and connect to backend tokenization API <br> - Add server-side validation, logging and retry/error handling <br> - Add monitoring for payment failures and success rates | 11/19/2025 | 11/19/2025      |                                         |
| 5   | - End-to-end testing of payment flows (success, decline, network failures) <br> - Run security checks and ensure compliance considerations (tokenization, TLS) <br> - Fix reported issues from tests | 11/20/2025 | 11/20/2025      |                                         |
| 6   | - Deploy payment features to staging <br> - Demo to team and collect feedback <br> - Document implementation, runbooks and monitoring alerts for production rollout                         | 11/21/2025 | 11/21/2025      |                                         |

### Week 11 Achievements:

* Attended AWS Cloud Mastery Series (Bitexco Financial Tower) on 11/17/2025
  * Participated in sessions covering DevOps mindset, CI/CD pipelines (CodeCommit, CodeBuild, CodeDeploy, CodePipeline), IaC (CloudFormation, CDK), container services (ECR/ECS/EKS), and observability (CloudWatch, X-Ray).
  * Gained actionable ideas for improving our CI/CD workflows and monitoring strategy.

* Implemented secure credit card payment capability
  * Designed and integrated payment flow using tokenization (no raw card storage)
  * Implemented backend payment endpoints, client integration, and server-side validation
  * Added logging, retries, and monitoring for payment success/failure rates

* Verified payment flows with end-to-end tests and security checks
  * Tested success, decline, and network-failure scenarios
  * Applied TLS, tokenization and basic compliance best practices

* Deployed payment features to staging and documented the integration
  * Prepared runbooks, monitoring alerts, and integration guide for the team
  * Demoed features to the team and collected feedback for production rollout
