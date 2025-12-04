---
title: "Event 2"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your report, including this warning.
{{% /notice %}}

# Summary Report: “AWS Well-Architected: Security”

### Event Objectives

- Introduce AWS Well-Architected Security and its core principles: Least Privilege, Zero Trust, Defense-in-Depth
- Explain the Shared Responsibility Model and common cloud threats relevant to Vietnam
- Present practical Identity & Access Management patterns and multi-account guardrails (IAM Identity Center, SCPs, permission boundaries)
- Demonstrate detection, logging and continuous monitoring practices (CloudTrail, GuardDuty, Security Hub, centralized logging)
- Share network and workload protection guidance (VPC segmentation, Security Groups/NACLs, WAF/Shield, network firewall)
- Provide data protection best practices (KMS, encryption at-rest/in-transit, Secrets Manager, data classification)
- Offer incident response playbooks and automation patterns for common cloud incidents (isolation, evidence collection, automated containment)
- Deliver a practical security learning roadmap and actionable takeaways for local enterprises

### Speakers

- **Jignesh Shah** – Director, Open Source Databases
- **Erica Liu** – Sr. GTM Specialist, AppMod
- **Fabrianne Effendi** – Assc. Specialist SA, Serverless Amazon Web Services

### Key Highlights (Security)

#### Identity & Access Management (IAM)

- Prefer short-lived credentials and role-based access; avoid long-term static keys
- Centralize user access with IAM Identity Center (SSO) and permission sets for predictable access control
- Apply SCPs and permission boundaries to enforce guardrails across accounts
- Enforce MFA, credential rotation and use Access Analyzer to validate policies
- Practical: validate IAM policies and simulate access during design reviews and CI checks

#### Detection

- Operate org-level CloudTrail and integrate with GuardDuty and Security Hub for unified threat detection
- Instrument logging across network, load balancers, storage and applications (VPC Flow Logs, ALB logs, S3 access logs, application traces)
- Use EventBridge + Lambda for automated alerting and initial containment workflows
- Adopt Detection-as-Code: store detection rules in source control and deploy via pipeline

#### Infrastructure Protection

- Design VPC segmentation and place workloads in private subnets where possible
- Use Security Groups for instance/task-level controls and NACLs for coarse subnet-level protections
- Protect edges and web layers with WAF, AWS Shield and Network Firewall for DDoS and OWASP-style protections
- Harden workloads: secure AMIs/containers, patching, host-level monitoring and runtime scanning

#### Data Protection

- Use AWS KMS with explicit key policies, grants and scheduled rotation for asymmetric and symmetric keys
- Encrypt data at-rest and in-transit for S3, EBS, RDS, DynamoDB and service endpoints
- Centralize secrets with Secrets Manager or Parameter Store and automate secret rotation where possible
- Perform data classification and apply least-privilege access patterns for sensitive data

#### Incident Response

- Maintain IR playbooks for common scenarios: compromised keys, public S3 exposure, and workload compromise
- Prepare evidence collection steps (snapshots, log exports, VPC traffic captures) and isolation procedures
- Automate safe containment actions (Lambda/Step Functions) for repeatable first-response tasks
- Conduct tabletop exercises and measure MTTR; iterate playbooks based on lessons learned

#### Local & Operational Context

- Consider local threat patterns and compliance expectations in Vietnam when prioritizing controls
- Emphasize pragmatic, high-impact controls first (IAM hygiene, logging, encryption, IR readiness)



### Key Takeaways (Security)

#### Security Mindset

- Adopt a security-first mindset: design systems assuming compromise and apply defense-in-depth
- Prioritize Least Privilege: think identity-first when designing access and data flows
- Treat detection and IR as first-class requirements, not optional add-ons

#### High-Impact Operational Recommendations

- Start with IAM hygiene (short-lived credentials, SSO, permission sets, MFA) and centralized logging
- Ensure org-level CloudTrail and automated alerts (GuardDuty/Security Hub) are enabled and monitored
- Implement KMS-backed encryption and centralized secrets management before wide rollout

#### Architecture & Controls

- Segment networks and minimize public surface area; apply layered controls (WAF, Shield, Firewall)
- Use least-privilege task roles for compute (EC2/ECS/EKS/Lambda) and secure container images
- Automate detection and response where safe; instrument observability across infra and apps

#### Incident Preparedness

- Maintain clear IR playbooks and automate repeatable containment steps (Lambda/Step Functions)
- Regularly practice tabletop exercises and measure Mean Time To Recover (MTTR)
- Preserve forensics-ready data (logs, snapshots) to speed investigation and remediation

#### Roadmap & Learning

- Build a practical roadmap: IAM → Logging/Detection → Data Protection → IR → Continuous improvement
- Invest in role-based learning (Security Specialty, SA Pro) and hands-on labs focused on IR and detection
- For Vietnam-specific contexts, align controls with local compliance and common threat patterns

### Applying to Work

- Operationalize lessons: add IAM policy checks in CI, enable org-level logging, and schedule IR drills
- Prioritize quick wins: enforce MFA, enable CloudTrail, rotate keys, and register critical alerts
- Track progress with simple metrics (number of IAM principals with MFA, % encrypted volumes, MTTR)

### Event Experience

Attending the **AWS Well-Architected Security Pillar** session provided a concise, practical roadmap to improve cloud security posture with tools, playbooks, and measurable operational steps. Speakers focused on pragmatic controls you can apply immediately, demonstrated policy validation and detection patterns, and emphasized readiness through playbooks and automation.


#### Networking and discussions

- Practitioners and speakers exchanged concrete patterns for IAM, account governance, and multi-account guardrails (permission sets, SCPs, permission boundaries).
- Peers shared operational experiences for Detection-as-Code, how they structure CloudTrail aggregation and prioritize GuardDuty findings for alert fatigue reduction.
- Attendees discussed practical IR automation approaches (EventBridge + Lambda, Step Functions) and trade-offs for safe auto-remediation.
- Local participants highlighted common constraints in Vietnam: compliance nuances, constrained SOC resources, and the need for pragmatic, high-impact controls.

#### Lessons learned

- IAM hygiene and org-level logging provide the highest immediate security ROI: enable SSO, MFA, short-lived credentials and centralized CloudTrail.
- Detection is most effective when backed by good telemetry: instrument VPC Flow Logs, ALB/S3 logs and application traces, and treat detection rules as code.
- Automate repeatable containment steps, but validate safety: automated responses (Lambda/Step Functions) speed containment when well-tested.
- Encrypt-by-default and centralized key management (KMS) reduce data exposure risk; manage secrets centrally and rotate frequently.
- Network segmentation and least-privilege task roles reduce blast radius; combine with WAF/Shield for edge protection.
- Regular IR exercises and tabletop drills materially reduce MTTR and improve confidence in playbooks and automation.

#### Some event photos
*Add your event photos here*  

> Overall, the event not only provided technical knowledge but also helped me reshape my thinking about application design, system modernization, and cross-team collaboration.
