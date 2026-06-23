# Retail Intelligence Platform - Database Design

## 1. Database Design Overview

The Retail Intelligence Platform database should support a phased product strategy. The first phase may require little or no dynamic data for the Providence marketing website, but the long-term platform should be ready for multi-organization retail operations, modular capabilities, analytics, and reporting.

The database design should favor clear ownership, organization-scoped records, auditability, and future reporting needs.

## 2. Design Goals

- Support Providence as the first organization.
- Allow future organizations to use the same platform.
- Keep platform core data separate from module-specific data.
- Support module enablement per organization.
- Prepare for CMS, catalog, CRM, inventory, orders, payments, analytics, reporting, and mobile applications.
- Capture events that can power analytics.
- Enable trustworthy reporting across business domains.

## 3. Core Data Principles

- Most business records should include an `organization_id`.
- Shared lookup or platform configuration records should be clearly identified.
- Each module should own its primary tables.
- Cross-module relationships should use stable identifiers.
- Records should include created and updated timestamps.
- Important administrative or financial changes should be auditable.
- Soft deletion should be considered for business records where history matters.
- Reporting requirements should influence event and status history design.

## 4. Conceptual Subject Areas

| Subject Area | Purpose |
| --- | --- |
| Platform Core | Organizations, users, roles, permissions, configuration, modules, audit logs. |
| CMS | Pages, content blocks, navigation, media, publishing states. |
| Catalog | Products, categories, attributes, variants, prices, product media. |
| CRM | Customers, leads, inquiries, preferences, segments, customer activity. |
| Inventory | Locations, stock items, balances, adjustments, transfers, reservations. |
| Orders | Carts, orders, order items, status history, fulfillment records. |
| Payments | Payment attempts, transactions, refunds, provider references. |
| Analytics | Events, sessions, campaign attribution, metric summaries. |
| Reporting | Report definitions, report runs, scheduled reports, saved filters. |
| Mobile | Devices, push tokens, mobile sessions, notification preferences. |

## 5. Core Platform Entities

### organizations

Represents a retail organization using the platform.

Typical fields:

- `id`
- `name`
- `slug`
- `status`
- `primary_domain`
- `created_at`
- `updated_at`

### users

Represents administrative or platform users.

Typical fields:

- `id`
- `email`
- `name`
- `status`
- `created_at`
- `updated_at`

### organization_users

Associates users with organizations and supports future multi-organization access.

Typical fields:

- `id`
- `organization_id`
- `user_id`
- `status`
- `created_at`
- `updated_at`

### roles and permissions

Define access control. Roles may be platform-wide or organization-specific depending on future requirements.

Typical entities:

- `roles`
- `permissions`
- `role_permissions`
- `organization_user_roles`

### modules

Defines available platform modules.

Typical fields:

- `id`
- `key`
- `name`
- `description`
- `status`

### organization_modules

Defines which modules are enabled for each organization.

Typical fields:

- `id`
- `organization_id`
- `module_id`
- `enabled`
- `settings`
- `created_at`
- `updated_at`

## 6. CMS Data Model

Future CMS tables may include:

- `pages`
- `page_versions`
- `content_blocks`
- `navigation_menus`
- `navigation_items`
- `media_assets`
- `campaigns`

CMS records should include `organization_id` so each organization can own its own content. Publishing states should support draft, published, scheduled, and archived content.

## 7. Catalog Data Model

Future catalog tables may include:

- `products`
- `product_variants`
- `categories`
- `product_categories`
- `collections`
- `collection_products`
- `product_attributes`
- `product_attribute_values`
- `product_media`
- `price_lists`
- `product_prices`

Catalog records should support organization ownership, publish status, search-friendly identifiers, and analytics references.

## 8. CRM Data Model

Future CRM tables may include:

- `customers`
- `customer_contacts`
- `leads`
- `inquiries`
- `customer_notes`
- `customer_segments`
- `customer_segment_members`
- `communication_preferences`
- `consent_records`

CRM design should distinguish between anonymous visitors, leads, and known customers where appropriate. Consent and communication preferences should be stored in a way that supports compliance and audit needs.

## 9. Inventory Data Model

Future inventory tables may include:

- `inventory_locations`
- `stock_items`
- `stock_balances`
- `stock_adjustments`
- `stock_transfers`
- `stock_reservations`

Inventory should reference catalog variants or stock keeping units. Stock changes should be traceable and should include reason codes, source module references, and timestamps.

## 10. Orders Data Model

Future order tables may include:

- `carts`
- `cart_items`
- `orders`
- `order_items`
- `order_status_history`
- `fulfillments`
- `fulfillment_items`
- `returns`
- `return_items`

Orders should preserve the product, price, tax, discount, and customer details needed for historical reporting, even if catalog or customer records later change.

## 11. Payments Data Model

Future payment tables may include:

- `payment_methods`
- `payment_attempts`
- `payment_transactions`
- `refunds`
- `payment_provider_accounts`

Sensitive card data should not be stored directly. Provider tokens, transaction references, statuses, and non-sensitive metadata should be stored as needed for reconciliation and reporting.

## 12. Analytics Data Model

Analytics should be event-driven. Candidate tables include:

- `analytics_events`
- `analytics_sessions`
- `campaign_attributions`
- `metric_daily_summaries`
- `module_event_definitions`

An `analytics_events` record should generally include:

- `id`
- `organization_id`
- `event_type`
- `source_module`
- `actor_type`
- `actor_id`
- `session_id`
- `entity_type`
- `entity_id`
- `occurred_at`
- `metadata`

Event metadata should be structured consistently and should not become the only place where important operational facts are stored.

## 13. Reporting Data Model

Reporting tables may include:

- `report_definitions`
- `report_filters`
- `report_runs`
- `scheduled_reports`
- `report_exports`
- `dashboard_widgets`

Reports should be organization-scoped and permission-aware. Standard report templates may be shared by the platform, while saved filters and schedules may belong to a specific organization or user.

## 14. Mobile Data Model

Future mobile support may require:

- `mobile_devices`
- `push_tokens`
- `mobile_sessions`
- `notification_preferences`
- `notification_deliveries`

Mobile records should be tied to organizations and users or customers where applicable.

## 15. Multi-Organization Data Strategy

The preferred long-term strategy is organization-scoped data within shared platform tables, with strict application-level and database-level safeguards against cross-organization access.

For higher isolation requirements, future deployment options may include:

- Separate databases per organization.
- Separate schemas per organization.
- Shared database with organization-scoped rows.

The initial design should keep `organization_id` available on business tables so that any of these strategies can be evaluated later with less rework.

## 16. Reporting and Analytics Considerations

Operational tables should remain reliable sources of truth for current business state. Analytics and reporting structures should support:

- Historical trend analysis.
- Daily, weekly, and monthly summaries.
- Funnel and conversion reporting.
- Product and category performance.
- Customer lifecycle reporting.
- Inventory movement reporting.
- Payment and revenue reconciliation.

Status history tables are important because many reports depend on when a record moved from one state to another, not only its current state.

## 17. Data Governance

The platform should define governance practices for:

- Naming conventions.
- Migration management.
- Seed data.
- Data retention.
- Backup and recovery.
- Audit logs.
- Personally identifiable information.
- Payment metadata.
- Analytics event taxonomy.
- Report definition ownership.

## 18. Initial Phase Recommendation

For the Providence marketing website phase, keep database usage minimal unless dynamic content is required. If database tables are introduced early, prioritize:

- `organizations`
- `modules`
- `organization_modules`
- CMS-oriented page or content tables only if needed.
- Basic analytics events only if the implementation is ready to capture them consistently.

This avoids premature complexity while preserving the platform direction.
