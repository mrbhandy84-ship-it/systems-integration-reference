# Integration Checklist

High-level checklist for evaluating readiness before a new integration goes to production. This is not a step-by-step guide — each item represents a category of work that must be completed and validated. The details are in the relevant contract documents, runbooks, and architecture notes.

---

## Contract and Schema

- [ ] Integration boundary has a formal contract (API or event contract from templates)
- [ ] Contract is versioned and stored in version control
- [ ] Payload schemas are registered in the schema registry (or documented equivalent)
- [ ] Breaking vs. non-breaking change policy is agreed between provider and consumer teams
- [ ] Consumer has pinned to a specific schema version, not "latest"

## Authentication and Authorization

- [ ] Auth mechanism is defined in the connector config — not hardcoded in application code
- [ ] Credentials are stored in the secrets manager, not in environment files or config files
- [ ] Token/credential rotation schedule is documented and automated where possible
- [ ] Inbound request authentication is enforced — unauthenticated requests are rejected
- [ ] Tenant isolation is enforced at the integration boundary if multi-tenant

## Reliability

- [ ] Retry policy is defined and documented per error class (transient vs. permanent)
- [ ] Dead-letter path is configured and monitored
- [ ] Circuit breaker is configured for the external dependency
- [ ] Timeout is set explicitly — no calls with default or infinite timeouts
- [ ] Idempotency is implemented for all write operations

## Observability

- [ ] Every request logs: source, destination, correlation ID, status, and latency
- [ ] Integration errors are surfaced in the alerting system with appropriate severity
- [ ] Metrics are being collected for: request volume, error rate, and p99 latency
- [ ] Distributed traces include the integration boundary in the trace span
- [ ] Dead-letter queue depth is alarmed

## Data Handling

- [ ] Data classification is documented for all fields crossing the boundary
- [ ] PII handling meets applicable data residency and retention requirements
- [ ] Payload encryption is configured if required by data classification
- [ ] Field mapping and transformation logic is tested against known edge cases

## Failure Modes

- [ ] Behavior on external system unavailability is documented and tested
- [ ] Behavior on authentication failure is documented and tested
- [ ] Behavior on schema validation failure is documented and tested
- [ ] Backpressure or queue saturation scenario is documented

## Operational Readiness

- [ ] Runbook exists covering: how to verify the integration is healthy, how to investigate a failure, and how to disable or isolate the integration if needed
- [ ] On-call team has been briefed on the new integration dependency
- [ ] Load test or capacity estimate exists for expected production volume
- [ ] Rollback plan is documented — what happens if the integration is disabled post-launch

---

Items not yet checked are not blockers automatically — they require a deliberate decision to defer with documented rationale, not silent omission.
