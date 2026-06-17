# Adapter Pattern

The adapter pattern isolates the integration logic for a specific external system behind a stable internal interface. Internal consumers call the adapter's interface; the adapter handles translation between the internal data model and the external system's API, data format, authentication mechanism, and error taxonomy.

The operational value is straightforward: when an external system changes its API, the change is absorbed by the adapter. Internal consumers are unaffected as long as the adapter's interface contract remains stable. Without adapters, API changes propagate across the codebase to every call site.

In practice, adapters become significant maintenance surfaces in their own right. They accumulate special cases as external APIs evolve: deprecated endpoint fallbacks, response format variations across API versions, undocumented error codes that require specific handling. Adapters that grow without discipline become harder to reason about than direct API calls. The discipline required is strict interface stability — the adapter's interface must not leak external system details, and the adapter's implementation must not accept internal dependencies. Leakage in either direction defeats the isolation purpose.

Authentication and session management belong in the adapter, not in the connector configuration layer. The adapter is responsible for obtaining, caching, and refreshing credentials for its target system. Connectors provide the transport configuration; adapters handle the protocol.

Testing adapters against real external APIs is the only reliable way to catch breaking changes before they affect production. Contract testing with recorded HTTP interactions covers regression well but misses behavioral changes in the external system that the recorded responses don't reflect. A minimal live smoke test suite that runs on a schedule is worth maintaining for critical external dependencies.
