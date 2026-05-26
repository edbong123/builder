---
name: Diagnose AGENTS.md
description: Analyzes the current AGENTS.md for missing fields, vague entries, contradictions, and manual tasks that require human action. Run once at the start of every session before writing any code.
---

# Skill: Diagnose AGENTS.md

## Purpose

Analyze the current AGENTS.md for missing fields, vague entries, contradictions, and manual tasks that need human action. Run once at the start of every session before writing any code.

---

## Instructions for the agent

When this skill is invoked, do the following in order.

---

### Step 1 - Health check

Check that all skill files are present in /skills/:

- diagnose-agents-md.md
- define-agents-md.md
- define-user.md
- define-stack.md
- define-scope.md
- define-features.md

If any are missing, flag them by name and note they need to be restored before the session continues.

---

### Step 2 - Required fields check

Verify each of the following fields exists in AGENTS.md and is not empty, vague, or a placeholder (e.g. "TBD", "to be defined", "nothing for now"):

- Target user
- Goal tier (must be one of: Demo only, Demo + continue, Prototype only, Prototype + continue, Production MVP, Production open-ended)
- Platform (must be: Mobile first or Desktop first)
- Tech stack
- Feature list
- Out of scope
- Third-party services

For each field that fails: flag it, label it MISSING or VAGUE, and note which skill fixes it.

| Field | Status | Fix with |
|---|---|---|
| Target user | MISSING / VAGUE / OK | define-user.md |
| Goal tier | MISSING / VAGUE / OK | define-agents-md.md |
| Platform | MISSING / VAGUE / OK | define-agents-md.md |
| Tech stack | MISSING / VAGUE / OK | define-stack.md |
| Feature list | MISSING / VAGUE / OK | define-features.md |
| Out of scope | MISSING / VAGUE / OK | define-scope.md |
| Third-party services | MISSING / VAGUE / OK | define-agents-md.md |

---

### Step 3 - Quality checks

Run these checks on fields that are present.

**Feature list**
- Does it have a priority order? If not: flag as INCOMPLETE.
- Does it have dependencies mapped between features? If not: flag as INCOMPLETE.

**Target user**
- Is it specific enough to inform UI and flow decisions? If it reads like "anyone who wants to use the app" or similar: flag as VAGUE.

**Goal tier vs feature list**
- If goal tier is Demo only but the feature list includes auth, payments, real user data, or production infrastructure: flag as CONTRADICTION.

**Data type vs security**
- If data type is Real private (PII) or Real compliant and security requirements are empty or not addressed: flag as CONTRADICTION.

**Platform vs visual style**
- If platform is Mobile first and visual style is a heavy theme system: flag as CONTRADICTION.

**Database vs provisioning**
- If database is New DB and there is no provisioning note or manual task flagged: flag as INCOMPLETE.

**MCP config vs frontend tool**
- If MCP config references tools that are not compatible with the selected frontend tool: flag as CONTRADICTION.

---

### Step 4 - Manual task flags

Check for any of the following conditions in AGENTS.md. For each that applies, flag it as a manual task the user must complete outside the agent.

| Condition | Manual task required |
|---|---|
| Database = New DB | Create instance, set connection string, seed schema |
| Database = Existing DB | Add connection config, verify access |
| Goal tier = Production MVP or Production open-ended | HTTPS setup, secrets manager, auth provider config, logging, pre-publish security checklist |
| Data type = Real private (PII) | Encryption setup, consent flows, data retention policy |
| Data type = Real compliant | All PII tasks + regulatory checklist (HIPAA, PCI, or relevant) |
| Feature requires OAuth | OAuth provider setup and key registration |
| Feature requires payments | Payment provider account, API keys, webhook config |
| Feature requires email sending | Email provider account and API key |
| Feature requires file storage | Storage bucket creation and access config |
| Feature requires custom domain / DNS | DNS setup outside the agent |
| Frontend tool requires account or API key | Account creation and key registration before build |

---

### Step 5 - Output

Present a diagnostic report in this format:

---

**AGENTS.md Diagnostic Report**

**Skill files** - [All present / Missing: list]

**Required fields**
[Table from Step 2]

**Quality issues**
[List each flag with: field name, issue type (INCOMPLETE / CONTRADICTION), description]

**Manual tasks flagged**
[List each task with: condition that triggered it, what the user needs to do]

**Overall status**
- PASS - All required fields present, no contradictions. Ready to build.
- NEEDS ATTENTION - [X] issues found. Recommend fixing before building.
- BLOCKED - Critical fields missing. Do not proceed until resolved.

---

Then ask the user:

> How do you want to proceed?
> A - Fix issues now (runs define-agents-md.md)
> B - Skip for this session
> C - Ignore always (disables diagnostic for this project)

---

## Acceptance criteria

The diagnostic is complete when:

- All skill files confirmed present or flagged
- All required fields checked and statused
- All quality checks run
- All manual tasks surfaced
- Report presented to user
- User has chosen A, B, or C
  
