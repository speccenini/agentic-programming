# MCP Chatbox Features: Implementation Checklist

## Purpose

This document defines the next quick-win features for the MCP-powered chatbox in the SAP on Azure Operations Cockpit.

The goal is **not** to build a generic AI chatbot. The goal is to provide a **controlled, read-only, tool-driven natural-language interface** over SAP on Azure operational data.

Positioning:

> A controlled natural-language interface over SAP on Azure operational data.

The chatbox should help users query, diagnose, correlate, and navigate operational information across SAP, Linux, Azure, and Ansible inventory data. Risky or invasive actions must remain in structured UI workflows, not directly executed from free-text chat.

---

## Guiding Principles

The MCP chatbox should be:

- **Read-only by default**
- **Tool-driven**, not hallucination-driven
- **Context-aware**, using the current frontend selection
- **Freshness-aware**, always showing when data was collected or checked
- **Evidence-based**, showing data source and relevant facts
- **Operations-oriented**, focused on actionable system insight
- **Safe**, never executing critical operations directly from free text
- **Navigational**, able to guide users to the correct cockpit view or workflow

---

## Core Demo Questions the Chatbox Must Answer

These are the questions that should work reliably for a sponsor/demo scenario.

### 1. SAP System Overview

The chatbox must answer questions about a selected SAP system or application group.

Example prompts:

```text
Show me the status of SAP system SLM.
Which VMs belong to SLM in DEV?
Show the architecture of SLM.
Which host is the HANA DB for SLM?
Which host is the PAS for SLM?
```

Expected response content:

- Application / SAP system name
- SID
- Environment
- Hostnames
- SAP roles: PAS, AAS, ASCS, HDB, JVA, etc.
- Instance numbers
- SAP status
- VM probe / power status if available
- Integration readiness
- Last checked / last collected timestamp

Example response shape:

```text
Summary:
SLM DEV consists of 3 VMs and is currently operationally healthy.

Hosts:
- xdab226642s0 | PAS | instance 00/01 | SAP status green
- xdab226642k0 | HDB | HANA host | SAP status green
- xdab226642j0 | JVA | SAP status green

Data freshness:
- Inventory collected at: <timestamp>
- SAP status checked at: <timestamp>
```

---

### 2. Linux Patching and Compliance

The chatbox must support questions about Red Hat patching, pending updates, reboot requirements, and uptime.

Example prompts:

```text
Which SAP VMs have pending Red Hat updates?
Show PROD SAP VMs requiring reboot.
Which systems are not patch-compliant?
Show VMs with reboot required grouped by SAP SID.
Which SAP VMs have uptime higher than 90 days?
```

Expected response content:

- SID / application
- Hostname
- SAP role
- Environment
- Red Hat release
- Linux kernel
- Pending updates count
- Reboot required: yes/no
- Uptime
- Last collected timestamp

Recommended grouping:

```text
Group by SID/application, then list affected VMs.
```

Example response shape:

```text
Summary:
I found 5 SAP VMs requiring reboot.

Affected systems:
1. SLM DEV
   - xdab226642s0 | PAS | reboot required: yes | pending updates: 3 | uptime: 92 days

2. PIP PRD
   - <hostname> | HDB | reboot required: yes | pending updates: 8 | uptime: 140 days

Data freshness:
Inventory collected at: <timestamp>
```

---

### 3. SAP and HANA Versions

The chatbox must answer questions about SAP kernel, HANA version, and HANA client version.

Example prompts:

```text
Which systems are running SAP kernel version 753.1400?
Show SAP kernel versions grouped by SID.
Show HANA version for all HDB hosts.
Which HANA hosts have HANA client version different from the HANA DB version?
Which systems have outdated SAP kernel versions?
```

Important behavior:

If no approved baseline/target version exists, the chatbot must not classify versions as outdated. It should say that the current versions can be listed, but no baseline is configured.

Expected response for missing baseline:

```text
I can list the current SAP kernel versions, but no approved target baseline is configured yet. Therefore I cannot classify them as outdated.
```

Expected response content:

- SID / application
- Hostname
- SAP role
- SAP kernel version
- HANA DB version
- HANA client version
- Mismatch indicator where relevant
- Last collected timestamp

---

### 4. Azure Metadata

The chatbox must correlate SAP systems and VMs with Azure metadata.

Example prompts:

```text
Where is SLM running?
Show SAP VMs by Azure region.
Which SAP VMs are in northeurope?
Which systems run on Standard_D4s_v3?
Show all SAP VMs in subscription <subscription-id>.
Which SAP systems have components in availability zone 1?
```

Expected response content:

- SID / application
- Hostname
- SAP role
- Subscription ID
- Resource group
- Azure region
- Availability zone
- VM SKU
- Environment

---

### 5. Systems Needing Attention

This is the most important sponsor/demo capability.

Example prompts:

```text
Which SAP systems need attention?
Show me all non-green systems.
Which systems have SAP status not fully green?
Which systems have both pending updates and reboot required?
Which PROD systems require operational follow-up?
```

The chatbox should aggregate multiple operational signals, for example:

- SAP status not green
- VM probe not successful
- Pending Red Hat updates
- Reboot required
- Uptime above threshold
- Integration not ready
- Version mismatch
- Missing or stale data

Expected response shape:

```text
Summary:
I found 4 SAP systems needing attention.

1. SLM DEV
   Severity: Warning
   Findings:
   - 1 VM requires reboot
   - 3 pending Red Hat updates
   - SAP status: green

2. PIP PRD
   Severity: Critical
   Findings:
   - SAP status not fully green on PAS
   - VM probe successful
   - Last status check: <timestamp>

Recommended next step:
Open the system details or the relevant controlled workflow.
```

Recommended severity levels:

- `OK`
- `Warning`
- `Critical`
- `Unknown`

---

### 6. Action Readiness and Available Safe Actions

The chatbox should not execute risky actions, but it should explain which actions are available and how they are controlled.

Example prompts:

```text
What actions are available for this VM?
Can I start this VM from the cockpit?
Is this system ready for automation?
Why is this VM not integration ready?
What safe actions are available for SLM?
```

Expected response content:

- Available actions
- Execution path
- Identity model
- Authorization model
- Impact level
- Whether approval is required
- Whether the action is read-only or mutating

Example response shape:

```text
Available safe actions for SLM DEV:

1. Probe VM status
   - Type: read-only
   - Execution path: Azure REST API
   - Identity: user delegated token through OBO
   - Authorization: Azure RBAC

2. SAP status check
   - Type: read-only
   - Execution path: backend-mediated SAP HTTPS status endpoint
   - Authorization: authenticated user context + application policy

3. Start VM
   - Type: mutating
   - Execution path: Azure REST API
   - Identity: user delegated token through OBO
   - Authorization: Azure RBAC
```

---

## Suggested Demo Prompt Set

Prepare these prompts as clickable suggested prompts in the chatbox.

```text
Show me the SAP status of SLM.
Which VMs belong to SLM DEV and what SAP roles do they have?
Which SAP VMs require reboot?
Show SAP VMs with pending Red Hat updates grouped by SID.
Which SAP systems need attention?
Show HANA hosts with their HANA version and HANA client version.
Which systems are running in northeurope and in which availability zone?
What safe actions are available for SLM?
```

---

## Frontend Features to Implement

### 1. Suggested Prompts

When the chatbox is empty, display clickable suggested prompts:

```text
Show systems needing attention
Show VMs requiring reboot
Show pending Red Hat updates
Show SAP status for selected system
Show architecture for selected system
Show available safe actions
```

Acceptance criteria:

- Suggested prompts are visible before the first user message.
- Clicking a prompt submits it to the chatbox.
- Prompts can use the current frontend context if available.

---

### 2. Frontend Context Awareness

The frontend should pass the currently selected context to the backend/MCP service.

Example context payload:

```json
{
  "selected_application": "SLM",
  "selected_environment": "DEV",
  "selected_sid": "SM1",
  "selected_hostname": "xdab226642s0",
  "selected_view": "Application Group View"
}
```

Expected behavior:

If the user asks:

```text
Show me the SAP status.
```

and `SLM DEV` is selected, the chatbox should understand the target as `SLM DEV` without requiring the user to repeat it.

Acceptance criteria:

- Context is included with every chat request.
- The backend can resolve ambiguous questions using current context.
- The response states which context was used.

Example:

```text
Using current context: SLM DEV.
```

---

### 3. Links / Buttons from Chat Response to UI

When the chatbox identifies systems, VMs, or workflows, it should provide UI actions where possible.

Examples:

```text
Open SLM DEV
Open VM details
Open SAP status panel
Open controlled reboot workflow
Open action panel
```

Acceptance criteria:

- Chat responses can include structured navigation actions.
- Frontend renders actions as buttons or links.
- Clicking an action navigates to the corresponding cockpit view or opens the relevant modal.

---

## MCP Tooling to Implement

Prefer small, deterministic tools instead of one generic query tool.

### Minimal Tool Set

Implement or verify the following MCP tools:

```text
get_sap_system_overview(sid_or_application, environment)
get_sap_instances(sid, environment)
get_vm_details(hostname)
get_sap_status(sid, environment)
list_vms_requiring_reboot(environment?)
list_vms_with_pending_updates(environment?)
list_systems_needing_attention(environment?)
list_versions_by_system(environment?)
list_vms_by_azure_location(region?, zone?)
get_available_actions(target_type, target_id)
```

### Tool Design Requirements

Each tool should:

- Return structured JSON
- Include source information
- Include `last_collected_at` or `last_checked_at`
- Include environment where applicable
- Apply authorization filtering where applicable
- Avoid returning unnecessary raw internal details
- Avoid accepting arbitrary URLs from the frontend

Example response shape:

```json
{
  "status": "success",
  "data": [],
  "metadata": {
    "source": "inventory_db",
    "last_collected_at": "2026-05-09T14:20:00Z",
    "freshness": "fresh"
  }
}
```

---

## Response Format Standard

The chatbox should use a consistent response structure.

Recommended format:

```text
Summary
Key findings
Details
Recommended next step
Data freshness
```

For short answers, the response can be compact, but it should still include data freshness when operational data is used.

Example:

```text
Summary:
SLM DEV is operationally healthy.

Key findings:
- 3 VMs found
- SAP roles: PAS, HDB, JVA
- SAP status: green
- Reboot required: no
- Pending updates: 0

Recommended next step:
No immediate action required.

Data freshness:
- Inventory: collected at 14:20
- SAP status: checked at 14:32
```

---

## Freshness Requirements

Every answer based on operational data must include freshness information.

Required fields:

- `last_collected_at` for inventory/facts
- `last_checked_at` for live/on-demand checks
- `source`
- `freshness`: `fresh`, `stale`, `unknown`

Suggested freshness logic:

```text
fresh   = collected within accepted threshold
stale   = older than accepted threshold
unknown = timestamp missing or unreliable
```

The response should explicitly warn when data is stale.

Example:

```text
Warning:
The inventory data for this system is stale. Last collected at: <timestamp>.
```

---

## Safe Failure Behavior

The chatbox must avoid inventing information.

### Missing Data

If data is missing:

```text
I do not have current data for this field.
```

### Missing Baseline

If a question requires a target baseline that is not configured:

```text
I can list the current values, but no approved baseline is configured yet. Therefore I cannot classify them as compliant or outdated.
```

### Ambiguous Target

If the target is ambiguous and no frontend context resolves it:

```text
I found multiple matching systems. Please select one: SLM DEV, SLM PRP, SLM PRD.
```

### Unsupported Action

If the user asks the chatbox to execute a risky action:

```text
I cannot execute this action directly from chat. I can open the controlled workflow where authorization, pre-checks, and audit are enforced.
```

---

## Authorization and Safety Requirements

Even if the chatbox is read-only, it must not expose all data to all users.

Required approach:

- Validate the user identity before serving chat responses.
- Apply application-level authorization filters.
- Respect Azure-related access boundaries where applicable.
- Do not expose systems, subscriptions, or hostnames outside the user's allowed scope.
- Do not allow arbitrary URL calls through MCP tools.
- Do not expose raw SAP endpoint output if it contains unnecessary internal details.

Recommended split:

```text
Chatbox MCP:
- query
- diagnose
- explain
- correlate
- suggest next step
- open workflow

Structured UI workflows:
- approve
- execute
- audit
- retry
- rollback/stop where applicable
```

---

## Priority Implementation Plan

### Priority 1: Systems Needing Attention

Implement:

```text
list_systems_needing_attention(environment?)
```

Signals to include:

- SAP status not green
- VM probe failed or unknown
- Pending Red Hat updates
- Reboot required
- Uptime above threshold
- Integration not ready
- Stale or missing inventory

This is the strongest demo capability.

---

### Priority 2: Reboot Required and Pending Updates

Implement:

```text
list_vms_requiring_reboot(environment?)
list_vms_with_pending_updates(environment?)
```

This directly supports security compliance and operational planning.

---

### Priority 3: SAP System Overview

Implement:

```text
get_sap_system_overview(sid_or_application, environment)
get_sap_instances(sid, environment)
```

This proves that the chatbox understands SAP systems as application groups, not isolated VMs.

---

### Priority 4: SAP Status

Implement:

```text
get_sap_status(sid, environment)
```

The tool should call the existing backend-mediated SAP HTTPS status mechanism, not expose the SAP endpoint directly to the browser.

---

### Priority 5: Available Safe Actions

Implement:

```text
get_available_actions(target_type, target_id)
```

The response should explain:

- Action name
- Read-only vs mutating
- Execution path
- Identity used
- Authorization model
- Whether approval is required
- Whether it opens a controlled workflow

---

## Definition of Done

A feature is complete when:

- The prompt works from the chatbox.
- The MCP service calls deterministic tools, not generic guessing logic.
- The response is structured and concise.
- Data freshness is shown.
- Missing data is handled explicitly.
- Ambiguous targets are handled safely.
- Authorization filtering is applied.
- The response can provide navigation actions back into the cockpit UI.
- No mutating or risky operation is executed directly from chat.

---

## Golden Demo Scenario

Use this sequence for sponsor demos:

1. Select an SAP application group in the cockpit, for example `SLM DEV`.
2. Open the chatbox.
3. Click suggested prompt: `Show SAP status for selected system`.
4. Ask: `Which VMs belong to this system and what SAP roles do they have?`
5. Ask: `Which SAP VMs require reboot?`
6. Ask: `Which SAP systems need attention?`
7. Ask: `What safe actions are available for this system?`
8. Use the returned UI action to open the corresponding system or VM details.

Key message:

> The chatbox is a controlled operations assistant. It queries real SAP on Azure operational data through MCP tools, provides freshness and evidence, and guides the user toward safe, structured workflows instead of executing risky actions directly.
