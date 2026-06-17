# Event Contract Template

Use this template when defining an event-based integration boundary where one system publishes events and one or more systems consume them.

---

## Contract Header

| Field | Value |
|---|---|
| Contract ID | `{{CONTRACT_ID}}` |
| Version | `{{SEMVER}}` |
| Publisher | `{{PUBLISHING_SYSTEM_NAME}}` |
| Consumers | `{{CONSUMING_SYSTEM_NAMES — comma separated}}` |
| Transport | `{{Kafka / EventBridge / Pub/Sub / SQS / custom}}` |
| Topic / Channel | `{{TOPIC_NAME}}` |
| Encoding | `{{JSON / Avro / Protobuf}}` |
| Schema Registry | `{{SCHEMA_REGISTRY_URL or "none"}}` |
| Owner Team | `{{OWNING_TEAM}}` |
| Status | `{{draft / active / deprecated / retired}}` |

---

## Event Types

For each event type published under this contract:

### `{{domain.entity.action}}`

**Description**: `{{What state change or occurrence this event represents}}`

**Trigger condition**: `{{When the publisher emits this event}}`

**Payload schema**: Reference to `{{SCHEMA_ID}}` version `{{VERSION}}`

**Ordering guarantees**: `{{Strict per-key / best-effort / none}}`

**Deduplication**: `{{Publisher guarantees / Consumer responsibility / idempotency key field}}`

**Retention**: `{{Duration events are retained on the topic}}`

**Expected volume**: `{{Events per second / minute / day under normal load}}`

---

## Payload Envelope

All events published under this contract use the following envelope:

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

Event-type-specific data is contained in `payload`. The envelope structure is governed by the canonical event schema; only `payload` varies per event type.

---

## Consumer Obligations

Consumers of this contract must:

- Handle unknown fields gracefully (forward compatibility)
- Implement idempotent processing for all event types
- Maintain a consumer group offset that allows replay within the retention window
- Not assume event ordering across partitions unless ordering guarantees are explicitly stated above

---

## Compatibility Policy

Event schema changes follow these rules:

| Change Type | Classification | Notice Required |
|---|---|---|
| Add optional field to payload | Non-breaking | None |
| Remove or rename existing field | Breaking | `{{N}}` days minimum |
| Change field type | Breaking | `{{N}}` days minimum |
| Add new event type | Non-breaking | None |
| Remove event type | Breaking | `{{N}}` days minimum |

Consumers pin to schema versions via the schema registry. Publisher must maintain support for the previous major schema version for `{{N}}` days after a breaking change is deployed.

---

## Operational Notes

Dead-letter handling: `{{What consumers should do with events that cannot be processed}}`

Backpressure: `{{Publisher behavior when consumers fall behind}}`

Replay policy: `{{Whether and how consumers can request replay}}`
