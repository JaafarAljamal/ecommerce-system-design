# Payment Service — Database Schema

```mermaid
erDiagram
    PAYMENTS {
        uuid id PK
        uuid order_id FK
        uuid customer_id FK
        enum type
        enum status
        decimal amount
        string currency
        string payment_method_id
        string gateway_transaction_id
        string failure_reason
        timestamp created_at
        timestamp updated_at
    }

    REFUNDS {
        uuid id PK
        uuid payment_id FK
        uuid order_id FK
        decimal amount
        string reason
        enum status
        string gateway_refund_id
        timestamp created_at
    }

PAYMENTS ||--o{ REFUNDS : "has"
```

## Payment Type Values
| Type | Description |
|------|-------------|
| `payment` | Customer payment |
| `refund` | Refund to customer |

## Payment Status Values
| Status | Description |
|--------|-------------|
| `pending` | Awaiting gateway response |
| `success` | Payment completed |
| `failed` | Payment declined |
| `refunded` | Amount returned to customer |

## Notes
- `payment_method_id` is Stripe token — card data never stored here (PCI DSS)
- `gateway_transaction_id` is the reference from Stripe/PayPal
-`failure_reason` populated only when status = failed

