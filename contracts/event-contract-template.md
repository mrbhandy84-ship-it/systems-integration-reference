# Event Contract Template

Use this template to define event-based integration boundaries between a publisher and one or more consumers. Populate all fields before review. Placeholder values use `{{PLACEHOLDER}}` notation.

---

## Contract Header

| Field | Value |
|---|---|
| Contract ID | `{{CONTRACT_ID}}` |
| Version | `{{SEMVER}}` |
| Publisher | `{{PUBLISHING_SYSTEM_NAME}}` |
| Consumers | `{{CONSUMING_SYSTEM_NAMES}}` |
| Transport | `{{TRANSPORT}}` |
| Topic / Channel | `{{TOPIC_NAME}}` |
| Encoding | `{{ENCODING}}` |
| Schema Registry | `{{SCHEMA_REGISTRY_URL}}` |
| Owner Team | `{{OWNING_TEAM}}` |
| Status | `{{STATUS}}` |

---

## Event Types

### `{{domain.entity.action}}`

**Payload schema**: Reference to `{{SCHEMA_ID}}` version `{{VERSION}}`

**Ordering guarantees**: `{{ORDERING_SPEC}}`

**Deduplication**: `{{DEDUP_SPEC}}`

**Retention**: `{{RETENTION_PERIOD}}`

**Expected volume**: `{{VOLUME_ESTIMATE}}`

---

## Payload Envelope

```json
{
  "event_id": "{{uuid}}",
  "event_type": "{{domain.entity.action}}",
  "source": "{{publisher_system_id}}",
  "timestamp": "{{ISO 8601 UTC}}",
  "version": "{{payload_schema_version}}",
  "correlation_id": "{{optional}}",
  "tenant_id": "{{optional}}",
  "payload": {}
}
```

---

## Consumer Obligations

- Handle unknown fields gracefully
- Implement idempotent processing for all event types
- Maintain consumer group offset within the retention window

---

## Compatibility Policy

| Change Type | Classification | Notice Required |
|---|---|---|
| Add optional field | Non-breaking | None |
| Remove or rename field | Breaking | `{{N}}` days |
| Change field type | Breaking | `{{N}}` days |
| Add new event type | Non-breaking | None |
| Remove event type | Breaking | `{{N}}` days |

---

## Operational Notes

Dead-letter handling: `{{DLQ_SPEC}}`

Backpressure: `{{BACKPRESSURE_SPEC}}`

Replay policy: `{{REPLAY_SPEC}}`
