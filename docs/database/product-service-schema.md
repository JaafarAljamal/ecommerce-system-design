# Product Service — Database Schema

```mermaid
erDiagram
    PRODUCTS {
        uuid id PK
        uuid seller_id FK
        string name
        string slug
        string description
        decimal price
        enum type
        float average_rating
        json attributes
        boolean is_active
        int delivery_days
        timestamp created_at
        timestamp updated_at
    }

    PRODUCT_VARIANTS {
        uuid id PK
        uuid product_id FK
        string sku
        decimal price
        int stock
        json options
    }

    PRODUCT_IMAGES {
        uuid id PK
        uuid product_id FK
        string url
        boolean is_primary
        int sort_order
    }

    CATEGORIES {
        uuid id PK
        uuid parent_id FK
        string name
        string slug
    }

    PRODUCT_CATEGORIES {
        uuid product_id FK
        uuid category_id FK
    }

    DISCOUNTS {
        uuid id PK
        uuid product_id FK
        decimal percentage
        enum type
        timestamp starts_at
        timestamp ends_at
        boolean is_active
    }

    PRODUCTS ||--o{ PRODUCT_VARIANTS : "has"
    PRODUCTS ||--o{ PRODUCT_IMAGES : "has"
    PRODUCTS ||--o{ PRODUCT_CATEGORIES : "belongs to"
    CATEGORIES ||--o{ PRODUCT_CATEGORIES : "has"
    CATEGORIES ||--o| CATEGORIES : "parent"
    PRODUCTS ||--o{ DISCOUNTS : "has"
```

## Product Type Values
| Type | Description |
|------|-------------|
| `physical` | Regular product with weight and dimensions |
| `food` | Perishable with prep time and temperature |
| `digital` | Downloadable, no shipping required |

## Discount Type Values
| Type | Description |
|------|-------------|
| `daily` | Daily deal |
| `seasonal` | Seasonal sale |
| `occasion` | Special occasion (e.g. Black Friday) |
| `seller` | Seller-defined discount |

## Notes
- `attributes` (JSONB) stores type-specific fields
- `PRODUCT_VARIANTS.options` stores variant details e.g. {"size": "M", "color": "red"}
- `CATEGORIES.parent_id` enables nested categories (e.g. Electronics > Phones > Android)

