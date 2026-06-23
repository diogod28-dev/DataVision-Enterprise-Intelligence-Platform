# Retail Intelligence Platform - Data Warehouse Design

## 1. Purpose

The Retail Intelligence Platform should eventually support a dedicated analytics and reporting layer that is separate from day-to-day operational processing. The data warehouse design defines how operational commerce, content, customer, inventory, payment, and event data can be transformed into reliable analytical models for executive reporting and business intelligence.

Providence is the first implementation, but the warehouse design should support future multi-organization reporting while preserving organization-level data boundaries.

## 2. OLTP vs OLAP Strategy

The platform should distinguish between operational transaction processing and analytical processing.

| Area | OLTP Strategy | OLAP Strategy |
| --- | --- | --- |
| Primary purpose | Run the platform and store current business state. | Analyze historical performance and trends. |
| Data shape | Normalized operational tables. | Dimensional models, summaries, and star schemas. |
| Workload | Inserts, updates, lookups, workflow transitions. | Aggregations, joins, filtering, trend analysis. |
| Users | Application services, admins, customers, integrations. | Executives, analysts, reporting users, dashboards. |
| Latency | Real time or near real time for operations. | Batch, scheduled, or near real time depending on KPI needs. |
| Examples | Orders, customers, products, stock balances, payments. | Sales facts, customer activity facts, inventory movement facts. |

The operational database should remain the system of record. The warehouse should be optimized for querying, historical analysis, repeatable KPI definitions, and executive reporting.

## 3. Data Warehouse Goals

- Provide consistent cross-module reporting.
- Preserve historical facts even when operational records change.
- Support organization-scoped reporting for multi-organization deployment.
- Enable executive dashboards across sales, customers, inventory, marketing, and operations.
- Reduce expensive analytical load on OLTP tables.
- Create reusable data models for future retail organizations.
- Establish a foundation for forecasting, segmentation, and advanced analytics.

## 4. Conceptual Analytics Architecture

A future analytics architecture may include the following layers:

| Layer | Responsibility |
| --- | --- |
| Source Systems | OLTP tables, application events, external payment providers, marketing sources, and mobile events. |
| Ingestion Layer | Extracts data from source systems using scheduled jobs, event streams, or incremental loads. |
| Staging Layer | Stores raw or lightly transformed source data for validation and reconciliation. |
| Transformation Layer | Cleans, conforms, deduplicates, and models data into facts and dimensions. |
| Warehouse Layer | Stores star schemas, conformed dimensions, and historical analytical tables. |
| Metrics Layer | Defines governed KPIs, calculations, and business rules. |
| Reporting Layer | Powers dashboards, scheduled reports, exports, and executive views. |

The first analytics implementation may begin with simple summary tables or reporting views. Over time, it should evolve toward a governed warehouse model.

## 5. Star Schema Concepts

A star schema organizes analytical data around measurable business events.

- Fact tables store events, transactions, counts, quantities, and monetary measures.
- Dimension tables store descriptive context used for filtering, grouping, and slicing reports.
- The fact table sits at the center of the model.
- Dimensions connect to facts through stable keys.
- The grain of each fact table must be clearly defined.

Example: a sales fact table may store one row per order item. It can join to date, organization, customer, product, channel, and location dimensions.

## 6. Fact Table Design Principles

Fact tables should:

- Declare a clear grain.
- Include `organization_key` or equivalent organization reference.
- Include relevant date keys for reporting periods.
- Store additive measures where possible.
- Preserve source system identifiers for traceability.
- Avoid storing descriptive attributes that belong in dimensions.
- Support late-arriving updates through controlled refresh or adjustment logic.
- Include load timestamps and data quality status where useful.

## 7. Candidate Fact Tables

| Fact Table | Suggested Grain | Example Measures |
| --- | --- | --- |
| `fact_sales` | One row per order item. | Gross sales, net sales, discount amount, tax amount, quantity sold, margin estimate. |
| `fact_orders` | One row per order. | Order count, order total, item count, refund amount, fulfillment duration. |
| `fact_payments` | One row per payment transaction. | Authorized amount, captured amount, refunded amount, failed amount. |
| `fact_customer_activity` | One row per customer event or summarized customer-day. | Visits, inquiries, purchases, repeat purchase flags, engagement score. |
| `fact_inventory_movement` | One row per stock movement. | Quantity in, quantity out, adjustment quantity, reserved quantity. |
| `fact_inventory_snapshot` | One row per product-location-date. | On-hand quantity, available quantity, reserved quantity, stock value. |
| `fact_marketing_engagement` | One row per campaign, channel, page, or event depending on source. | Impressions, clicks, sessions, inquiries, conversion count. |
| `fact_content_engagement` | One row per content item or page-day. | Page views, unique visitors, clicks, average engagement time. |
| `fact_operational_workflow` | One row per workflow state transition. | Time in state, transition count, completion count, exception count. |

Fact tables should be introduced as source modules mature. The Providence marketing website phase may start with content and inquiry engagement facts before sales, inventory, and payment facts exist.

## 8. Dimension Table Design Principles

Dimension tables should:

- Provide business-friendly descriptive attributes.
- Support filtering, grouping, and drill-down paths.
- Include organization context when values are organization-specific.
- Preserve historical changes when reporting requires them.
- Use surrogate keys in the warehouse where practical.
- Retain source identifiers for reconciliation.

Slowly changing dimension handling should be selected by business need. For example, a product category change may need historical preservation, while a typo correction may not.

## 9. Candidate Dimension Tables

| Dimension Table | Purpose |
| --- | --- |
| `dim_date` | Standard calendar, fiscal period, week, month, quarter, and year attributes. |
| `dim_time` | Optional time-of-day analysis for traffic, order, or operations reporting. |
| `dim_organization` | Organization, tenant, brand, status, and reporting hierarchy. |
| `dim_customer` | Customer profile, segment, lifecycle status, and acquisition source. |
| `dim_product` | Product, SKU, variant, category, collection, and brand attributes. |
| `dim_category` | Category hierarchy and merchandising structure. |
| `dim_location` | Store, warehouse, fulfillment, or inventory location. |
| `dim_channel` | Web, mobile, store, marketplace, social, email, or other source channel. |
| `dim_campaign` | Marketing campaign, source, medium, and campaign grouping. |
| `dim_payment_provider` | Payment provider, method type, and provider account grouping. |
| `dim_order_status` | Order lifecycle status and status groupings. |
| `dim_content` | Page, content block, campaign page, or CMS content item. |
| `dim_module` | Source platform module for event and operational reporting. |

## 10. Conformed Dimensions

Some dimensions should be shared across multiple fact tables so reports can compare different business areas consistently.

Important conformed dimensions include:

- `dim_date`
- `dim_organization`
- `dim_customer`
- `dim_product`
- `dim_location`
- `dim_channel`
- `dim_campaign`

For example, sales, marketing, and customer activity facts should all use the same organization and date dimensions. This allows executive dashboards to compare traffic, orders, revenue, and inquiries for the same reporting period.

## 11. Grain Examples

Defining grain avoids ambiguous reporting.

| Fact | Grain | Notes |
| --- | --- | --- |
| Sales | One row per order item. | Best for product, category, and margin analysis. |
| Orders | One row per order. | Best for order lifecycle and average order value. |
| Inventory snapshot | One row per product, location, and date. | Best for stock health and aging reports. |
| Inventory movement | One row per inventory transaction. | Best for audit and movement analysis. |
| Content engagement | One row per content item, organization, and date. | Best for marketing website analytics. |
| Customer activity | One row per customer event or customer-day. | Grain should be chosen based on reporting volume and detail needs. |

## 12. Executive Reporting Requirements

Executive reporting should provide a clear view of business health without requiring operational users to interpret raw data.

Executive reports should support:

- Organization-level summary views.
- Multi-organization comparison where permissions allow it.
- Period-over-period trends.
- Revenue and sales performance.
- Customer acquisition, retention, and engagement.
- Inventory health and stock risk.
- Marketing effectiveness.
- Operational workflow performance.
- Exceptions, alerts, and negative trends.
- Drill-down from executive summary to supporting detail.

Executive dashboards should distinguish between finalized metrics and provisional metrics when data is still being processed.

## 13. Data Refresh Strategy

The platform may use different refresh patterns depending on business need:

| Pattern | Use Case |
| --- | --- |
| Batch refresh | Daily executive reporting, historical summaries, inventory snapshots. |
| Incremental refresh | Orders, payments, customer activity, and recent event reporting. |
| Near real-time stream | High-value operational alerts, live dashboards, or marketing campaign monitoring. |
| Manual rebuild | Backfills, corrections, and model changes. |

Initial implementations should favor simple, reliable batch or incremental refreshes before adopting streaming complexity.

## 14. Data Quality and Reconciliation

Warehouse data should be trusted. Data quality controls should include:

- Source record counts compared with loaded counts.
- Payment and order reconciliation.
- Duplicate event detection.
- Missing dimension key handling.
- Invalid date or organization references.
- Late-arriving data handling.
- Failed load alerts.
- Audit fields for load time, source system, and transformation version.

Operational totals and warehouse totals should be reconcilable for financial and executive reporting.

## 15. Security and Access Control

Warehouse and reporting access should preserve organization boundaries.

Required controls include:

- Organization-scoped report filters.
- Role-based access to executive, financial, customer, and operational metrics.
- Protection of personally identifiable information.
- Limited access to raw event metadata.
- Audit logging for exports and sensitive report access.
- Clear separation between platform-wide administrative reporting and organization-level reporting.

## 16. Initial Implementation Recommendation

For the first Providence marketing website phase, the warehouse does not need to be fully implemented. The platform should prepare for it by:

- Capturing consistent analytics events when tracking is introduced.
- Including organization context in events and future business records.
- Defining KPI names and formulas before dashboards are built.
- Avoiding direct executive reports over unstable operational tables.
- Starting with simple daily summaries for content engagement and inquiries if analytics are enabled.

The long-term goal is a governed warehouse that allows Providence and future organizations to make decisions from consistent, trusted metrics.
