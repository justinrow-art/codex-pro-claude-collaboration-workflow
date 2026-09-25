# Workflow Specification: Plan Review Coordinated by Luna

[繁體中文](WORKFLOW.md)

## Purpose and design rationale

Use GPT-6 Luna as the Codex coordinator for routing requests, round-trip communication, counters, and records. The side-panel Pro session (model label shown as GPT-6) proposes the plan. Claude Code and Agent Teams challenge and check it, then Pro responds to each point.

When the same model plans and reviews, it may carry the same assumptions forward or miss the same class of issue. This workflow adds other review roles to surface different views. This is a design to reduce shared-blind-spot risk; it does not guarantee reviewer independence or a correct result. Using Luna as coordinator is intended to control coordination cost; actual savings would need to be measured separately.

## 1. Roles and responsibilities

| Role | Responsibility |
| --- | --- |
| User / decision owner | State the request, constraints, success criteria, and whether execution is authorized. |
| Codex coordinator (GPT-6 Luna) | Route the request and plan, maintain the canonical version, enforce cycle limits, record reviews, and track open issues. |
| Side-panel Pro (model label GPT-6) | Create the initial plan, address review comments point by point, and provide revisions. |
| Claude Code | Cross-check outside the Pro conversation; identify omissions, risks, counterexamples, and claims needing verification. |
| Agent Teams | Let multiple agents discuss the plan and review findings; record visible agent outputs, the team conclusion, and model information. |

A different tool does not necessarily mean a statistically independent model. If Agent Teams member models or internal request counts are not visible, mark them UNKNOWN or record only the aggregate result.

## 2. Freeze the task and materials

Record the objective, deliverable, allowed and prohibited actions, success criteria, execution authorization scope, maximum complete cycles, and maximum model requests.

Prepare a read-only evidence packet and record its version and SHA-256. Log exactly what each role received and whether anything was truncated or unavailable. A material change requires a new version and hash. Do not describe unread files as reviewed.

## 3. Tool and model preflight

Before each call, record the role, tool, model name shown by the interface, visible reasoning setting, input version, prompt version, and request number. Keep only necessary sign-in status; never record passwords, tokens, cookies, or authorization headers.

Record the Pro side-panel label, the Luna model selected for coordination, and visible Claude Code or Agent Teams model settings from the current session. If not visible, use UNKNOWN rather than inferring a backend from a label. A completed tool status only means the call ended; read back its content.

## 4. Counters

Set two limits before starting:

- **Complete review cycles**: one full Pro plan/revision → Claude Code cross-check → Agent Teams discussion → Pro point-by-point response.
- **Model requests**: every model request actually sent. Retries count as new requests; count each visible Agent Teams agent call separately.
- **Hidden team-call limits**: if Agent Teams does not expose individual requests or cannot enforce the cap, record the visible aggregate and the limitation. Do not claim exact control over hidden calls.
- **Complete result**: every required stage returns substantive, readable content based on the correct material version. An interruption, empty response, or missing role makes the cycle INCOMPLETE or BLOCKED.

Do not conflate cycles with requests. Stop adding calls at the limit and preserve unresolved issues.

## 5. Steps in one review cycle

1. **Pro creates or revises the plan**: list steps, rationale, assumptions, risks, unknowns, and acceptance method.
2. **Claude Code cross-checks**: use the same frozen material to find faulty assumptions, omissions, counterexamples, insufficient grounds, and failure conditions.
3. **Agent Teams discussion**: preserve identifiable individual views before summarizing the team conclusion; distinguish support, objections, additions, and items that could not be judged.
4. **Luna organizes and relays findings to Pro**: de-duplicate and number the comments, linking each to the original claim and material version. Luna routes and organizes; it is not another authoritative reviewer.
5. **Pro responds point by point**: mark each issue accepted, partially accepted, rejected, or unknown; explain why and submit a revised plan.
6. **Cycle closure check**: confirm that all four stages completed, request limits were respected, and acceptance criteria can be checked. Otherwise mark INCOMPLETE or BLOCKED.

If issues remain and the preset limit allows, start another cycle from the latest frozen plan and open-issue list. Record a new version when material changes. If the limit is reached with issues outstanding, report them; do not treat silence, agreement, or majority opinion as proof that errors were eliminated.

## 6. Plan outcome and execution gate

Luna records the final version, review outcomes, reasons for accepted or rejected comments, unresolved points, acceptance status, and evidence locations. Use these outcomes precisely:

- **READY_FOR_DECISION**: specified checks passed; the user decides whether to execute.
- **NEEDS_REVISION**: issues remain that can be corrected.
- **BLOCKED**: material, tools, authorization, or important grounds are missing.
- **UNRESOLVED**: the limit was reached with disagreements or unknowns remaining.

Passing checks does not mean zero errors. Execution requires explicit authorization that covers the action. Preserve the baseline first; afterward, mark each item PASS, FAIL, BLOCKED, or NOT_ATTEMPTED and attach readable acceptance evidence.

## 7. Security review before publication

Manually review text, file lists, attachments, links, and Git metadata for private conversations, account details, personal paths, or unauthorized material. Use a trusted local secret scanner on current files and the complete Git history; review every finding. No findings is not proof of zero risk.

If a scanner actively verifies credentials, check its network destination and request behavior first. Do not run that mode without authorization or upload private material to an external scanning service. If a potentially exposed active credential is found, revoke or rotate it before cleaning history and scanning again. Confirm repository visibility and the exact content to be pushed before publishing.

## 8. States

| State | Meaning |
| --- | --- |
| DRAFT | Task scope is not frozen. |
| PREPARED | Request, packet, limits, and acceptance criteria are recorded. |
| PREFLIGHT_PASSED | Tools, model labels, sign-in state, and packet availability are checked. |
| REVIEWING | Review is underway within the preset limits. |
| INCOMPLETE | A required stage did not return a complete, readable response. |
| NEEDS_REVISION | Review found issues requiring changes. |
| READY_FOR_DECISION | Specified checks are complete; the user decides next steps. |
| EXECUTION_AUTHORIZED | Existing explicit authorization covers the execution scope. |
| EXECUTING | Execution has started but acceptance is not complete. |
| VERIFIED | Evidence shows all required acceptance criteria passed. |
| PARTIAL | Some work is complete, but required items remain incomplete or unverified. |
| BLOCKED / UNRESOLVED | A prerequisite is missing, or unknowns/disagreements remain at the limit. |
| CANCELLED | The decision owner cancelled further work. |
