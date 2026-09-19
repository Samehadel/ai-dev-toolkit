---
name: integration-testing
description: Plan, implement, run, and diagnose integration tests across application components, APIs, databases, queues, and service adapters. Use to verify behavior across real component boundaries; not for isolated unit tests or UI acceptance testing.
---

# Integration Testing

Verify that connected components honor their contracts and produce the expected observable state. Match the work to the request: a plan does not imply execution, and a test run does not imply permission to modify application code.

## Establish the boundary and expected behavior

Read project instructions, relevant contracts and requirements, existing integration tests, test configuration, and CI commands. Use the project's existing runner, fixtures, and dependency setup unless they cannot exercise the requested boundary. If `project context/business_resource.md` exists, follow relevant sources when business rules determine expected results.

Identify the entry point, components exercised, dependencies, and observable outcomes. Derive expectations from documented contracts and requirements; use implementation to locate boundaries and diagnose failures, not as the sole authority for correctness. Label inferred expectations and clarify ambiguities that materially affect assertions while continuing supported coverage.

State which dependencies will be real and which will be substituted. Keep the boundary under test real: mocking a repository does not verify database integration, and stubbing an HTTP provider verifies only the local adapter against the stub's assumptions. Use provider sandboxes or contract verification when provider compatibility is in scope; report when those checks are unavailable.

## Select meaningful scenarios

Choose cases based on the requested change and its likely integration failures. Record each scenario's source or contract, setup, stimulus, expected response and side effects, and dependency requirements. Start with the main flow and select relevant cases such as:

- Serialization, validation, authentication, or authorization at the actual boundary.
- Persistence and transaction behavior, including rollback and absence of partial writes on failure.
- Dependency errors, timeouts, retries, or duplicate delivery where the system promises recovery or idempotency.
- Asynchronous outcomes, event payloads, ordering, or eventual consistency where required by the contract.

Avoid multiplying cases already covered by unit tests unless crossing the boundary changes the risk. For a regression, choose assertions that expose the reported failure rather than merely exercising the changed lines.

## Prepare an isolated environment

Resolve the target environment and effective connection configuration before executing tests that write data. Prefer existing local or disposable test infrastructure. Do not silently fall back to a shared or production target when a test dependency is unavailable. Reuse established secret handling without copying credentials into tests or reports.

Use unique run identifiers, isolated databases or schemas, and scoped fixtures as appropriate. Apply the project's real migrations when database schema compatibility is under test. Transaction rollback is sufficient only when all writers participate in that transaction; separate processes and background workers may require explicit cleanup.

Create only the data needed by each scenario and avoid order dependence. Clean up resources owned by the run, including after failures; do not clear shared tables or queues indiscriminately. Use test accounts and sandbox destinations for external effects. If required execution exceeds existing authorization, finish the safe preparation and identify the specific action that needs approval.

## Implement and execute

When test creation is requested, add tests alongside the existing integration suite. Exercise public component entry points through the real wiring, protocol, or persistence layer relevant to the boundary. Assert contract-level outputs and durable effects, including the absence of unintended effects on failure. A successful status code alone is insufficient when the contract requires a saved record or emitted event.

For asynchronous behavior, wait on an observable condition with a bounded deadline and useful timeout diagnostics. Avoid fixed sleeps and blanket retries that hide failures. Account for test concurrency, clocks, generated identifiers, and background processing when they affect determinism.

Run the smallest relevant test selection first, then the affected integration suite and required project checks. Record the exact commands and meaningful environment details. Distinguish assertion failures from setup failures, missing dependencies, and unavailable credentials; skipped or unexecuted cases do not pass.

Investigate failures using scoped logs, responses, and persisted state without exposing secrets. Reproduce only when it adds evidence and is safe; inspect uncertain external outcomes before retrying. Do not rerun until green or weaken assertions to match a defect. Fix application code only when requested or already within the authorized task.

## Report the result

Summarize the boundary tested, real versus substituted dependencies, commands, and results. Give scenario-level expected versus actual behavior for failures, with reproducible setup and relevant evidence. Separate product defects from environment blockers and uncertain contract expectations.

List coverage gaps, skipped checks, and any resources left behind. Explain what the tests establish and what remains unverified, especially compatibility with substituted external services. For planning-only work, explicitly state that tests were not executed.
