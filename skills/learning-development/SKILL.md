---
name: learning-development
description: Build working features for learning projects from development requests in the user's prompt. Use for direct implementation without a story, ticket, tracker, refinement, or design approval workflow.
---

# Learning Development

Implement the user's request directly. Treat the prompt and relevant conversation as the requirements; do not require or create a story, ticket, tracker entry, refinement report, or design approval.

Inspect the project, applicable `AGENTS.md` instructions, and nearby code before editing. Follow established patterns where useful. Choose libraries, architecture, names, and implementation details independently. Ask only when essential information or access is missing and cannot reasonably be inferred; continue work that does not depend on the answer.

Deliver the smallest working feature that meets the request. Include backend, UI, database changes, and fictional practice data when relevant. Keep code readable, modular, and easy to extend. Preserve existing authentication and authorization boundaries. Use exact arithmetic for financial values and calculations.

Move quickly. Avoid unnecessary abstractions, infrastructure, documentation, and repeated checks. Give brief progress updates. Continue through implementation and fixes until the requested feature works.

Run the relevant build and focused manual checks of changed behavior. Follow checks required by the repository. Do not add automated tests unless the user requests them or the repository requires them. Fix failures caused by the change and state any actual validation limits.

Finish with a concise report of what works, how to run it, what was validated, and any actual limitations. Commit and push only when explicitly requested.
