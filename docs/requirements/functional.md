# Functional Requirements

## 1. Customer
- Can register with first name, last name, gender, email, and phone number
- Can login and logout securely
- Can change password
- Can edit profile information
- Can switch platform language between Arabic and English
- Can browse products as guest or logged-in user
- Can view product listing with name, price, and add-to-cart button
- Can filter products by category, price (low to high / high to low), rating, date added, and custom criteria
- Can view products with discounts showing old price (strikethrough), new price, and discount percentage
- Can view daily, monthly, seasonal, and occasion-based offers on a dedicated offers page
- Can enter a product detail page showing images, description, specifications, price, stock, and delivery duration
- Can search for products or categories with advanced search
- Can add products to cart from listing or detail page
- Can view and modify cart contents
- Can add products to wishlist
- Can add one or more delivery addresses with country, state, city, district, street, building, unit, and phone
- Can set a default delivery address
- Can see whether price includes shipping and tax
- Can proceed to checkout and payment gateway
- Can pay using multiple saved payment methods
- Can receive email invoice after purchase with itemized details, price, and tax
- Can track shipment after handoff to shipping provider
- Can view order history in profile
- Can request return for defective products within 7 days of delivery
- Can rate and review products after purchase
- Can rate sellers
- Can view ratings and reviews for products and sellers
- Can submit complaints and suggestions via a dedicated link
- Can experience consistent UI across web, mobile browser, and mobile app

## 2. Seller
- Can register with first name, middle name, last name, phone, email, national ID (both sides), and unique business name
- Can upload verification documents (national ID, passport, business license, tax document)
- Can create and manage store team members (owner, manager, accountant, staff)
- Can login after admin approval
- Can view own rating on dashboard
- Can add, edit, and delete products with images, description, stock quantity, pricing, and delivery duration
- Can apply discounts on products and view discount percentage
- Can create offers (daily, seasonal, occasion-based)
- Can manage product variants (size, color, etc.)
- Can receive platform notifications and email for new orders including product, quantity, buyer name, address, and phone
- Can view all products and stock quantities on a dedicated page
- Can generate financial and tax reports by date range
- Can view total order value and applied commission rate
- Can request payout of earnings to registered payout method
- Can register multiple payout methods (bank transfer, digital wallet) per country
- Can view store page accessible via business name link showing all seller products

## 3. Admin
- Can approve or reject seller registration requests with reason
- Can freeze (suspend) sellers who violate platform policies
- Can reactivate suspended sellers
- Can browse all products and sellers
- Can filter data by seller, gender, city, category, and date
- Can view total platform sales
- Can view per-seller sales, payout account references, and applied commission rates
- Can set platform default commission rate
- Can set custom commission rate per seller
- Can view seller net amount after commission deduction
- Can export reports in Excel or PDF format per seller including period, total sales, commission rate, and net amount
- Can manage platform settings
- Can review and approve or reject seller verification documents

## 4. Customer Support
- Can login as a customer support agent
- Can search for users by email, phone, or business name
- Can view buyer profile and order history (read-only)
- Can view seller products (read-only)
- Can view complaints and suggestions in chronological order
- Can register a complaint on behalf of a customer
- Can respond to complaints and suggestions
- Can update complaint or suggestion status (in progress, resolved, closed)
- Each complaint is prefixed with C and each suggestion with S in its reference number
- Can filter tickets by channel (call, email, platform)
- Can search tickets by reference number

## 5. Wallet & Financial
- Platform maintains a wallet per seller per currency
- Seller wallet balance is split into available, pending, and reserved amounts
- Every financial event is recorded as an immutable ledger transaction
- Commission is deducted automatically per order based on seller rate or platform default
- Seller can submit payout request from available balance
- Admin processes and approves payout requests
- Payout is executed via configured provider per region (e.g. PayTabs for UAE, iyzico for Turkey)
- All payout transactions record raw provider response for audit
- Wallet balances are periodically reconciled against ledger

## 6. Seller Team & Verification
- Seller owner can invite team members with defined roles (manager, accountant, staff)
- Each team member has scoped access based on role
- Seller must submit verification documents before activation
- Verification documents have expiry dates; expired documents trigger auto-suspension
- Admin reviews and approves or rejects documents with rejection reason

## 7. Platform & Localization
- Platform supports Arabic and English (with optional Turkish in future)
- Product names and descriptions can be entered in one or both languages
- Missing translations are auto-filled via AI translation and marked for review
- Each user has preferred language and timezone settings
- All sensitive user actions are recorded in audit logs with actor and IP address
- Soft delete is applied to users, sellers, and customers to preserve financial and legal history

## 8. Order Management (Additional)
- Customer can cancel order before it is shipped

## 9. Product Visibility & Curation
- Seller can temporarily hide a product from listings
- Admin can feature selected products on homepage
- Admin can feature selected stores on homepage
- Products go through Noon/Trendyol-style moderation: seller submits → admin reviews → admin approves or rejects before going live

## 10. Customer Support (Additional)
- Support agent can start a read-only impersonation session to view a customer's experience (all impersonation sessions are logged)

## 11. Shipping
- Platform supports multiple shipping providers per region
- Each shipment is tracked with a sequence of status events
- Shipping rates vary by zone and package weight
- Customer can view real-time shipment tracking

## 12. Wishlist
- Customer can view all wishlist items in one page
- Customer can move wishlist item directly to cart

## 13. Reviews
- Customer can vote a review as helpful or not helpful
- Customer can attach photos/video to a product review
- Reviews are only allowed after a verified purchase

## 14. Promotions & Coupons
- Admin and seller can create promotions with defined rules (date range, eligible products)
- Customer can apply coupon codes at checkout
- Platform tracks coupon usage per customer

## 15. Returns
- Customer can attach photo/video evidence when requesting a return
- Return request goes through defined status stages until refund is issued
- Approved returns automatically trigger a refund transaction in Wallet Service

## 16. Tax
- Platform applies tax rules per country (e.g. VAT in UAE, KDV in Turkey)
- Tax rate and type are configurable by admin per country

## 17. Search
- Customer can search products and sellers using full-text, typo-tolerant search
- Search results support filtering and ranking by relevance

## 18. Store Identity
- Each seller has a unique store slug for SEO-friendly store page URLs

## 19. Inventory Reservation
- Stock is reserved during checkout for a configurable period (e.g. 10 minutes)
- Expired reservations automatically release stock back to available pool
- Overselling must be prevented even under concurrent checkout attempts

## 20. Order Lifecycle
- Order moves through defined statuses: pending_payment, paid, processing, packed, shipped, delivered, cancelled, returned, refunded
- Each status transition is recorded with timestamp

## 21. Notification System
- Platform sends notifications via email, in-app, and push (future mobile app)
- Notification triggers include: order placed, order shipped, payout approved, verification approved, product rejected, return approved, dispute opened
- User can manage notification preferences per channel

## 22. Seller Vacation Mode
- Seller can temporarily pause store operations
- New orders are blocked while vacation mode is active
- Existing orders continue processing normally

## 23. Product Questions & Answers
- Customer can ask questions on a product page
- Seller can answer questions publicly
- Admin can moderate questions and answers

## 24. Fraud & Abuse Protection
- Platform detects and flags suspicious login activity
- Platform limits repeated failed login attempts (account lockout)
- Platform detects abnormal order behavior (e.g. rapid repeated orders, unusual amounts)

## 25. Dispute Management
- Customer can open a dispute when unsatisfied with a return outcome
- Seller can respond to a dispute
- Admin can mediate and issue a final decision

## 26. SEO
- Products and categories have SEO-friendly URLs
- Products support meta title, meta description, and Open Graph metadata

## 27. Seller Analytics
- Seller can view product views and conversion rate
- Seller can view best-selling products
- Seller can view revenue trends over time

## 28. Shipping Provider
- Shipping provider can receive shipment requests
- Shipping provider can update shipment status
- Shipping provider can submit tracking number
- Shipping provider can calculate shipping cost
- Shipping provider can define shipping zones

## 29. Product Draft & Resubmission
- Seller can save product as draft before submission
- Seller can edit and resubmit rejected products with admin feedback addressed

## 30. Coupon Limits
- Coupon can have a total usage limit
- Coupon can have a per-customer usage limit

## 31. Seller Performance Metrics
- Platform tracks seller order fulfillment rate
- Platform tracks seller order cancellation rate
- Platform tracks seller average response time to questions/orders
- Metrics used for seller ranking and trust scoring

## 32. Shipment Failure Handling
- Shipment can be marked as failed or returned-to-sender
- Failed shipments trigger notification to customer, seller, and admin

## 33. Review Moderation
- Admin can hide or remove abusive or fraudulent reviews

## 34. Notification Templates
- Admin can manage notification templates per trigger type and language

## 35. Legal Compliance
- Customer can request account deletion
- Platform retains financial records according to local regulations per country

