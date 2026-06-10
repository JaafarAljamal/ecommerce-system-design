# User Service — Database Schema

```mermaid
erDiagram
    USERS {
        uuid id PK
        string first_name
        string last_name
        string email
        string phone
        string password_hash
        enum role
        string profile_image
        timestamp created_at
        timestamp updated_at
    }

    SELLERS {
        uuid id PK
        uuid user_id FK
        string middle_name
        string id_image
        string bank_name
        string account_number
        string iban
        string business_name
        float rating
        enum status
    }

    CUSTOMERS {
        uuid id PK
        uuid user_id FK
        enum gender
        string payment_method_id
    }

    ADDRESSES {
        uuid id PK
        uuid customer_id FK
        string street
        string city
        string country
        string zip_code
        boolean is_default
    }

    SUPPORT_AGENTS {
        uuid id PK
        uuid user_id FK
        uuid active_ticket_id FK
    }

    USERS ||--o| SELLERS : "has"
    USERS ||--o| CUSTOMERS : "has"
    USERS ||--o| SUPPORT_AGENTS : "has"
    CUSTOMERS ||--o{ ADDRESSES : "has"
```

## Seller Status Values

|      Status        |      Description                                               |
|----------------|-------------------------------------------------|
| `pending`       |   Registered, awaiting admin approval       |
| `active`           |   Approval and operational                          |
| `suspended`  |   Frozen by admin due to policy violation  |
| `inactive`       |   Deactivated by seller                                   |

