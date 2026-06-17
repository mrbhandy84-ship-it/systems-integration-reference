# API Contract Template

Use this template when defining a new integration boundary between two systems communicating over HTTP/REST or a similar request-response protocol.

---

## Contract Header

| Field | Value |
|---|---|
| Contract ID | `{{CONTRACT_ID}}` |
| Version | `{{SEMVER}}` |
| Provider | `{{PROVIDER_SYSTEM_NAME}}` |
| Consumer | `{{CONSUMER_SYSTEM_NAME}}` |
| Protocol | `{{HTTP / gRPC / GraphQL}}` |
| Base URL | `{{BASE_URL}}` |
| Auth Mechanism | `{{bearer / api_key / oauth2 / mtls}}` |
| Owner Team | `{{OWNING_TEAM}}` |
| Status | `{{draft / active / deprecated / retired}}` |
| Effective Date | `{{ISO_DATE}}` |

---

## Endpoints

For each endpoint included in this contract:

### `{{METHOD}} {{/path/to/resource}}`

**Description**: `{{What this endpoint does and when consumers should call it}}`

**Request**
- Headers: `{{Required and optional headers}}`
- Path parameters: `{{Parameter name, type, constraints}}`
- Query parameters: `{{Parameter name, type, required/optional}}`
- Body: Reference to `{{PAYLOAD_SCHEMA_ID}}` version `{{VERSION}}`

**Response**
- Success: `{{2xx status}}` — Reference to `{{RESPONSE_SCHEMA_ID}}`
- Client error: `{{4xx status}}` — `{{Conditions that produce this status}}`
- Server error: `{{5xx status}}` — `{{Conditions that produce this status}}`

**Idempotency**: `{{Yes / No — key field if yes}}`

**Rate limits**: `{{Requests per window, burst limit, 429 behavior}}`

---

## Error Contract

Standard error response shape for all endpoints in this contract:

```json
{
  "error_code": "{{NAMESPACED_ERROR_CODE}}",
  "message": "{{Human-readable description}}",
  "request_id": "{{Correlation ID for tracing}}",
  "details": {}
}
```

Error codes are namespaced by domain (`auth.`, `validation.`, `resource.`, `upstream.`). Consumers should handle error codes explicitly, not just HTTP status codes.

---

## SLA

| Metric | Target |
|---|---|
| p50 latency | `{{ms}}` |
| p99 latency | `{{ms}}` |
| Availability | `{{%}}` |
| Data freshness | `{{N/A or lag tolerance}}` |

---

## Change Management

Breaking changes require consumer notification with a minimum of `{{N}}` days lead time. Non-breaking changes (additive fields, new optional parameters) are deployed without advance notice. Deprecated fields are retained for `{{N}}` days after deprecation notice.

Versioning strategy: `{{URL versioning / header versioning / none}}`. Previous major version supported until `{{sunset policy}}`.

---

## Contract Validation

Consumer tests against this contract are maintained in `{{CONSUMER_REPO/path/to/tests}}`. Provider must not ship changes that break pinned consumer test suites without coordinating version migration.
