# API Contract Template

Use this template to define integration boundaries between systems communicating over request-response protocols. Populate all fields before review. Placeholder values use `{{PLACEHOLDER}}` notation.

---

## Contract Header

| Field | Value |
|---|---|
| Contract ID | `{{CONTRACT_ID}}` |
| Version | `{{SEMVER}}` |
| Provider | `{{PROVIDER_SYSTEM_NAME}}` |
| Consumer | `{{CONSUMER_SYSTEM_NAME}}` |
| Protocol | `{{PROTOCOL}}` |
| Base URL | `{{BASE_URL}}` |
| Auth Mechanism | `{{AUTH_MECHANISM}}` |
| Owner Team | `{{OWNING_TEAM}}` |
| Status | `{{STATUS}}` |
| Effective Date | `{{ISO_DATE}}` |

---

## Endpoints

### `{{METHOD}} {{/path/to/resource}}`

**Request**
- Headers: `{{HEADERS}}`
- Path parameters: `{{PATH_PARAMS}}`
- Query parameters: `{{QUERY_PARAMS}}`
- Body: Reference to `{{PAYLOAD_SCHEMA_ID}}` version `{{VERSION}}`

**Response**
- Success: `{{2xx status}}`
- Client error: `{{4xx status}}`
- Server error: `{{5xx status}}`

**Idempotency**: `{{YES_NO}}`

**Rate limits**: `{{RATE_LIMIT_SPEC}}`

---

## Error Contract

```json
{
  "error_code": "{{NAMESPACED_ERROR_CODE}}",
  "message": "{{MESSAGE}}",
  "request_id": "{{CORRELATION_ID}}",
  "details": {}
}
```

---

## SLA

| Metric | Target |
|---|---|
| p50 latency | `{{MS}}` |
| p99 latency | `{{MS}}` |
| Availability | `{{PCT}}` |
| Data freshness | `{{FRESHNESS}}` |

---

## Change Management

Breaking change notice period: `{{N}}` days. Non-breaking changes deployed without advance notice. Deprecated fields retained for `{{N}}` days.

Versioning strategy: `{{VERSIONING_STRATEGY}}`. Previous major version supported until `{{SUNSET_POLICY}}`.

---

## Contract Validation

Consumer tests maintained in `{{CONSUMER_REPO/path/to/tests}}`.
