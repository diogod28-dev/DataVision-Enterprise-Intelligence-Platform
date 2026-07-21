# Retail Intelligence Platform - Analytics and Event Catalog

## 1. Purpose

This document defines the initial analytics and event catalog for the Retail Intelligence Platform. Events should support Providence website learning first, then expand into CMS, catalog, CRM, inventory, orders, payments, reporting, and mobile workflows.

## 2. Event Design Principles

- Events must include organization context when available.
- Event names should be stable once reports depend on them.
- Event payloads should avoid unnecessary personal data.
- Operational records remain the source of truth for business state.
- Analytics events should describe meaningful activity, not implementation details.
- Modules should document their own events before release.
- Event changes that affect reports should be versioned or clearly migrated.

## 3. Event Naming

Use dot notation:

```text
module.entity.action
```

Examples:

| Event | Meaning |
| --- | --- |
| `cms.page.viewed` | A public or CMS-managed page was viewed. |
| `crm.inquiry.submitted` | A visitor submitted an inquiry. |
| `catalog.product.viewed` | A product detail view occurred. |
| `orders.order.placed` | An order was placed. |
| `payments.payment.completed` | A payment completed successfully. |
| `core.module.enabled` | A module was enabled for an organization. |

## 4. Standard Event Fields

| Field | Purpose |
| --- | --- |
| `event_name` | Stable event name. |
| `organization_id` | Organization context, such as Providence. |
| `occurred_at` | Event timestamp. |
| `source_module` | Module that emitted the event. |
| `actor_id` | Authenticated user when available. |
| `visitor_id` | Anonymous visitor identifier when used. |
| `session_id` | Session identifier when used. |
| `entity_type` | Related entity type, such as page or inquiry. |
| `entity_id` | Related entity identifier. |
| `properties` | Structured event-specific metadata. |
| `privacy_classification` | Public, internal, personal, sensitive, or restricted. |

## 5. Providence Website Events

| Event | Trigger | Reporting Use |
| --- | --- | --- |
| `cms.page.viewed` | Visitor loads a public page. | Traffic, content reach, page engagement. |
| `providence.collection.clicked` | Visitor selects a collection. | Collection interest. |
| `providence.lookbook.viewed` | Visitor opens the lookbook. | Visual content engagement. |
| `providence.whatsapp.clicked` | Visitor taps a WhatsApp action. | Contact intent. |
| `providence.social.clicked` | Visitor opens a social link. | Outbound social engagement. |
| `crm.inquiry.submitted` | Visitor submits an inquiry. | Lead conversion and contact demand. |

## 6. Core Events

| Event | Trigger |
| --- | --- |
| `core.organization.created` | Organization record is created. |
| `core.user.invited` | User invitation is issued. |
| `core.role.assigned` | Role is assigned to a user. |
| `core.module.enabled` | Module is enabled for an organization. |
| `core.configuration.updated` | Configuration setting changes. |

Core events may feed audit logs, analytics, and operational reporting.

## 7. Privacy Guidance

Analytics must avoid collecting sensitive data unless there is a documented business need and appropriate protection.

Do not place these values in general event metadata:

- Passwords or tokens.
- Full payment details.
- Government identifiers.
- Private notes.
- Unapproved personal contact details.
- Raw message bodies unless explicitly required and protected.

Inquiry and contact events should reference the operational inquiry record rather than duplicating all submitted personal data.

## 8. Event Readiness Checklist

Before an event is implemented, confirm:

- Event name follows the standard pattern.
- Trigger condition is clear.
- Required and optional properties are documented.
- Organization context is available where needed.
- Privacy classification is assigned.
- Reporting use is known.
- Tests confirm the event is emitted for important workflows.
- Changes are coordinated with reports or dashboards that depend on the event.
