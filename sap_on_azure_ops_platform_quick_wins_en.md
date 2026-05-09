# SAP on Azure Operations Platform — Quick Wins to Gain Sponsorship

## Executive Summary

The application is currently in development, but it already has the foundation of a credible enterprise operations platform for SAP on Azure.

The immediate objective should not be to complete the full platform. The immediate objective should be to obtain visibility, trust, and sponsorship through targeted demos that show concrete operational value.

The core message is:

> This platform reduces operational risk, improves compliance visibility, enables controlled self-service, and centralizes SAP on Azure operations data.

The strongest positioning is:

> A controlled self-service operations platform for SAP on Azure, combining operational visibility, compliance checks, conversational diagnostics, and secure automation while keeping Azure RBAC and auditability at the center.

---

## Current Platform Capabilities

The platform has two main pillars.

### 1. Controlled Automation Layer

The application provides several execution paths depending on the type of operation:

- Azure VM status and VM start operations are executed directly through Azure REST APIs using the user’s delegated OBO access token.
- SAP service status is retrieved through a backend-mediated call to the SAP HTTPS status endpoint, after validating the user through the OBO flow.
- More complex SAP, OS, and backup-related operations are executed through Azure Automation, Hybrid Worker, and Ansible.

The architecture separates:

- user authentication,
- authorization in the target subscription,
- technical delegation through managed identities,
- centralized execution,
- credential retrieval through Key Vault,
- audit and traceability.

### 2. MCP-Based Conversational Operations Layer

The frontend also includes a chatbox connected to an MCP service.

The MCP service exposes controlled tools connected to an operational database containing information about SAP systems, Linux hosts, Azure VMs, and Ansible inventory metadata.

The database includes, among others:

#### SAP data

- SAP SID
- SAP kernel version
- HANA version
- HANA client version
- SAP instance numbers
- SAP role or VM label, such as HDB, AAS, ASCS, or PAS
- SAP service status

#### Linux data

- Red Hat release
- Linux kernel version
- pending Red Hat repository updates
- reboot-required status
- uptime

#### Azure data

- subscription ID
- resource group
- VM SKU
- availability zone
- Azure region
- VM power/probe status

#### Ansible inventory data

- SAP SID
- SAP role of the VM
- SAP instance numbers installed on the VM
- inventory host metadata

This turns the platform into more than an automation launcher. It becomes an operational intelligence layer for SAP on Azure.

---

## Guiding Principle for Sponsorship

The goal is to demonstrate value in a way that is immediately understandable to both technical leads and managers.

Avoid leading with implementation details such as React, Flask, FastAPI, OBO, MCP, runbooks, Hybrid Workers, and Ansible.

Lead with business and operational value:

> We can provide a single controlled view of SAP on Azure systems, show compliance and operational exceptions, allow natural-language diagnostics, and enable safe self-service actions using the user’s real Azure permissions.

---

# Recommended Quick Wins

## 1. SAP Systems Health & Compliance Dashboard

This is the strongest first quick win.

Create a dashboard that shows the operational state of SAP-related VMs and systems in one place.

Example columns:

| System | VM | Role | SAP Status | VM Status | Pending Updates | Reboot Required | Uptime | Region |
|---|---|---|---|---|---|---|---|---|
| P01 | p01-aas01 | AAS | Green | Running | Yes | No | 45 days | West Europe |
| Q01 | q01-hdb01 | HDB | Green | Running | No | Yes | 112 days | North Europe |

Useful filters:

- environment: PROD, QA, DEV
- SID
- subscription
- Azure region
- SAP role: HDB, PAS, AAS, ASCS
- reboot required: yes/no
- pending updates: yes/no
- SAP status: green/yellow/red

Why this helps sponsorship:

- It is immediately understandable.
- It shows value without requiring people to understand the underlying architecture.
- It centralizes information that is normally scattered across Azure, Linux, SAP, Ansible, and manual checks.

Positioning phrase:

> We are turning scattered SAP on Azure operational checks into a single controlled health and compliance view.

---

## 2. “Systems Needing Attention” View

Create an exception-based view that highlights only systems requiring operational attention.

Example summary:

```text
Systems needing attention
- 7 VMs require reboot
- 12 VMs have pending Red Hat updates
- 3 SAP systems have one or more stopped services
- 5 HANA hosts have uptime above 90 days
- 2 systems have HANA client versions not aligned with the expected baseline
```

Why this helps sponsorship:

- It converts technical data into operational priorities.
- It helps managers and leads understand risk quickly.
- It supports patching, compliance, and maintenance planning.

Positioning phrase:

> Instead of manually checking every system, we show only what needs attention.

---

## 3. MCP Chatbox Demo with Five Strong Prompts

The chatbox should be demonstrated with a small number of prepared, high-impact prompts.

The message must be clear:

> The chatbot is not a generic AI assistant. It is a controlled natural-language interface over SAP on Azure operational data.

Recommended demo prompts:

```text
Show all PROD SAP VMs that require reboot.
```

```text
Which systems have pending Red Hat updates?
```

```text
Show me the SAP status of SID P01.
```

```text
List HANA hosts with uptime higher than 90 days.
```

```text
Which systems are running in West Europe and have stopped SAP services?
```

Why this helps sponsorship:

- It shows innovation in a concrete and controlled way.
- It demonstrates that AI/chat can be useful without being uncontrolled.
- It makes operational data accessible without requiring users to know SQL, Azure Portal paths, Ansible inventory structure, or SAP host naming conventions.

Important design rule:

> The chatbox should remain read-only at first. It should support diagnostics, reporting, and navigation, while execution should remain in structured workflows.

---

## 4. End-to-End Demo: VM Status and VM Start Using OBO

Create a focused demo showing a simple Azure VM operation executed using the user’s delegated identity.

Scenario:

```text
1. User selects a VM.
2. Frontend sends the user token to the backend.
3. Backend performs the OBO flow.
4. Backend calls the Azure REST API using the user’s delegated access token.
5. Azure RBAC decides whether the user can read VM status or start the VM.
6. The operation is logged with user, target VM, subscription, timestamp, result, and correlation ID.
```

Why this helps sponsorship:

- It demonstrates controlled self-service.
- It avoids a global service principal with excessive privileges.
- It shows that Azure RBAC remains the authorization boundary.
- It creates trust because the operation is both delegated and auditable.

Positioning phrase:

> This is self-service, but not uncontrolled self-service. The user can only perform actions allowed by their real Azure permissions.

---

## 5. SAP Status Through Backend-Mediated Access

Create a demo where the user selects a SID and checks SAP service status.

Flow:

```text
1. User selects a SAP system or SID.
2. Backend validates the user via OBO.
3. Backend resolves the SAP endpoint internally.
4. Backend calls the SAP HTTPS status endpoint.
5. Backend normalizes and filters the response.
6. Frontend displays only the relevant service status.
```

Important security principle:

> The browser should not call the SAP endpoint directly. The backend should mediate access, validate the user, resolve the endpoint internally, normalize the response, and log the request.

Why this helps sponsorship:

- It provides immediate operational value.
- It is low risk and read-only.
- It shows a controlled approach to exposing SAP status information.

---

## 6. Exportable Compliance / Attention Report

Add an export function for CSV or Excel.

Recommended export:

```text
Systems requiring attention
```

Suggested fields:

- SID
- hostname
- SAP role
- environment
- subscription ID
- resource group
- Azure region
- VM SKU
- pending updates
- reboot required
- uptime
- SAP status
- SAP kernel version
- HANA version
- HANA client version
- last collected timestamp

Why this helps sponsorship:

- Managers and technical leads can use the output in meetings.
- It supports compliance reporting.
- It makes the platform useful even before all automation features are complete.

Positioning phrase:

> The platform does not only show data; it creates reusable operational evidence.

---

## 7. Freshness Indicator for Operational Data

Every displayed value should show when it was collected and from which source.

Example:

```text
Reboot required: Yes
Last collected: 2026-05-09 08:15 UTC
Source: Ansible fact collection
Freshness: Fresh
```

Possible freshness states:

- fresh
- stale
- unknown
- real-time
- last snapshot

Why this helps sponsorship:

- It increases trust in the data.
- It avoids confusion between real-time checks and historical snapshots.
- It gives the platform a more enterprise-grade feel.

Positioning phrase:

> Operational data is only useful when users know how fresh it is.

---

# Recommended Demo Structure

The most effective sponsorship demo should be short, focused, and value-driven.

Recommended duration: around 10 to 12 minutes.

## Part 1 — SAP on Azure Operations Dashboard

Show a central dashboard with:

- number of SAP systems
- systems with pending Red Hat updates
- systems requiring reboot
- SAP systems not fully green
- VMs with high uptime
- kernel or HANA version deviations
- Azure region and subscription distribution

Message:

> We currently have operational data scattered across Azure, SAP, Linux and Ansible. This gives us one controlled view.

## Part 2 — Systems Needing Attention

Show the exception-based view.

Message:

> The platform helps us focus on systems that actually require operational attention.

## Part 3 — MCP Chatbox

Run two or three prepared prompts, for example:

```text
Show PROD systems requiring reboot.
```

```text
Show SAP status for SID P01.
```

```text
Which systems have pending Red Hat updates?
```

Message:

> The chat uses controlled MCP tools over our operational database. It is read-only, tool-driven, and based on structured data.

## Part 4 — Safe Self-Service Action

Demo a low-risk action such as:

- probe VM status,
- read VM power state,
- start a DEV VM.

Message:

> The action uses the user’s delegated Azure token. Azure RBAC remains the authorization boundary.

## Part 5 — Audit View

Show a simple audit record:

- user
- operation
- target VM
- SID
- subscription
- timestamp
- result
- correlation ID

Message:

> This is self-service with control, traceability, and accountability.

---

# What Not to Demo First

Avoid making BCM switch the first sponsorship demo.

A BCM switch workflow is valuable, but it is also critical and may create concern if shown too early.

Better positioning:

```text
Phase 1: visibility and safe read-only operations
Phase 2: controlled simple actions
Phase 3: guided workflows for operational procedures
Phase 4: critical workflows such as BCM switch
```

Message:

> We are starting with low-risk, high-value use cases. The same architecture can later support critical guided workflows.

---

# Roadmap Framing

## Phase 1 — Visibility and Diagnostics

- SAP systems health dashboard
- systems needing attention
- MCP chatbox for read-only diagnostics
- SAP status check
- VM status/probe
- exportable compliance reports

## Phase 2 — Controlled Self-Service

- VM start for allowed users
- simple operational actions using OBO and Azure RBAC
- improved audit and correlation ID
- role-based visibility in the application

## Phase 3 — Guided Operational Workflows

- restart workflows
- backup checks
- controlled Ansible-based operations
- pre-check and post-check screens
- approval gates where needed

## Phase 4 — Critical Procedures

- BCM switch workflow
- state-machine-driven execution
- lock management
- four-eyes approval
- explicit point-of-no-return handling
- full audit report

---

# Key Architectural Message

The architecture is strong because it separates insight from execution.

```text
Chatbox / MCP
  -> read-only diagnostics, reporting, correlation, navigation

Structured UI / Workflows
  -> controlled execution, approvals, state transitions, audit

Backend
  -> authentication, authorization, policy, validation, logging

Azure Automation / Ansible
  -> technical execution

Operational DB
  -> SAP, Linux, Azure, and inventory context
```

The safest and most persuasive principle is:

> Chat for understanding. Workflow for action.

---

# Sponsorship Pitch

Do not pitch this as a technical implementation.

Avoid:

> I built a React frontend with Flask/FastAPI, OBO, MCP, Azure Automation, Hybrid Worker, and Ansible.

Use instead:

> I can show a single controlled view of our SAP on Azure landscape, including patching status, reboot requirements, SAP service status, versions, VM metadata, and operational exceptions. The same platform also supports read-only conversational diagnostics and first safe self-service actions based on the user’s real Azure permissions.

Short version:

> This is a controlled self-service and operations intelligence platform for SAP on Azure.

---

# Priority List

| Priority | Quick Win | Sponsorship Value |
|---:|---|---|
| 1 | SAP health and compliance dashboard | Easy to understand, immediately useful |
| 2 | Systems needing attention view | Converts data into priorities |
| 3 | MCP chatbox with prepared prompts | Shows practical AI value |
| 4 | VM status/start via OBO | Demonstrates secure self-service |
| 5 | SAP status via backend | Low-risk operational value |
| 6 | Exportable report | Useful for meetings and compliance |
| 7 | Freshness indicators | Builds trust in the data |
| 8 | Audit/correlation ID view | Makes the platform enterprise-ready |

---

# Final Assessment

The application should be positioned as a developing SAP on Azure operations platform, not as a collection of scripts or automations.

The strongest early value is visibility, compliance, diagnostic speed, and controlled self-service.

The best sponsorship strategy is to demonstrate a small number of low-risk, high-value use cases that are easy to understand:

1. Show what needs attention.
2. Ask questions through the MCP chatbox.
3. Execute a safe action through delegated user permissions.
4. Show the audit trail.

This creates a credible path from development prototype to sponsored internal platform.
