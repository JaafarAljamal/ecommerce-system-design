# Service Communication

## Two Patterns

### 1. Synchronous (REST)
Used when an immediate response is required.

Used when an immediate response is required.

Order Service ──HTTP──► Payment Service
"Is payment valid?"
◄── "Yes, approved"

**Used for:**
- Payment verification
- User authentication check
- Product availability check

### 2. Asynchronous (Kafka)
Used when a Service needs to notify others without waiting.

Order Service ──► Kafka Topic ──► Notification Service
"order.created"               sends email to customer
──► Shipping Service
prepares shipment

**Used for:**
- Sending email notifications
- Triggering shipment preparation
- Updating inventory after order

## Key Principle
- **REST** → I need an answer NOW
- **Kafka** → I need to inform others, but I won't wait

## Kafka Benefits
- Messages are durable (never lost even if consumer is down)
- Services are decoupled (sender doesn't know who listens)
- Scalable (multiple consumers can read the same topic)

