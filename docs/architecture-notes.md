# Architecture Notes

## Integration Layer Principles

The integration layer is infrastructure, not a product surface. Its job is to make data and events flow reliably between systems that were built independently, with different data models, different failure semantics, and different operational characteristics. The layer should be as thin as possible and as observable as possible.

Complexity that accumulates in the integration layer — business logic, derived computations, conditional branching based on payload content — is complexity that belongs somewhere else. Push it back to the owning system. The integration layer should translate, route, and deliver; it should not decide.

## Point-to-Point vs. Hub-and-Spoke

Point-to-point integrations are the default starting point and become a maintenance problem at scale. Each new system that needs data from an existing system requires a new integration point — connection management, auth configuration, retry policy, observability. At N systems, you have up to N(N-1)/2 integration paths to maintain.

Hub-and-spoke (event bus, integration broker) reduces integration paths to N connections to the hub and introduces the hub as a shared operational dependency. The tradeoff is appropriate when the number of integration points justifies the operational overhead of the hub and when multiple consumers genuinely need access to the same events.

The decision between the two is almost always a function of growth trajectory and consumer count per event type, not a philosophical preference. Most systems start point-to-point and migrate to hub-and-spoke when the maintenance burden of point-to-point becomes concrete.

## Data Ownership at Boundaries

Every field crossing an integration boundary has an owner. The owning system defines the schema, the semantics, and the valid values. Consuming systems adapt to the owner's model — they do not redefine it on their side and then maintain a translation. When there is genuine semantic disagreement between systems about what a field means, that disagreement must be surfaced and resolved at the design level, not papered over with a transformation rule that no one documents.

## Versioning Approach

All integration contracts are versioned. The versioning scheme and compatibility policy are defined in the contract template. In practice, backward compatibility is maintained for one major version behind current. When a breaking change is necessary, both versions are supported for the migration window defined in the contract's change management section. Silent schema changes are not acceptable — consumers discover them only when they break.

## Observability First

An integration that cannot be observed is an integration that cannot be operated. Logging, metrics, and tracing are configured before the integration goes to production, not after the first production incident. The observability minimum is: request volume, error rate, latency percentiles, and dead-letter queue depth — all queryable by integration ID and tenant ID.
