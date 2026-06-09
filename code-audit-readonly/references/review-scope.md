# Review Scope Reference

Use this checklist to confirm coverage, not as a substitute for file-by-file reading.

## Real-World Use Filter

For every suspected issue, first classify the file or code path as production runtime, admin/runtime tooling, CI/release, migration, test-only, example, generated, or local-only. Report a finding only when the problem can affect a normal user, authorized operator, CI/release pipeline, deployed service, supported integration, or documented maintenance workflow.

Do not report a candidate only because a pattern is generally risky. Check caller contracts, input validation, framework protections, feature flags, environment checks, build-time transforms, and deployment exposure. If normal use cannot reach the behavior, keep it out of the numbered findings.

## Correctness And Logic

- Bugs, edge flows, nullability, unsafe conversions, typing gaps.
- Race conditions, async/concurrency issues, inconsistent state transitions.
- Error handling, exception boundaries, retry behavior, and recovery paths.
- Report edge cases only when a normal user, integration, migration, or operator workflow can realistically trigger them.

## Security

- Hardcoded secrets, tokens, keys, passwords, sensitive endpoints, and credential-bearing configs, always reported with redacted evidence.
- Injection: SQL, NoSQL, command, template, expression, prompt, and shell construction.
- XSS, CSRF, SSRF, open redirect, path traversal, unsafe deserialization.
- Upload validation, file type/size checks, archive extraction, and storage permissions.
- Authentication, authorization, bypasses, privilege escalation, tenant isolation.
- Input validation, output encoding, weak crypto, inadequate hashing, insecure randomness.
- Insecure production config, broad CORS, missing headers, debug exposure.
- Sensitive data leakage in logs, errors, telemetry, test fixtures, and artifacts.
- Avoid security findings that require an attacker to already control the host, edit trusted config, bypass authentication elsewhere, or invoke private/internal functions that are not exposed by the product.

## Performance

- Expensive loops, repeated work, excessive I/O, unnecessary allocations.
- N+1 patterns, missing pagination, inefficient data structures.
- Caching opportunities and stale-cache risks.
- Report performance findings only with a plausible data size, frequency, endpoint/job, or workload that normal use can create.

## Maintainability And Architecture

- Literal and logical duplication.
- Long functions, mixed responsibilities, hidden coupling, confusing APIs.
- Outdated comments, naming ambiguity, unclear ownership boundaries.
- Cross-module contracts that are implicit, untested, or fragile.

## Observability And Reliability

- Log quality and actionability without secret exposure.
- Metrics/tracing gaps for critical flows.
- Error message consistency, alertability, retry/idempotency, and failure isolation.

## Tests And Dependencies

- Missing tests for critical logic, security boundaries, error paths, and integration workflows.
- Brittle tests, weak assertions, fixture drift, or skipped coverage.
- Vulnerable, deprecated, pinned, or overly permissive dependencies.
- CI/release/package configuration that can ship stale or untested code.
