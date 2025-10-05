---
title: "Blog 6"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.6. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# From Flexibility to Framework: Enforcing Tool Order in MCP Servers

The **Model Context Protocol (MCP)** was created to bring consistency to how applications interact with Generative AI models. Instead of piecing together separate integrations for every model or hosting environment, MCP provides a **standardized communication layer**.

## Introduction: Why MCP is Important



This standardization makes it powerful for AI applications, especially those that rely on agents using external tools. But with this flexibility comes a gap: **MCP doesn’t natively enforce the sequence in which tools should be used**. In scenarios like **Infrastructure as Code (IaC)**, this lack of ordering can lead to critical workflow failures.


---

## The Challenge: Why Tool Ordering Matters

MCP lets an LLM (via an agent) call any available tool—such as sending an email or fetching weather data—without restrictions on order. But in practice, many tools have dependencies.

### Common Dependency Scenarios

* **Chained calls** – One tool must run before another.

  * Example: `getOrderId()` must precede `getOrderDetail()`.
  * Example: `fetch_weather_data()` must run before `send_email()`.
* **MCP’s Default Behavior** – All tools act as independent functions. The framework doesn’t know which one should come first.

This is especially problematic in structured processes like **CI/CD pipelines**, where every stage must run in a strict order:

1. A pull request triggers the pipeline.
2. Linting, unit tests, and security checks are run.
3. A failure halts the workflow immediately.

Add to this the **non-deterministic behavior of LLMs**—where identical prompts don’t always yield identical outputs—and you see the need for **a mechanism to enforce order without sacrificing flexibility**.

---

## Understanding MCP Communication

MCP defines three lifecycle phases:

1. **Initialization** – The client and server negotiate protocol version and capabilities.
2. **Operation** – The client invokes tools and processes responses.
3. **Shutdown** – The connection closes gracefully.

During **initialization**, the MCP server shares available tools, their schemas, and usage instructions. This schema data allows the AI agent to learn not only what tools exist, but also what inputs and outputs they expect.

For instance, a tool schema might require a `Result from get_aws_session_info()` or a `security_scan_token`. By exposing these requirements early, MCP creates an opportunity to guide workflows.

---

## The Solution: Token-Based Orchestration

Because MCP doesn’t provide direct dependencies between tools, the **CCAPI MCP server** introduces a **token messenger pattern**.

Instead of tools passing information to each other, the server issues **cryptographically secure tokens** that act as proof a dependency was satisfied.

### How It Works

#### 1. Enhanced Functions with `@mcp.tool()`

* Every tool is wrapped with input validation rules and schema definitions.
* Documentation makes explicit what each tool requires.
* Example: `generate_infrastructure_code()` won’t run unless a valid `session_token` is supplied.

#### 2. Dependency Discovery at Initialization

* The server publishes a full dependency map during startup.
* The AI agent learns which parameters (and tokens) are needed before a tool can run.
* Example sequence:

  ```
  get_aws_session_info() → generate_infrastructure_code() → run_checkov() → create_resource()
  ```

#### 3. Server-Side Token Validation

* Tokens are stored in memory (`_workflow_store`) and expire after use.
* Tools consume tokens and generate new ones, forming a chain.
* If a token is missing, expired, or reused, the operation fails instantly.

This ensures tools follow the intended sequence without the LLM “guessing” the right order.

---

## Example Workflow

1. `get_aws_session_info()` → generates `session_token`.
2. `generate_infrastructure_code()` → validates `session_token`, consumes it, and creates `generated_code_token`.
3. `run_checkov()` → requires `generated_code_token`, then produces `security_scan_token`.
4. `create_resource()` → executes only if `security_scan_token` is valid.

This creates a **cryptographic chain of trust** that enforces workflow integrity.

---

## Challenges and Limitations

### 1. Session Management

* Tokens are bound to sessions and reset when sessions expire.
* This mirrors **AWS credential expiration**, aligning security with workflow lifecycles.

### 2. Concurrent Sessions

* Each workflow runs independently, avoiding cross-contamination between agents.

### 3. Persistence

* Tokens are memory-bound for security.
* Persistent storage is possible but generally unnecessary, as tokens are meant to be short-lived.

---

## Looking Ahead: The Future of MCP

While token orchestration works today, the MCP protocol could evolve to support deterministic workflows more natively.

* **Schema-Defined Dependencies**

  ```python
  @mcp.tool(depends_on=["run_checkov"])
  ```
* **Lifecycle Hooks** – Similar to Claude Code’s hooks, these would enforce guaranteed ordering inside the framework.

For IaC, CI/CD, and other deterministic domains, these enhancements will be essential for adoption at scale.

---

## Conclusion

MCP’s strength lies in its flexibility, but complex enterprise workflows require **predictability and control**.

By adding **token-based orchestration** to the **CCAPI MCP server**:

* Enforced strict tool ordering.
* Secured workflows with server-side validation.
* Preserved MCP’s flexible architecture.

This approach shows how MCP can move from **flexibility to framework**—supporting both innovation and the strict reliability required for cloud infrastructure management.

The story of MCP is still unfolding, but token-based orchestration offers a clear path forward: **from experimentation to enterprise-grade operations**.
