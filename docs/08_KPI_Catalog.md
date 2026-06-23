# Retail Intelligence Platform - KPI Catalog

## 1. Purpose

This KPI catalog defines the primary performance indicators the Retail Intelligence Platform should support as commerce, CRM, inventory, marketing, analytics, and reporting modules mature.

The initial Providence phase is a mobile-friendly marketing website, so many commerce and operational KPIs are future-facing. They are included here to guide module design, event capture, database modeling, and reporting requirements.

## 2. KPI Governance Principles

KPIs should be governed so reports remain consistent across organizations and modules.

Each KPI should eventually define:

- Business definition.
- Calculation formula.
- Source tables or events.
- Refresh frequency.
- Owner or accountable module.
- Required filters and dimensions.
- Security classification.
- Known exclusions or caveats.

KPI names should be stable. If a calculation changes materially, the change should be documented and historical reports should explain the difference.

## 3. Common KPI Dimensions

Most KPIs should be filterable by common dimensions:

- Organization.
- Date range.
- Channel.
- Location.
- Product.
- Category.
- Customer segment.
- Campaign.
- Order status.
- Payment status.
- Fulfillment status.

Not every KPI needs every dimension, but shared dimensions make executive and cross-module reporting more useful.

## 4. Executive KPIs

Executive KPIs summarize overall business health.

| KPI | Definition | Primary Source |
| --- | --- | --- |
| Gross Revenue | Total sales value before discounts, refunds, taxes, or other adjustments. | Orders, order items. |
| Net Revenue | Revenue after discounts, refunds, and adjustments according to finance rules. | Orders, payments, refunds. |
| Total Orders | Count of completed or accepted orders in the reporting period. | Orders. |
| Average Order Value | Net revenue divided by total orders. | Orders, payments. |
| Customer Count | Number of known customers associated with the organization. | CRM. |
| New Customers | Customers first created or first purchased during the period. | CRM, orders. |
| Repeat Customer Rate | Percentage of purchasing customers with more than one purchase. | CRM, orders. |
| Conversion Rate | Percentage of visits, sessions, or leads that complete a target action. | Analytics, orders, inquiries. |
| Inventory Value | Estimated value of current stock on hand. | Inventory, catalog pricing. |
| Stock Risk Count | Count of products below reorder threshold or at risk of stockout. | Inventory. |
| Marketing Conversion Contribution | Orders, inquiries, or revenue attributed to marketing campaigns. | Analytics, campaigns, orders. |
| Operational SLA Compliance | Percentage of workflows completed within expected service targets. | Orders, fulfillment, operational events. |

## 5. Sales KPIs

Sales KPIs measure commercial performance.

| KPI | Definition | Notes |
| --- | --- | --- |
| Gross Sales | Total order item value before discounts and refunds. | Useful for demand analysis. |
| Net Sales | Sales after discounts and refunds. | Should align with finance reporting rules. |
| Sales Growth Rate | Percentage change in sales compared with a prior period. | Requires comparable date ranges. |
| Order Count | Number of orders in the reporting period. | Should define included statuses. |
| Average Order Value | Net sales divided by order count. | May exclude cancelled orders. |
| Items Per Order | Total quantity sold divided by order count. | Helps understand basket size. |
| Discount Amount | Total discount value applied to orders or order items. | Should separate promotion and manual discounts. |
| Refund Amount | Total refunded value in the period. | May be reported by refund date or original order date. |
| Refund Rate | Refunded orders or refunded amount divided by eligible sales. | Useful for quality and satisfaction trends. |
| Sales by Product | Net or gross sales grouped by product. | Requires product dimension. |
| Sales by Category | Net or gross sales grouped by category. | Requires category hierarchy. |
| Sales by Channel | Sales grouped by web, mobile, store, marketplace, or other channel. | Requires channel attribution. |
| Payment Success Rate | Successful payment attempts divided by total payment attempts. | Payments module. |
| Failed Payment Rate | Failed payment attempts divided by total payment attempts. | Useful for checkout health. |

## 6. Customer KPIs

Customer KPIs measure acquisition, retention, engagement, and relationship health.

| KPI | Definition | Notes |
| --- | --- | --- |
| Total Customers | Count of known customer records. | Should exclude deleted or merged duplicates. |
| New Customers | Customers added during the reporting period. | Can be based on profile creation or first purchase. |
| Active Customers | Customers with qualifying activity during the period. | Activity definition should be documented. |
| Repeat Customers | Customers with more than one completed purchase. | Requires order history. |
| Repeat Customer Rate | Repeat customers divided by total purchasing customers. | Key retention indicator. |
| Customer Lifetime Value | Cumulative net revenue attributed to a customer. | Can mature into predictive CLV later. |
| Average Purchase Frequency | Average number of purchases per customer in a period. | Useful for retention analysis. |
| Lead Count | Number of leads or inquiries captured. | Relevant from marketing website phase onward. |
| Lead Conversion Rate | Leads that become customers or orders divided by total leads. | Requires CRM and order linkage. |
| Customer Segment Growth | Change in size of a defined customer segment. | Requires segment definitions. |
| Consent Coverage | Percentage of customers with valid communication preferences or consent records. | Important for compliant communication. |
| Customer Engagement Score | Composite score based on visits, clicks, inquiries, purchases, or other activity. | Should be explicitly defined before use. |

## 7. Inventory KPIs

Inventory KPIs measure stock availability, movement, and operational risk.

| KPI | Definition | Notes |
| --- | --- | --- |
| Stock on Hand | Current physical quantity available in inventory. | May differ from sellable quantity. |
| Available Stock | Stock on hand minus reserved or unavailable quantity. | Used for availability decisions. |
| Reserved Stock | Quantity reserved for carts, orders, or fulfillment. | Requires reservation tracking. |
| Out-of-Stock Count | Count of products or SKUs with zero available stock. | Should be scoped by location when relevant. |
| Low-Stock Count | Count of products or SKUs below reorder threshold. | Requires reorder rules. |
| Stockout Rate | Percentage of tracked SKUs that are out of stock. | Useful for assortment health. |
| Inventory Turnover | Cost of goods sold divided by average inventory value. | Requires cost and valuation data. |
| Days of Inventory on Hand | Estimated days current stock can support demand. | Requires sales velocity. |
| Stock Adjustment Count | Number of manual or system stock adjustments. | Helps detect process issues. |
| Stock Adjustment Value | Value impact of inventory adjustments. | Requires cost or valuation logic. |
| Fulfillment Availability Rate | Percentage of ordered items available for fulfillment. | Requires order and inventory integration. |
| Inventory Accuracy | Difference between expected and counted stock. | Requires physical counts or reconciliation. |

## 8. Marketing KPIs

Marketing KPIs measure website, campaign, and content performance.

| KPI | Definition | Notes |
| --- | --- | --- |
| Website Sessions | Count of tracked website sessions. | Initial Providence website candidate metric. |
| Unique Visitors | Count of unique visitors within the reporting period. | Depends on tracking approach and privacy rules. |
| Page Views | Total page view events. | CMS and analytics modules. |
| Landing Page Conversion Rate | Target actions divided by landing page sessions. | Target action may be inquiry, signup, or purchase. |
| Inquiry Count | Number of submitted inquiries or contact forms. | Relevant in initial marketing phase. |
| Inquiry Conversion Rate | Inquiries divided by sessions or visitors. | Formula should specify denominator. |
| Campaign Click-Through Rate | Clicks divided by impressions. | Requires campaign source data. |
| Campaign Conversion Rate | Conversions divided by campaign visits or clicks. | Requires attribution. |
| Cost Per Lead | Campaign spend divided by leads generated. | Requires marketing spend data. |
| Return on Ad Spend | Revenue attributed to campaign divided by advertising spend. | Future capability. |
| Content Engagement Rate | Engaged visits divided by total visits for a content item. | Engagement definition must be documented. |
| Organic Traffic Share | Organic sessions divided by total sessions. | Requires channel classification. |

## 9. Operational KPIs

Operational KPIs measure workflow performance, reliability, and service quality.

| KPI | Definition | Notes |
| --- | --- | --- |
| Order Processing Time | Time from order placement to processing completion. | Requires order status history. |
| Fulfillment Time | Time from order placement or release to fulfillment completion. | Requires fulfillment events. |
| On-Time Fulfillment Rate | Fulfillments completed within target time divided by total fulfillments. | SLA target should be configurable. |
| Cancellation Rate | Cancelled orders divided by total eligible orders. | Should define eligible order statuses. |
| Return Rate | Returned orders or items divided by eligible sold orders or items. | Can be measured by count or value. |
| Exception Count | Count of workflow exceptions requiring manual intervention. | Requires exception events. |
| Admin Activity Count | Count of administrative changes or actions. | Audit log source. |
| API Error Rate | Failed API responses divided by total API requests. | Requires platform monitoring. |
| Report Delivery Success Rate | Successful scheduled reports divided by report delivery attempts. | Reporting module. |
| Data Load Success Rate | Successful analytics loads divided by scheduled load attempts. | Warehouse operations. |
| Module Availability | Percentage of time a module is operational. | Requires monitoring. |
| Support Issue Volume | Number of support issues or incidents. | Future support integration. |

## 10. KPI Implementation Readiness

Before a KPI is implemented in production reporting, it should have:

- A written definition.
- A calculation formula.
- A source data owner.
- A validation approach.
- Required dimensions.
- Security classification.
- Refresh frequency.
- Display format.
- Acceptance criteria.

KPIs used in executive reporting should be especially stable and reconciled against operational sources when financial or customer-impacting decisions depend on them.
