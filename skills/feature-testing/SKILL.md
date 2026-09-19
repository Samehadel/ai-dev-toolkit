---
name: feature-testing
description: Test features through the UI using business requirements discovered from project context/business_resource.md. Use for business-driven quality control, acceptance testing, and exploratory feature testing; not for unit or integration tests.
---

# Feature Testing

Evaluate whether a feature fulfills its business purpose by exercising the application as its users do. Business requirements define expected behavior; the current implementation does not define correctness.

## Understand the business first

Start by reading `project context/business_resource.md` relative to the target project's root. Treat this as the entry point to the business sources, not necessarily the complete specification. Follow its relevant document, ticket, and other resource references and read the material needed to understand the requested feature. Resolve relative references against the containing document unless the project specifies otherwise.

Identify the feature's purpose, actors and permissions, preconditions, business rules, state transitions, expected outcomes, and acceptance criteria. Note relevant exceptions and dependencies on adjacent workflows. Keep source links or file locations so expected results can be traced to business evidence.

If the entry point is missing, search the project for a moved or equivalently named business resource file. If essential sources remain unavailable, ask for the missing source or access and mark dependent coverage blocked. Do not substitute source code or observed UI behavior for missing business requirements. Continue with scenarios whose expectations are supported; label exploratory observations separately.

Distinguish explicit requirements from assumptions. When sources conflict or leave a material behavior ambiguous, identify the discrepancy and seek clarification instead of inventing an acceptance rule. Code may help locate the feature or diagnose a failure, but it is supporting evidence rather than the business authority.

## Derive useful scenarios

Create a concise scenario set before execution. For each scenario record the business rule/source, actor, preconditions and test data, user actions, and expected observable outcome. Prioritize the main business journey and high-impact failure paths, then select relevant alternatives, boundary values, invalid input, role restrictions, and state transitions. Include persistence, cancellation, repeated actions, or adjacent workflow effects when they matter to this feature; avoid a generic checklist unrelated to its business rules.

Confirm the target environment, application entry point, available roles, and usable test data from project context or the user. Ask only for missing details that affect execution. Use authorized test accounts and data. A testing request does not by itself authorize real purchases, messages to other people, destructive production changes, or other consequential external actions; stop before such an action unless it is already authorized.

## Exercise the actual UI

Use available browser or native UI automation appropriate to the application, following the tool's instructions. Perform feature actions through the UI and inspect visible results. Do not replace UI coverage with direct API requests, database edits, unit tests, or integration tests. Diagnostic tools may explain a failure, but do not establish that the UI scenario passed.

Execute complete user journeys, including the resulting business state and relevant downstream views. A successful click, toast, or absence of console errors alone is insufficient when the requirement concerns a saved record, calculation, permission, or workflow outcome. Reopen or refresh relevant views when persistence is part of the expected behavior.

Record actual results as scenarios run. Capture focused screenshots or other UI evidence for failures and significant outcomes, omitting secrets and unnecessary personal data. Keep evidence tied to the scenario and environment. Distinguish product failures from unavailable accounts, missing data, environment outages, and automation limitations.

Reproduce a suspected defect when safe and useful, using the smallest relevant sequence. Do not repeatedly trigger an action whose completion is uncertain; inspect the state first. Stop retries when they add no evidence or could duplicate consequential effects. Clean up only test data created by this run when cleanup is safe and authorized, and report anything left behind.

Test and report; do not modify application code or business requirements unless the user also requested fixes. If the user requests a test plan only, stop after producing the plan and clearly state that it was not executed.

## Report the evidence

Provide a concise QC report scaled to the feature:

- Business purpose, sources consulted, environment, roles, and scope tested.
- Scenario results with expected versus actual behavior, evidence references, and statuses: **Passed**, **Failed**, **Blocked**, or **Not run**. Only executed and verified scenarios may pass.
- Reproducible defects with preconditions, steps, expected and actual results, evidence, and severity explained by business impact. Keep requirement questions separate from confirmed defects.
- Coverage gaps, unresolved assumptions, remaining test data, and an overall assessment limited to what was actually verified.

Do not claim the feature is fully validated while material business requirements or critical journeys remain untested. Do not file tickets or send reports externally unless requested.
