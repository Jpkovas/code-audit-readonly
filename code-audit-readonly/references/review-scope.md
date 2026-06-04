# Review Scope Reference

Use this checklist to confirm coverage, not as a substitute for file-by-file reading.

## Correctness And Logic

- Bugs, edge flows, nullability, unsafe conversions, typing gaps.
- Race conditions, async/concurrency issues, inconsistent state transitions.
- Error handling, exception boundaries, retry behavior, and recovery paths.

## Security

- Hardcoded secrets, tokens, keys, passwords, sensitive endpoints, and credential-bearing configs, always reported with redacted evidence.
- Injection: SQL, NoSQL, command, template, expression, prompt, and shell construction.
- XSS, CSRF, SSRF, open redirect, path traversal, unsafe deserialization.
- Upload validation, file type/size checks, archive extraction, and storage permissions.
- Authentication, authorization, bypasses, privilege escalation, tenant isolation.
- Input validation, output encoding, weak crypto, inadequate hashing, insecure randomness.
- Insecure production config, broad CORS, missing headers, debug exposure.
- Sensitive data leakage in logs, errors, telemetry, test fixtures, and artifacts.

## Performance

- Expensive loops, repeated work, excessive I/O, unnecessary allocations.
- N+1 patterns, missing pagination, inefficient data structures.
- Caching opportunities and stale-cache risks.

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
