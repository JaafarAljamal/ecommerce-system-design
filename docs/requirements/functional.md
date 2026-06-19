# Functional Requirements
> Organized by Bounded Context (Domain-Driven Design)
> Each section maps directly to one or more Microservices.

---

## 1. Identity & Access Management
> **Maps to:** User Service

### Customer
- Can register with first name, last name, gender, email, and phone number
- Can login and logout securely
- Can change password
- Can edit profile information
- Can switch platform language between Arabic and English
- Can manage notification preferences per channel
- Can request account deletion (platform retains financial records per local regulations)

### Seller
- Can register with first name, middle name, last name, phone, email, national ID (both sides), and unique business name with unique store slug
- Can upload verification documents (national ID, passport, business license, tax document)
- Can login after admin approval
- Can invite team members with defined roles (owner, manager, accountant, staff)
- Can view own rating and performance metrics on dashboard
- Can temporarily pause store operations (vacation mode — blocks new orders while existing orders continue)

### Admin
- Can approve or reject seller registration requests with reason
- Can approve or reject seller verification documents with reason
- Can freeze or reactivate sellers
- Can manage platform settings (default commission rate, tax rules, etc.)

### Customer Support
- Can login as a support agent
- Can search users by email, phone, or business name
- Can start a read-only impersonation session to view a customer's experience (all sessions are logged)

---

## 2. Catalog & Product Management
> **Maps to:** Catalog Service

### Seller
- Can save product as draft before submission
- Can submit product for admin review
- Can edit and resubmit rejected products with admin feedback addressed
- Can add product with name, description, images, specifications, category, price, stock, delivery duration, and variants (size, color, etc.)
- Can set product type (physical, food, digital)
- Can temporarily hide a product from listings
- Can manage product questions and answer them publicly

### Admin
- Can approve or reject submitted products with reason
- Can feature selected products on homepage
- Can feature selected stores on homepage
- Can feature selected categories on homepage
- Can hide or remove abusive or fraudulent reviews
- Can moderate product questions and answers
- Products follow Noon/Trendyol-style moderation workflow:
  `draft → pending_review → approved → published → hidden → rejected`

### Customer
- Can browse products as guest or logged-in user
- Can view product listing with name, price, discount badge, and add-to-cart button
- Can enter a product detail page showing images, description, specifications, price, stock, delivery duration, and seller info
- Can view products with discounts showing old price (strikethrough), new price, and discount percentage
- Can ask questions on a product page
- Can view seller store page via business name slug showing all seller products
- Can add products to wishlist
- Can view all wishlist items and move them directly to cart
- Products and categories have SEO-friendly URLs with meta title, meta description, and Open Graph metadata

---

## 3. Search & Discovery
> **Maps to:** Search Service (OpenSearch/Elasticsearch)

### Customer
- Can search products and sellers using full-text, typo-tolerant search
- Can filter search results by category, price, rating, seller, and date added
- Can sort results by relevance, price (low to high / high to low), rating, and newest
- Can browse categories via navigation bar at top of platform
- Can view daily, monthly, seasonal, and occasion-based offers on a dedicated offers page

---

## 4. Inventory Management
> **Maps to:** Inventory Service

### System
- Stock is reserved during checkout for a configurable period (e.g. 10 minutes)
- Expired reservations automatically release stock back to available pool
- Overselling is prevented even under concurrent checkout attempts
- Seller can set stock quantity as finite or unlimited

### Seller
- Can view all products and stock quantities on a dedicated inventory page
- Can update stock quantities at any time

---

## 5. Cart & Checkout
> **Maps to:** Cart Service (cart) + Order Service (checkout/order creation)

### Customer
- Can add products to cart from listing or detail page
- Can view and modify cart contents (quantity, remove items)
- Can see whether price includes shipping and tax
- Can see delivery duration per product
- Can apply coupon codes at checkout
- Can select delivery address at checkout
- Can select payment method at checkout
- Can proceed to payment gateway

---

## 6. Orders & Fulfillment
> **Maps to:** Order Service

### Customer
- Can view order history in profile
- Can track shipment in real time after handoff to shipping provider
- Can cancel order before it is shipped
- Can receive email invoice after purchase with itemized details, price, and tax

### Seller
- Can receive platform notification and email for new orders including product, quantity, buyer name, address, and phone
- Can update order status through fulfillment stages

### System — Order Lifecycle
Order moves through defined statuses with timestamp recorded at each transition:

pending_payment → paid → processing → packed → shipped → delivered
↓
cancelled / returned / refunded

---

## 7. Return & Dispute Management
> **Maps to:** Return Service + Support Service

### Customer
- Can request return for defective or unsatisfactory products within 7 days of delivery
- Can attach photo/video evidence when requesting a return
- Can open a dispute if unsatisfied with return outcome
- Can view return and dispute status

### Seller
- Can respond to return requests
- Can respond to disputes

### Admin
- Can mediate disputes and issue final decisions
- Approved returns automatically trigger a refund transaction in Wallet Service

---

## 8. Payment
> **Maps to:** Payment Service

### Customer
- Can pay using multiple saved payment methods (card, digital wallet)
- Payment methods are saved securely via provider token (no raw card data stored)

### System
- Platform supports multiple payment providers per region:
  - UAE: PayTabs, Telr, Stripe
  - Turkey: iyzico, PayTR
  - Syria: local wallets (Sham Cash, Haram Cash), cash on delivery
- Failed payments are recorded with failure reason
- Idempotency is enforced to prevent duplicate charges on retry

---

## 9. Wallet & Payouts
> **Maps to:** Wallet Service

### Seller
- Can view wallet balance split into available, pending, and reserved amounts
- Can register multiple payout methods per region (bank transfer, digital wallet)
- Can submit payout request from available balance
- Can view commission rate applied to each order
- Can view total earnings and net amount after commission

### Admin
- Can set platform default commission rate
- Can set custom commission rate per seller
- Can view per-seller sales, payout references, and commission breakdown
- Can process and approve payout requests
- Can export per-seller financial reports in Excel or PDF including period, total sales, commission rate, and net amount

### System
- Commission is deducted automatically per order
- Every financial event is recorded as an immutable ledger transaction
- Wallet balances are periodically reconciled against ledger
- Payout transactions record raw provider response for audit
- Platform retains financial records per local regulations

---

## 10. Shipping
> **Maps to:** Shipping Service

### Customer
- Can view real-time shipment tracking via tracking number or link
- Can view estimated delivery duration before purchase

### Seller
- Receives notification when order is ready for handoff to shipping provider

### Shipping Provider
- Can receive shipment requests from platform
- Can update shipment status at each stage
- Can submit tracking number upon pickup
- Can calculate shipping cost based on zone and weight
- Can define shipping zones and rates
- Can mark shipment as failed or returned-to-sender

### System
- Failed or returned shipments trigger notifications to customer, seller, and admin
- Platform supports multiple shipping providers per region

---

## 11. Promotions & Coupons
> **Maps to:** Promotion Service

### Seller
- Can create discounts on products with percentage and validity period
- Can create time-based offers (daily, seasonal, occasion-based)
- Can view discount percentage applied to each product

### Admin
- Can create platform-wide promotions
- Can manage coupon codes with:
  - Total usage limit
  - Per-customer usage limit
  - Validity period
  - Eligible products or categories

### Customer
- Can view discounted products with old price (strikethrough), new price, and discount percentage
- Can apply coupon code at checkout
- Can browse dedicated offers page

---

## 12. Reviews & Ratings
> **Maps to:** Review Service

### Customer
- Can rate and review products after a verified purchase only
- Can attach photos or video to a product review
- Can vote a review as helpful or not helpful
- Can rate sellers
- Can view ratings and reviews for products and sellers

### Admin
- Can hide or remove abusive or fraudulent reviews

---

## 13. Support & Complaints
> **Maps to:** Support Service

### Customer
- Can submit complaints (C) and suggestions (S) via dedicated link
- Each ticket has a unique reference number:
  - Complaint: `C-{YEAR}-{SEQUENCE}` e.g. C-2026-000001
  - Suggestion: `S-{YEAR}-{SEQUENCE}` e.g. S-2026-000001
- Can view ticket status and agent responses

### Customer Support Agent
- Can search users by email, phone, or business name
- Can view buyer profile and order history (read-only)
- Can view seller products (read-only)
- Can register a complaint on behalf of a customer
- Can view complaints and suggestions in chronological order
- Can filter tickets by channel (call, email, platform)
- Can search tickets by reference number
- Can respond to complaints and suggestions
- Can update ticket status: `in_progress → resolved → closed`
- Ticket channel is recorded (call, email, platform)

---

## 14. Notifications
> **Maps to:** Notification Service

### System
- Platform sends notifications via email, in-app, and push (future mobile app)
- Notification triggers include:
  - Order placed, packed, shipped, delivered, cancelled
  - Payout requested, approved, executed
  - Verification document approved or rejected
  - Product approved or rejected
  - Return approved or rejected
  - Dispute opened, responded to, resolved
  - Shipment failed or returned-to-sender
  - Seller verification document expiring soon

### Admin
- Can manage notification templates per trigger type and language

### Customer / Seller
- Can manage notification preferences per channel (email, push, SMS)

---

## 15. Analytics & Reporting
> **Maps to:** Analytics Context (consumes Domain Events from Order, Wallet, Review, Search)

### Seller
- Can view product views and conversion rate
- Can view best-selling products
- Can view revenue trends over time
- Can generate financial and tax reports by date range
- Can view own fulfillment rate, cancellation rate, and average response time

### Admin
- Can view total platform sales
- Can filter data by seller, gender, city, category, and date
- Can export reports in Excel or PDF format

---

## 16. Fraud & Security
> **Maps to:** Fraud/Risk Context (consumes Domain Events from User, Payment, Order, Wallet)

### System
- Platform detects and flags suspicious login activity
- Platform limits repeated failed login attempts (account lockout with configurable threshold)
- Platform detects abnormal order behavior
- All sensitive user actions are recorded in audit logs with actor, timestamp, and IP address
- Support agent impersonation sessions are fully logged

---

## 17. Localization & Accessibility
> **Cross-Cutting Concern** — applied across all Contexts, not a Bounded Context itself

### Customer / Seller
- Can switch platform language between Arabic and English (Turkish planned)
- Each user has preferred language and timezone settings

### Seller
- Can enter product name and description in one or both languages
- Missing translations are auto-filled via AI and marked for human review
- Can edit auto-translated content

### System
- Platform delivers consistent experience across web, mobile browser, and mobile app

---

## 18. Domain Events
> Core events published across the platform. Consumed by Wallet, Notification, Analytics, Fraud, and Search Contexts.

| Event | Published By | Consumed By |
|-------|--------------|--------------|
| `user.registered` | Identity | Notification, Analytics |
| `seller.approved` | Identity | Notification, Catalog |
| `seller.verification.expired` | Identity | Notification, Fraud |
| `product.submitted` | Catalog | Notification |
| `product.approved` | Catalog | Notification, Search |
| `product.rejected` | Catalog | Notification |
| `cart.abandoned` | Cart | Notification, Analytics |
| `order.placed` | Order | Inventory, Notification, Analytics, Fraud |
| `order.paid` | Order | Wallet, Notification |
| `order.shipped` | Order | Notification |
| `order.delivered` | Order | Wallet, Review, Notification |
| `order.cancelled` | Order | Inventory, Wallet, Notification |
| `return.requested` | Return | Notification |
| `return.approved` | Return | Wallet, Notification |
| `payout.requested` | Wallet | Notification |
| `payout.completed` | Wallet | Notification, Analytics |
| `review.submitted` | Review | Catalog, Notification |
| `dispute.opened` | Support | Notification, Fraud |

## 19. Media Management
> **Maps to:** Media Context (shared file storage for products, reviews, verification documents, user avatars)

- Platform provides centralized file upload, storage, and retrieval
- Supports image and document types (JPEG, PNG, PDF)
- Generates CDN-backed URLs from storage keys
- Used by: Catalog (product images), Review (review media), Identity (avatars, verification documents)

## 20. Configuration & Feature Flags
> **Cross-Cutting Concern**

- Admin can manage platform-wide settings (default commission rate, tax rules)
- Admin can enable or disable features per region or seller tier (feature flags)


