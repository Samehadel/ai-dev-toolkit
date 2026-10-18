# Security, AI data flows, logging, and resources

## Trace boundaries before judging

For relevant changes, map input → identity/tenant → authorization → transformation → storage/logging → outbound destination. Include background jobs, caches, errors, telemetry, and streaming responses. Identify attacker-controlled fields, sensitive data, and the enforcement point for each boundary. Inspect shared middleware/callers before assuming validation or authorization is missing.

## Credentials and AI provider keys

- Trace keys from configuration or user entry through storage, transport, SDK initialization, refresh, and deletion. Check server-only handling for service-owned keys and intentional custody/access controls for user-supplied keys. Distinguish public identifiers from credentials.
- Inspect frontend bundles and public environment variable prefixes, static exports, source maps, mobile/desktop assets, Docker layers/build arguments, CI artifacts, fixtures, notebooks, and tracked configuration. Client-distributed service keys are extractable even if obfuscated. An environment variable is not inherently server-only.
- Inspect request/response serialization, debug endpoints, exceptions, SDK debug modes, HTTP interceptors, tracing exporters, analytics, and crash reports for key/header exposure. Verify nested redaction and error paths, not only successful responses.
- Check least privilege, credential lookup ownership, tenant scoping, cache keys, and shared mutable SDK clients for cross-user key reuse. Check configurable provider/proxy endpoints, redirects and base URLs: credentials must not be forwarded to attacker-selected hosts. Consider SSRF and access to metadata/private network endpoints.
- Never print candidate secrets or transmit them to a provider to test validity. Report only location and credential type with a redacted description. For credible exposure, recommend revocation/rotation and artifact/history cleanup; removing the current line alone does not invalidate a leaked key. Do not rotate keys or alter history without authorization.

## Data leakage and privacy

- Check object-level and field-level authorization on reads, writes, exports, downloads, subscriptions, and batch APIs. Verify tenant constraints also exist in asynchronous jobs, caches, object storage URLs, vector stores and search filters.
- Trace PII, credentials, proprietary documents, chat history, uploaded files, and embeddings into third-party APIs, queues, analytics and persistence. Verify the minimum required fields leave the boundary and that destination/access/retention behavior matches the application's requirements.
- Inspect response DTOs, overfetching, exception details, public caching, shared conversation state, temporary files, attachment permissions, and deletion propagation. Encryption alone does not establish authorization or appropriate data use.
- For prompt injection, treat retrieved documents, web pages, tool output, and model output as untrusted data. Verify tool authorization and argument validation outside the model, least-privileged execution, and checks before consequential actions. Do not assume a system prompt prevents exfiltration or privilege escalation.
- Verify retrieval authorization before content enters model context; output filtering cannot undo disclosure to a provider. Check cross-tenant conversation/vector identifiers, prompt caches, thread reuse, and provider-hosted file/session lifecycles.
- Inspect model responses before rendering HTML/Markdown or passing them to SQL, shell commands, URLs, or tool executors. Check token/output bounds, agent iteration limits, tool budgets, and server-enforced usage quotas against abuse and runaway spend.
- Do not assert provider training, residency, or retention guarantees from memory. Use the actual configured product/account settings and current official documentation if such a claim is necessary; otherwise identify the verification gap.

## Sensitive logging and telemetry

- Follow request bodies, headers, cookies, URLs/query strings, provider SDK errors, prompt/completion text, tool arguments/results, retrieved context and attachments into logs, traces, breadcrumbs, audit trails and crash reporting.
- Check allowlisted structured fields or effective redaction at every sink, including nested objects, streaming errors, retries, debug modes and third-party exporters. Verify redaction occurs before serialization/export and does not retain an unredacted copy elsewhere.
- Prefer request IDs, timings, token counts and safe error codes where payloads are unnecessary. Verify audit records remain useful without credentials or unrestricted content. Check log access, retention and log-injection risks when relevant to the change.

## Application security

- Evaluate authentication/session/token verification, expiration and revocation; server-side authorization; privilege escalation and mass assignment; SQL/command/template injection, XSS, CSRF, SSRF, path traversal, unsafe deserialization and file upload boundaries as applicable.
- Review CORS/cookie configuration, TLS verification, webhook signatures and replay protection, rate limits and error disclosures where touched. Validate dependencies/advisories against the actual version and reachable use; do not label every new dependency vulnerable.

## Memory, lifetime, and bounded work

- Trace allocation versus input size and concurrent requests. Include uploads, decompression, parsing, serialization, base64 expansion, prompt construction, tokenization, embeddings, vector batches, buffered streams, and repeated copies. Streaming interfaces may still buffer internally.
- Look for unbounded caches/maps, queues, event listeners, subscriptions, accumulated chat history, abandoned tasks, retained closures, and unreleased file/socket/DB handles. Verify eviction, ownership, cleanup on errors/disconnects, and bounded retention.
- Check request/body/output limits, pagination and batch sizes, maximum concurrency, backpressure, timeouts and cancellation propagation. Account for retries multiplying in-flight work and slow consumers retaining buffers.
- Estimate worst-case memory from evidence when useful: per-request retained bytes × concurrency + persistent caches/queues. State assumptions and distinguish measured peaks from estimates. Avoid inventing universal safe limits.
- Review transactions/locks held during external model calls, event-loop blocking CPU work, N+1 queries, quadratic processing, and large result materialization. A missing bound merits a finding only with a credible growth path and consequence.

## Focused verification

Use available fixtures or synthetic data to check the highest-risk boundaries: cross-tenant access, nested error redaction, provider endpoint credential forwarding, oversized payload rejection, and cancellation/cleanup. Do not send real data or credentials to external services. Run only checks permitted by the environment; distinguish static analysis, executed tests, and suggested follow-up. A scanner's absence of results is not proof that no secrets or leaks exist.
