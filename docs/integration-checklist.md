# Integration Checklist

Pre-production checklist for new integration points. Each category represents a required area of review before go-live. Items not completed require documented rationale for deferral.

---

## Contract and Schema

- [ ] Integration boundary has a formal contract
- [ ] Contract is versioned and stored in version control
- [ ] Payload schemas are registered in the schema registry
- [ ] Breaking vs. non-breaking change policy is agreed between teams
- [ ] Consumer has pinned to a specific schema version

## Authentication and Authorization

- [ ] Auth mechanism is defined in the connector config
- [ ] Credentials are stored in the secrets manager
- [ ] Token/credential rotation schedule is documented
- [ ] Inbound request authentication is enforced
- [ ] Tenant isolation is enforced at the integration boundary

## Reliability

- [ ] Retry policy is defined per error class
- [ ] Dead-letter path is configured and monitored
- [ ] Circuit breaker is configured for the external dependency
- [ ] Timeout is set explicitly on all outbound calls
- [ ] Idempotency is implemented for all write operations

## Observability

- [ ] Every request logs source, destination, correlation ID, status, and latency
- [ ] Integration errors surface in the alerting system
- [ ] Metrics collected for request volume, error rate, and p99 latency
- [ ] Distributed traces include the integration boundary span
- [ ] Dead-letter queue depth is alarmed

## Data Handling

- [ ] Data classification is documented for all fields crossing the boundary
- [ ] PII handling meets applicable retention requirements
- [ ] Payload encryption is configured if required by data classification

## Failure Modes

- [ ] Behavior on external system unavailability is documented and tested
- [ ] Behavior on authentication failure is documented and tested
- [ ] Behavior on schema validation failure is documented and tested

## Operational Readiness

- [ ] Runbook exists covering health verification, failure investigation, and isolation procedure
- [ ] On-call team has been briefed on the new integration dependency
- [ ] Rollback plan is documented
