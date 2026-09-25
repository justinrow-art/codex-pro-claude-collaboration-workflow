# Plan Review Prompt Templates

[繁體中文](REVIEW-PROMPTS.md)

Record the prompt version and input hash for every request. All review roles should receive the same frozen material. Clearly mark omissions or truncation.

## A. Side-panel Pro: create or revise the plan

You are responsible for proposing or revising the plan. Base your response only on material you actually read, distinguishing source-supported facts, inferences, and unknowns. Do not execute the plan, modify files, or contact anyone.

**Request and objective:** [Content]

**Allowed scope / exclusions:** [Content]

**Acceptance criteria:** [Content]

**Frozen material and version:** [Paste it or identify its exact version and SHA-256]

List steps, rationale, supporting grounds, assumptions, risks, unknowns, acceptance checks, and the claims most in need of external review.

## B. Claude Code: cross-check

Review the plan adversarially. Do not assume it is correct because another model proposed it; do not reject supported points merely to find faults. Review only material you actually read; do not execute programs, modify files, or call external services.

**Request and frozen material version:** [Same material version and hash]

**Pro plan:** [Full text and version]

State what you actually received and any material gaps. For each point, list support / contradiction / insufficient evidence, specific grounds, counterexamples, failure conditions, and necessary verification. If the material is incomplete or mismatched, report that directly; do not fill gaps yourself.

## C. Agent Teams: multi-agent discussion

Discuss the plan to find possible errors, omissions, and unknowns. Preserve each identifiable agent's view before writing the team summary. Do not execute the plan or modify the materials.

**Frozen material version:** [Same material version and hash]

**Pro plan:** [Full text and version]

**Claude Code review:** [Full text; mark any truncation]

For each point, identify the agent role / visible model, comment, grounds, counterexample, disagreement, and claims requiring external verification. If the tool does not expose member models or internal request counts, mark them UNKNOWN rather than guessing.

## D. Pro: reconcile cross-review findings

Respond point by point to the Claude Code and Agent Teams findings. For each issue, mark accepted, partially accepted, rejected, or unknown; explain the grounds, plan changes, and remaining uncertainty.

**Original plan version:** [Version and hash]

**Claude Code findings:** [Full text]

**Agent Teams findings:** [Individual comments and team summary]

End with the revised plan, unresolved issues, acceptance criteria still unmet, and a recommended status. Do not claim zero errors, execution, or verification.
