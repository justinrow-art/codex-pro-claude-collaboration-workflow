# Plan Review with GPT-6 Luna, Pro, Claude Code, and Agent Teams

[繁體中文](README.md)

## Quick links

- [Full workflow](WORKFLOW.en.md) · [Review prompt template](templates/REVIEW-PROMPTS.en.md) · [Session record template](templates/SESSION-RECORD.en.md)
- [完整流程（繁中）](WORKFLOW.md) · [審查提示詞範本](templates/REVIEW-PROMPTS.md) · [任務紀錄表範本](templates/SESSION-RECORD.md)

This workflow positions **Codex running GPT-6 Luna as a lower-cost message coordinator**. Luna receives the user's request, routes it to the side-panel Pro web conversation (model label shown as GPT-6), sends the resulting plan to Claude Code and Agent Teams for cross-checking, and returns their findings to Pro for revision.

Luna's main job is routing messages, enforcing discussion limits, organizing review points, and keeping records. Pro drafts the plan; Claude Code and Agent Teams review it. This design reduces the risk that one model carries the same assumptions or blind spots from planning into its own review. Using a lower-cost model for coordination is a cost-control goal; this document does not claim measured savings.

> Draft: this is process documentation and templates, not a validated API integration or automation product. Side-panel controls, model labels, the models used by Agent Teams, and available call limits must be checked in the actual interface and tools used.

## One complete cycle

1. The user gives the Codex coordinator an objective, constraints, and acceptance criteria.
2. GPT-6 Luna sends the frozen request to the side-panel Pro conversation, where Pro creates or revises the plan.
3. Claude Code cross-checks the plan for flaws, omissions, counterexamples, and items requiring verification.
4. Agent Teams discuss the plan and review findings; record available member perspectives separately from the team conclusion.
5. Luna organizes the issues and sends them back to Pro for point-by-point responses and revisions.
6. Repeat within the preset cycle limit. If the limit is reached with unresolved issues, mark the result pending or blocked; do not claim zero errors.

## Roles

| Role | Responsibility |
| --- | --- |
| User / decision owner | Define the request, scope, acceptance criteria, and whether execution is authorized. |
| Codex (GPT-6 Luna) | Route messages, freeze materials, manage cycle and request counts, and record findings and status. |
| Side-panel Pro (model label GPT-6) | Draft the initial plan and respond to review points one by one. |
| Claude Code | Cross-check the plan and identify risks, omissions, faulty assumptions, and verification needs. |
| Agent Teams | Discuss the plan from multiple agent perspectives; record the visible members, models, and request counts. |

## Boundaries

- Set both a maximum number of complete review cycles and a maximum number of model requests. Count visible Agent Teams calls individually. If the tool does not expose internal calls, document that limit; do not claim full control of the count.
- Different tools do not necessarily mean statistically independent models. Record the model and settings you can observe; do not treat majority agreement as proof.
- A review is incomplete if Pro, Claude Code, or Agent Teams did not receive the full material.
- A bounded discussion proves only that the specified checks were completed, not that a plan is error-free. Preserve disagreements, unknowns, and unmet acceptance criteria.
- Plan review and execution are separate. Execution requires explicit authorization and acceptance evidence.
- Before publication, manually review the content and scan current files plus the complete Git history with a local tool. If a credential may have been exposed, revoke or rotate it first; no scanner finding is not proof of zero risk.

## Documents

- [Full workflow specification](WORKFLOW.en.md) · [繁體中文](WORKFLOW.md)
- [Role prompt templates](templates/REVIEW-PROMPTS.en.md) · [繁體中文](templates/REVIEW-PROMPTS.md)
- [Session record template](templates/SESSION-RECORD.en.md) · [繁體中文](templates/SESSION-RECORD.md)
