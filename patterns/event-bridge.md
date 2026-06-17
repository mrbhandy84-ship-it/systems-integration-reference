# Event Bridge

This pattern decouples event producers from consumers through an intermediary that handles routing, transformation, and delivery. The bridge owns fan-out logic and dead-letter handling. Delivery guarantees are determined by the underlying transport configuration.
