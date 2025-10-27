---
title: "Week 6 Worklog"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 6 Objectives:

* Learn and practice Amazon Cognito (User Pools, App Clients, auth flows).
* Study Spring Data JpaRepository for CRUD, query methods, and pagination.
* Discuss and shortlist AWS services to use for the project with the team.
* Diagnose and fix Spring Boot backend API errors returning HTTP 500.
### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                  | Start Date | Completion Date | Reference Material |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------ |
| 2   | - Learn Amazon Cognito basics (User Pool, App Client, domains) <br> - Try sign-up/sign-in flows using Hosted UI or SDK <br> - Inspect ID/Access tokens and verify JWT signature                     | 10/13/2025 | 10/13/2025      | https://docs.aws.amazon.com/cognito/ |
| 3   | - Implement Spring Data JpaRepository for core entities <br> - Add query methods, Pageable, and sorting <br> - Write unit tests for repository CRUD                                                  | 10/14/2025 | 10/14/2025      | https://docs.spring.io/spring-data/jpa/reference/ |
| 4   | - Team discussion: choose AWS services for project (Cognito, API Gateway/ALB, RDS for SQL Server, S3, CloudWatch) <br> - Draft high-level architecture diagram and data flow                       | 10/15/2025 | 10/15/2025      | https://wa.aws.amazon.com/ |
| 5   | - Reproduce Spring Boot backend 500 error <br> - Add logging and exception handling (@ControllerAdvice) <br> - Fix service/repository null cases and validation, return correct status codes       | 10/16/2025 | 10/16/2025      | https://docs.spring.io/spring-boot/reference/web/servlet.html#web.servlet.spring-mvc.exception-handling |
| 6   | - Add integration tests for critical APIs <br> - Verify 2xx/4xx responses via Postman/newman <br> - Update README and monitoring/health checks                                                      | 10/17/2025 | 10/17/2025      | https://spring.io/guides/gs/testing-web/ |
### Week 6 Achievements:

* Amazon Cognito: set up and validated authentication flows
  * Created a User Pool and App Client; configured password policy and (optional) MFA
  * Tested sign-up/sign-in using Hosted UI/SDK and retrieved ID/Access tokens
  * Verified JWTs using JWKs endpoint; confirmed scopes and claims for API access

* Spring Data JPA: implemented repositories and improved data access
  * Built repositories extending JpaRepository with derived query methods and pagination
  * Wrote unit tests for CRUD operations and ensured transactional consistency
  * Reduced boilerplate and improved readability of data layer

* Team decision on AWS services for the project
  * Selected Cognito for auth, Amazon RDS for SQL Server for relational data, S3 for object storage
  * Considered API Gateway vs. ALB for routing; chose based on current integration needs
  * Planned monitoring with CloudWatch and structured logs for APIs

* Fixed Spring Boot backend HTTP 500 issues and hardened error handling
  * Identified root causes (null handling, validation, unhandled exceptions) via logs and traces
  * Added global exception handling with @ControllerAdvice and consistent error responses
  * Introduced input validation (@Valid) and proper ResponseEntity status mapping
  * Created integration tests to prevent regressions; validated endpoints return correct 2xx/4xx
