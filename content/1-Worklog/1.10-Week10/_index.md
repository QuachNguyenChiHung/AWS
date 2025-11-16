---
title: "Week 10 Worklog"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 10 Objectives:

* Implement user-side CRUD operations: edit user profile, add/delete orders.
* Hold a group meeting at Phuc Long to discuss project plan and connecting AWS Bedrock Chatbot to the backend.
* Test and deploy changes to staging for verification.


### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                         | Start Date | Completion Date | Reference Material                      |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | --------------------------------------- |
| 2   | - Add edit profile feature for users (avatar, address, contact info) <br> - Test frontend and API for profile update <br> - Write unit tests for update endpoint                         | 10/11/2025 | 10/11/2025      |                                         |
| 3   | - Implement user-side create/delete order functionality <br> - Add business validation and handle edge cases <br> - Write integration tests for order APIs                             | 10/12/2025 | 10/12/2025      |                                         |
| 4   | - Group meeting at Phuc Long: discuss Bedrock Chatbot integration with backend <br> - Draft API design, auth flow, and security considerations <br> - Record action items and timeline   | 10/13/2025 | 10/13/2025      |                                         |
| 5   | - Prototype chatbot integration with backend (mock/stub) <br> - Perform end-to-end chat flow tests <br> - Prepare integration guide and sample code for the team                     | 10/14/2025 | 10/14/2025      |                                         |

### Week 10 Achievements:

* User-side CRUD features implemented
  * Added profile edit (avatar, address, contact details) with frontend and API support
  * Implemented create/delete order flows with validation and error handling
  * Added unit and integration tests to cover user endpoints

* Team meeting at Phuc Long and Bedrock integration plan
  * Discussed Chatbot integration architecture and identified integration points and authentication flow
  * Produced an action list and timeline for implementing Bedrock-based Chatbot

* Basic prototype and documentation
  * Built a mock Chatbot-backend prototype and verified end-to-end chat flows
  * Prepared integration guide and example snippets for the team

* Improved user experience and integration readiness
  * Enabled users to manage orders directly from the UI
  * Laid groundwork for Chatbot-assisted shopping features
