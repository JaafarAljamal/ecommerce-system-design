# Order Service — Database Schema

```mermaid
erDiagram
    ORDERS {
        uuid id PK
        uuid customer_id FK
        uuid address_id FK
        enum status
        decimal subtotal
        decimal tax
        decimal shipping_price
        decimal total
        string tracking_number
        timestamp created_at
        timestamp updated_at
    }

    ORDER_ITEMS {
        uuid id PK
        uuid order_id FK
        uuid product_id FK
        uuid variant_id FK
        uuid seller_id FK
        int quantity
        decimal unit_price
        decimal discount_applied
        decimal subtotal
    }

    ORDER_STATUS_HISTORY {
        uuid id PK
        uuid order_id FK
        enum status
        string note
        timestamp created_at
    }

    RETURNS {
        uuid id PK
        uuid order_id FK
        uuid order_item_id FK
        string reason
        enum status
        timestamp created_at
    }

    ORDERS ||--o{ ORDER_ITEMS : "contains"
    ORDERS ||--o{ ORDER_STATUS_HISTORY : "tracks"
    ORDERS ||--o{ RETURNS : "has"
    ORDER_ITEMS ||--o| RETURNS : "returned via"
```

## Order Status Values
| Status | Description |
|--------|-------------|
| `pending` | Awaiting payment confirmation |
| `payment_failed` | Payment was declined |
| `confirmed` | Payment successful, seller notified |
| `processing` | Seller is preparing the order |
| `shipped` | Handed to shipping provider |
| `in_transit` | On the way to customer |
| `delivered` | Successfully received |
| `return_requested` | Customer requested a return |
| `returned` | Return completed |
| `cancelled` | Order cancelled |

## Notes
- `ORDER_STATUS_HISTORY` tracks every status change with timestamp
- `unit_price` in ORDER_ITEMS uses Snapshot Pattern
- `tracking_number` populated when status = shipped

