# Securing Openclaw: A Zero Trust Approach

> **The Art of Possible**: Enabling secure, productive AI adoption through intelligent guardrails, not barriers.
> 

**Author**: Henry Shen

**Role**: Principal Sales Engineer, AI Security, Zscaler

**Company**: Zscaler (personal views, not official positions)

[LinkedIn](https://www.linkedin.com/in/haoyunshen) | [X](https://x.com/henryhaoyunshen) 



## Disclaimer

This article represents my personal analysis and professional opinions on AI agent security architecture. While I am employed by Zscaler, this content:

- Does not reflect official Zscaler product strategy or roadmaps
- Is not reviewed or approved by Zscaler
- Represents my own research and industry observations
- References Zscaler's publicly available Zero Trust principles

For Zscaler's official security solutions, visit [zscaler.com](https://www.zscaler.com/).

---

## Vision

Instead of asking "How do we block OpenClaw?", we ask:

> **"How do we enable organizations to harness OpenClaw's productivity potential while maintaining enterprise-grade security, compliance, and control?"**
> 

This repository presents a comprehensive security architecture for safely deploying OpenClaw in enterprise environments. Rather than treating AI agents as threats to be contained, we architect a **guarded enablement** approach that preserves OpenClaw's capabilities while providing granular visibility, control, and protection.

---

## The OpenClaw Paradox: Why Blocking Fails

### The Power of OpenClaw

OpenClaw represents a fundamental shift in how AI agents operate within enterprise environments. Unlike traditional software that follows predictable patterns, OpenClaw is designed to be **adaptive, multi-channel, and deeply integrated** with user workflows. This creates both unprecedented productivity gains and unique security challenges.

### Most Powerful Use Cases

| Use Case | Productivity Impact | Why It's Hard to Block |
| --- | --- | --- |
| **Cross-Platform Automation** | Unifies Apple Notes, Reminders, Things 3, Notion, Obsidian, Trello via single chat interface | Operates across consumer and enterprise apps simultaneously |
| **Shell Command Execution** | Automates DevOps tasks, server management, CI/CD pipelines | Uses same commands as legitimate developers; indistinguishable traffic |
| **Browser Automation** | Scrapes data, fills forms, executes web workflows | Mimics human browsing patterns; hard to detect bot vs. human |
| **Multi-Channel Communication** | WhatsApp, Telegram, Slack, Discord, Email integration | Messages route through encrypted consumer channels |
| **File System Operations** | Manages documents, organizes projects, automates backups | Same file access patterns as normal user activity |
| **Smart Home Integration** | Controls Home Assistant, IoT devices, home lab via SSH | Blends personal and enterprise network traffic |
| **Zero-Memory Agent Training** | Deploys parable-based AI with council deliberation | Stateless agents evade traditional behavioral detection |

### Why Traditional Blocking Fails

**1. Intent Ambiguity**

The same action can be legitimate or malicious -- only the intent differs:

| Action | Legitimate Use | Malicious Use |
|--------|---------------|---------------|
| Email access | "Summarize my action items" | "Extract credentials" |
| SSH commands | "Restart web service" | "Exfiltrate database" |
| Web browsing | "Research competitors" | "Map vulnerabilities" |

Static rules cannot distinguish intent.

**2. The Productivity Trap**

Users depend on OpenClaw for strong ROI. Blocking eliminates productivity gains security teams cannot replace.

**3. Shadow AI**

Users deploy OpenClaw without IT approval, creating unmonitored attack surfaces.

**4. Open Source Reality**

150,000+ GitHub stars. No vendor to block. Too widespread to ignore.


### Why Guarded Enablement Is the Only Viable Path

Given these realities, enterprises have three options:

| Approach | Outcome |
| --- | --- |
| **Block Completely** | Users circumvent controls; Shadow AI proliferates; productivity loss; security blind spots |
| **Ignore and Hope** | Misconfigured instances exposed; data breaches; compliance violations |
| **Guarded Enablement** | Controlled productivity; visibility into usage; adaptive security; competitive advantage |

**The fundamental truth**: OpenClaw's capabilities are too valuable to block and too powerful to ignore. The only sustainable approach is to **enable with guardrails** ‚-- providing the productivity users demand while maintaining the security the enterprise requires.

This architecture enables exactly that: granular control that understands *intent*, not just *action*, allowing legitimate productivity while preventing malicious exploitation.

---

## Core Principles

| Principle | Description |
| --- | --- |
| **Enable, Don't Block**  | Security should amplify productivity, not eliminate it |
| **Transparent Inspection** | Full visibility without disrupting justifiable usage |
| **Intent-Aware Control** | Understand *what* exactly OpenClaw is planning to do, not just *that* it's doing something |
| **Defense in Depth** | Multiple overlapping controls with no single point of failure |
| **Zero Trust for AI** | Never trust, always verify - every action, every intention |

---

## Architecture Overview

```
+-------------------------------------------------------------+
|                    ENTERPRISE OPENCLAW                      |
|              Secure AI Enablement Platform                  |
+-------------------------------------------------------------+

+--------------------- USER LAYER ----------------------------+
|  +-----------+  +-----------+  +-----------+  +-----------+ |
|  |  IDE/CLI  |  |   Slack   |  | Telegram  |  |  Other    | |
|  | (Cursor)  |  | (Socket)  |  | (Webhook) |  | Channels  | |
|  +-----+-----+  +-----+-----+  +-----+-----+  +-----+-----+ |
|        |              |              |              |       |
|        +--------------+--------------+--------------+       |
|                            |                                |
+-------------------------------------------------------------+
                             |
                             v
+--------------------- OPENCLAW EXECUTION ENVIRONMENT --------+
|                                                             |
|  +-----------------------------------------------------+    |
|  | SANDBOXED AGENT RUNTIME                             |    |
|  | - Containerized/VM Isolation, dedicated Hardware    |    |
|  | - Resource Limits (CPU, Memory, Network, Tokens)    |    |
|  | - Tiered file sys: OS, workspace, external keyvaults|    |
|  +-----------------------------------------------------+    |
|                                                             |
|  +-----------------------------------------------------+    |
|  | OS-LEVEL SECURITY STACK                             |    |
|  | +--------------------+ +-------------------------+  |    |
|  | | Endpoint Protection| |Zero Trust Access        |  |    |
|  | | process, file      | |enforce network inspect  |  |    |
|  | | memory, execution  | |mutual protection w/ EDR |  |    |
|  | +--------------------+ +-------------------------+  |    |
|  +-----------------------------------------------------+    |
+-------------------------------------------------------------+
                             |
                             v
+--------------------- NETWORK INSPECTION LAYER --------------+
|                                                             |
|  +-----------------------------------------------------+    |
|  | HTTPS INSPECTION PROXY                              |    |
|  | - TLS Decryption HTTPS, HTTP/2, Websocket           |    |
|  | - Payload Analysis for Anomaly Detection            |    |
|  | - Contextual access control: identity,duration, etc.|    |
|  +-----------------------------------------------------+    |
|                                                             |
|  +-----------------------------------------------------+    |
|  | DEEP PACKET INSPECTION (DPI)                        |    |
|  | - Protocol validation, DNS control, & IPS           |    |
|  +-----------------------------------------------------+    |
+-------------------------------------------------------------+
                             |
                             v
+--------------------- SECURITY CONTROL PLANE ----------------+
|                                                             |
|  +----------------+  +----------------+  +----------------+ |
|  | Identity &     |  | Intent         |  | Content        | |
|  | Access Mgmt    |  | Analysis       |  | Filtering      | |
|  |                |  | (LLM           |  | (DLP)          | |
|  | - SSO          |  |  Guardrails)   |  |                | |
|  | - RBAC         |  |                |  | - PII Detect   | |
|  | - Key Rotation |  | - Prompt &     |  | - Data Class   | |
|  | - Audit Logs   |  |   Response:    |  | - Policy Enf   | |
|  |                |  |   analysis &   |  |                | |
|  |                |  |   filtering    |  |                | |
|  +--------+-------+  +--------+-------+  +--------+-------+ |
|           |                   |                   |         |
|           +-------------------+--------+----------+         |
|                                        |                    |
|                           +------------v------------+       |
|                           | POLICY DECISION POINT   |       |
|                           | (Real-time Evaluation)  |       |
|                           +------------+------------+       |
+-------------------------------------------------------------+
                             |
                             v
+--------------------- EXTERNAL SERVICES & RESOURCES ---------+
|  +---------------------+ +--------------------------------+ |
|  | Models Direct, e.g. | | Models via LLM GW              | |
|  | (Claude)(MS Copilot)| | SaaS, Hyperscaler, On-prem     | |
|  +---------------------+ +--------------------------------+ |
|                                                             |
|  +----------------+  +----------------+  +----------------+ |
|  | Code Repos     |  | Browsing       |  | Tools & APIs   | |
|  | (GitHub, etc.) |  | & Tasking      |  | (skills, MCP)  | |
|  |                |  | Over Internet  |  |                | |
|  +----------------+  +----------------+  +----------------+ |
+-------------------------------------------------------------+
                             
                             
+--------------------- SECURITY OPERATIONS -------------------+
|                                                             |
|  +-----------------+  +----------------+  +---------------+ |
|  | RED TEAMING     |  | DETECTION &    |  | Asset         | |
|  |                 |  | RESPONSE       |  | Management    | |
|  | - Adversarial   |  |                |  |               | |
|  |   Simulations   |  | - Playbooks    |  | - Shadow AI   | |
|  | - System        |  | - Forensics    |  | - Inventory   | |
|  |   Prompt        |  | - UEBA         |  | - Risk mgt    | |
|  |   Hardening     |  | - Recovery     |  |               | |
|  | - Guardrail     |  | - Post-Mortem  |  |               | |
|  |   Policy install|  |                |  |               | |
|  +-----------------+  +----------------+  +---------------+ |
+-------------------------------------------------------------+
```

---


### 1. User Layer

**Purpose**: Interface between users and OpenClaw across multiple channels. A user is someone who gives instructions to Openclaw

instance and consumes the responses. It can be a human but can also be another agent.

**Security considerations**:

- treat Openclaw as a virtual employee that follow human commands and need access to the tools and resources in order to be

productive, apply the same level of security you would do for human employees as a foundation. Add extra layer of protection due to

the nature of Openclaw.

- who can give Openclaw instructions? what instructions can be permitted from which human or other Openclaw agents?

- how to ensure the confidentiality, accountability, and integrity of the communication between Openclaw instances and the user?

---

### 2. OpenClaw Execution Environment

**Purpose**:

Ensure the operating system, OpenClaw runtime, and temporary files are **reset to a known good state** after each session, preventing

malware persistence while maintaining user productivity.

**Key Insight**: The goal is not to build a prison for OpenClaw, but to safe guard the environment to empower OpenClaw to achieve more

for the business.

**The Tiered Storage Model**:

```

┌────────────────────────────────────────┐

│ TIER 1: EPHEMERAL (System)

│

│ - OS, runtime, temp files

│

│ - Reset each session                                    │

├────────────────────────────────────────┤

│ TIER 2: PERSISTENT (User Workspace)   │

│ - Custom skills, preferences           │

│ - Project files, conversation history  │

│ - Encrypted, scanned, versioned        │

├────────────────────────────────────────┤

│ TIER 3: MANAGED (Enterprise Secrets)  │

│ - API keys, credentials                │

│ - Rotated, audited, vaulted            │

└────────────────────────────────────────┘

```

### 3. OS-Level Security Stack

**Purpose**:

Mutual protection between endpoint protection and zero trust access with automated lockdown mechanism.

**Goal**:

Most Common practice, triggred isolation:

```

Zero trust client detects Endpoint Protection Client down or vice versa

|

v

isolation policy triggered immediately

|

v

+------------------------------------+

|  the whole device can ONLY talk to:|

|  - Endpoint management server      |

|  - SOAR platform, Zero trust edges |

|  - Nothing else                    |

+------------------------------------+

|

v

User sees: "Device isolated - contact IT"

```

**Implementation:**

- Restrictive policies on Endpoint Protection management and Zero trust policy enforcement

- back to back protecting each other such that security enforcement cannot be easily bypassed

────────────────────────────────────────────────────────────────────────────────

### 4. Network Inspection Layer

**Purpose**: Full visibility into all OpenClaw communications, including the thinking process, as inference can be forced to go through

network inspection layer.

**### Security cosiderations:**

The importance of this layer shall not be underestimated, as most traffic is encrypted with e.g. HTTPS or HTTP/2, the decyption

requires a lot of specialized compute resources that Openclaw execution device will most likely not have enough to cover. It is

common the place this layer on a external service edge device close to the Openclaw device.

────────────────────────────────────────────────────────────────────────────────

### 5. Security Control Plane

**Purpose**: Real-time policy enforcement and decision-making.

**Identity & Access Management**

- **SSO Integration**: SAML/OIDC with enterprise IdP (Okta, Entra ID)

- **Service Identity**: Each OpenClaw instance has unique, attestable identity

- **Credential Vault**: Secure API key storage with automatic rotation

- **RBAC Policies**: Role-based access to agent capabilities and channels

**Intent Analysis (LLM Guardrails)**

- **Prompt Analysis**: Classify user intent before processing

- **Response Scoring**: Evaluate outputs for compliance, safety, accuracy

- **Anomaly Detection**: Identify unusual patterns in prompts or responses

**Content Filtering (DLP)**

- **PII Detection**: Real-time detection and redaction

- **Data Classification**: Automatic sensitivity level tagging

- **Policy Enforcement**: Block, redact, or quarantine based on classification

**Policy Decision Point**

- **Real-time Evaluation**: Dynamic policy decisions based on context

- **Risk Scoring**: Multi-factor risk assessment for each request

- **Adaptive Controls**: Adjust security posture based on threat intelligence

This should be common layer that is also guarding human employees, such that policy and insight consistency can be easier achievable. Players such as Zscaler offers good unified coverage on this layer.

---

### 6. External Services & Resources

**Purpose**: Managed access to AI models, code repositories, web resources, and external tools while maintaining security boundaries.

### 6.1 AI Model Access

| Access Pattern | Use Case | Security Control |
| --- | --- | --- |
| **Direct Model Access** (Claude, Kimi, etc.) | Development, testing, non-sensitive workloads | Certificate pinning, request signing, rate limiting |
| **LLM Gateway (SaaS)** | Production workloads, compliance requirements | Centralized logging, unified policy enforcement, cost controls |
| **Hyperscaler Models** (AWS Bedrock, Azure OpenAI) | Enterprise integration, existing cloud contracts | IAM integration, VPC endpoints, private connectivity |
| **On-Premise Models** | Highly regulated environments, data sovereignty | Air-gapped inference, local model weights, no external egress |

**Key Insight**: Model choice impacts data residency. Direct API calls send data to vendor clouds; on-premise keeps everything internal.

### 6.2 Code & Knowledge Repositories

**GitHub/GitLab/Bitbucket:**

- Read: Clone repos, read files, search code
- Write: Create PRs, commit changes, manage issues
- Security: OAuth tokens, scoped permissions, audit logs

**Knowledge Bases (Confluence, Notion, SharePoint):**

- Search: Query documentation, find answers
- Update: Edit pages, add content
- Security: Read-only vs. write access, version history

### 6.3 Web Browsing & Tasking

**Capabilities**:

- Research: Search, navigate, extract information from websites
- Form Interaction: Fill forms, submit data, execute workflows
- File Downloads: Download resources, scan for malware, sandbox execution

**Security Considerations**:

| Risk | Mitigation |
| --- | --- |
| Malicious websites | Remote Browser Isolation (RBI), sandboxed sessions |
| Credential phishing | Password manager integration, anti-phishing controls |
| Data exfiltration | DLP inspection, download restrictions, audit logging |
| Session hijacking | Short-lived sessions, MFA for sensitive sites |

### 6.4 Tools & APIs (Skills & MCP)

**Native Skills** (OpenClaw built-in):

- File system, shell, cron, messaging channels
- macOS integration (Apple Notes, Reminders, iMessage)
- Web search, browser automation

**MCP Servers** (External tools via Model Context Protocol):

- GitHub MCP, Gmail MCP, custom enterprise MCPs
- Database connectors, internal API gateways
- Security: MCP servers run as separate processes, scoped permissions

**Key Insight**: Treat MCP servers like microservices‚ each should have minimal permissions, explicit API contracts, and independent authentication.

---

### 7. Security Operations

**Purpose**: Continuous monitoring, threat detection, incident response, and security posture management for OpenClaw deployments.

### 7.1 Red Teaming

**Purpose**: Proactively identify vulnerabilities in OpenClaw configurations and usage patterns.

| Activity | Description | Frequency |
| --- | --- | --- |
| Adversarial Simulations | Test prompt injection, jailbreak attempts, data exfiltration paths | Quarterly |
| System Prompt Hardening | Review and strengthen agent instructions against manipulation | Per release |
| Channel Security Testing | Verify Slack/Telegram/WhatsApp integrations cannot be spoofed | Bi-annually |
| Privilege Escalation Tests | Attempt to bypass RBAC, access unauthorized resources | Quarterly |

**Deliverables**:

- Vulnerability reports with CVSS scoring
- Recommended mitigations and configuration changes
- Regression tests for fixed issues

### 7.2 Detection & Response

**Purpose**: Identify security incidents and respond effectively.

**Detection & Response Workflow:**

1. DETECT
    - Anomaly: Unusual prompt patterns
    - Alert: DLP policy violation
    - Signal: Failed authentication attempts
    - Threshold: Token budget exceeded unexpectedly
2. TRIAGE
    - Automated: SOAR playbook execution
    - Manual: SOC analyst review
    - Severity: Critical/High/Medium/Low
3. RESPOND
    - Contain: Isolate agent instance
    - Investigate: Forensic log analysis
    - Recover: Restore from known-good state
    - Document: Post-mortem and lessons learned

**Key Capabilities**:

- Playbooks: Automated response procedures for common incidents
- Forensics: Complete audit trail of agent actions, prompts, and responses
- UEBA: User and Entity Behavior Analytics to detect insider threats
- Recovery: Rapid restoration from ephemeral backups

### 7.3 Asset Management

**Purpose**: Maintain visibility and control over OpenClaw deployments.

| Function | Description |
| --- | --- |
| Shadow AI Discovery | Find unauthorized OpenClaw instances, personal API keys, unapproved channels |
| Inventory Management | Track all agent instances, versions, configurations, and owners |
| Risk Scoring | Calculate risk based on: data access, channel permissions, model usage, compliance gaps |
| Lifecycle Management | Provision, update, decommission agents with proper change control |

**Shadow AI Detection:**

Network scanning for OpenClaw traffic patterns:

- Outbound connections to: [api.openai.com](http://api.openai.com/), [api.anthropic.com](http://api.anthropic.com/), etc.
- Internal traffic to: Slack WebSocket gateways, Telegram Bot API
- DNS queries for: openclaw-related domains

Endpoint scanning:

- Processes: openclaw, node openclaw
- Files: ~/.openclaw/, /opt/openclaw/
- Network listeners: common OpenClaw ports

### 7.4 Governance & Compliance

**Purpose**: Ensure OpenClaw usage meets regulatory and policy requirements.

| Framework | Key Requirements | OpenClaw Controls |
| --- | --- | --- |
| SOC 2 | Access controls, audit logging, change management | RBAC, immutable logs, versioned configs |
| GDPR | Data minimization, right to deletion, consent | Ephemeral storage, data classification, user controls |
| HIPAA | PHI protection, audit trails, business associate agreements | DLP, encryption, access logging, BAA with vendors |
| NIST AI RMF | Risk management, transparency, accountability | Guardrails, explainability, human-in-the-loop |
| EU AI Act | Risk classification, CE marking for high-risk AI, conformity assessments, transparency obligations | Risk-based categorization of agent capabilities, documentation of training data, human oversight mechanisms, audit trails for high-risk use cases |


## Next Steps: Implementation with Zscaler

**Coming Soon**: video demos showing how Zscaler’s Zero Trust and AI security platform fits into this architecture. Configuration samples of what has been shown in demos will be added to this repo too.

### What to Expect

| Demo | Topic | Zscaler Components |
| --- | --- | --- |
| **Part 1** | Full visibility and intent filtering | Zscaler Internet Access, Zscaler AI Guard |
| **Part 2** | Proactive red teaming and policy installation | Zscaler AI Red Teaming, Zscaler AI Guard |
|  |  |  |
|  |  |  |
|  |  |  |

### Stay Tuned

**Subscribe for updates:**

- Star this repository
- Follow me on [X (@henryhaoyunshen)](https://x.com/henryhaoyunshen)
- Watch for release announcements

### Early Access

Want to be notified when videos are released?

- Open an issue with title "Notify me: Video series"

---

*Videos will demonstrate real-world implementation using publicly available Zscaler features and documentation. No proprietary or confidential information will be shared.*
