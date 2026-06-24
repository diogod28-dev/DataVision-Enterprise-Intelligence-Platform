# Retail Intelligence Platform - Core Module Specification

## 1. Purpose

The Core Platform module provides shared services and foundational entities used by all Retail Intelligence Platform modules. It supports organizations, users, roles, permissions, module registry, configuration, audit logging, event registry, and shared platform services.

Providence is the first organization and pilot implementation. The Core module must support Providence without becoming Providence-specific.

## 2. Core Module Responsibilities

The Core module is responsible for:

- Organization management.
- User identity integration.
- Organization membership.
- Roles and permissions.
- Module registry and module enablement.
- Configuration management.
- Audit logging.
- Event registry.
- Shared validation, naming, and service patterns.
- Organization-scoped access foundations.
- Core database entities.

The Core module should not own business capabilities that belong to future modules such as catalog, CRM, inventory, orders, payments, CMS, analytics, or reporting.

## 3. Organizations

Organizations represent retail businesses or tenants using the platform.

Organization responsibilities:

- Identify the business entity.
- Store organization status and public identity.
- Define primary domain or slug.
- Scope business data.
- Own theme, content, module enablement, and configuration.
- Support future multi-organization administration.

| Field | Purpose |
| --- | --- |
| `id` | Stable organization identifier. |
| `name` | Display name. |
| `slug` | URL-safe or system-safe organization key. |
| `status` | Active, inactive, suspended, archived, or similar state. |
| `primary_domain` | Main public domain if applicable. |
| `created_at` | Creation timestamp. |
| `updated_at` | Last update timestamp. |

Providence should be represented as an organization record or equivalent organization configuration.

## 4. Users

Users represent people who can access protected platform functionality.

User responsibilities:

- Store identity reference and basic profile information.
- Support authentication integration.
- Link to one or more organizations.
- Receive roles and permissions through organization membership.
- Support audit attribution.

| Field | Purpose |
| --- | --- |
| `id` | Stable user identifier. |
| `email` | Login or contact email. |
| `name` | Display name. |
| `status` | Active, invited, disabled, or archived. |
| `last_login_at` | Optional last login timestamp. |
| `created_at` | Creation timestamp. |
| `updated_at` | Last update timestamp. |

Authentication details may be handled internally or by a future identity provider, but Core should preserve a stable user identity for permissions and audit logs.

## 5. Organization Users

Organization users associate users with organizations.

Responsibilities:

- Support users belonging to one or more organizations.
- Store membership status.
- Support organization-scoped role assignment.
- Prevent cross-organization access without explicit membership or platform-level permission.

| Field | Purpose |
| --- | --- |
| `id` | Stable membership identifier. |
| `organization_id` | Related organization. |
| `user_id` | Related user. |
| `status` | Active, invited, suspended, or removed. |
| `created_at` | Creation timestamp. |
| `updated_at` | Last update timestamp. |

## 6. Roles

Roles group permissions into assignable access profiles.

Role responsibilities:

- Define common access patterns.
- Support platform-level and organization-level access.
- Simplify permission assignment.
- Make authorization easier to review.

| Role | Purpose |
| --- | --- |
| Platform Administrator | Manage platform-wide settings and organizations. |
| Organization Administrator | Manage one organization and its users, modules, and settings. |
| Content Manager | Manage pages, media, and publishing workflows. |
| Catalog Manager | Manage products, categories, collections, and product media. |
| Operations Manager | Manage inventory, orders, fulfillment, and operational workflows. |
| Analyst | View reports, dashboards, and analytics. |
| Viewer | Read-only access to permitted areas. |

Roles should be configurable over time, but initial defaults should be simple and documented.

## 7. Permissions

Permissions define specific actions a user may perform.

Permission standards:

- Use stable permission keys.
- Use lowercase dot notation.
- Keep permissions action-oriented.
- Scope permissions by module and resource.
- Avoid hard-coding permissions in unrelated modules.

Recommended pattern:

```text
module.resource.action
```

| Permission | Meaning |
| --- | --- |
| `core.organizations.view` | View organizations. |
| `core.organizations.manage` | Create or update organizations. |
| `core.users.manage` | Manage users and memberships. |
| `core.roles.manage` | Manage roles and role assignments. |
| `core.modules.manage` | Manage module registry and enablement. |
| `cms.pages.publish` | Publish CMS pages. |
| `reporting.reports.view` | View reports. |

Authorization should evaluate both permission and organization scope.

## 8. Module Registry

The module registry defines modules available to the platform.

Responsibilities:

- Register module key, name, status, and description.
- Identify module ownership and dependencies.
- Support enablement per organization.
- Provide a foundation for admin navigation and module configuration.

| Field | Purpose |
| --- | --- |
| `id` | Stable module identifier. |
| `key` | Unique module key such as `cms` or `catalog`. |
| `name` | Display name. |
| `description` | Module purpose. |
| `status` | Planned, active, deprecated, or disabled. |
| `version` | Optional module version. |
| `created_at` | Creation timestamp. |
| `updated_at` | Last update timestamp. |

Candidate module keys:

- `core`
- `providence_theme`
- `cms`
- `catalog`
- `crm`
- `inventory`
- `orders`
- `payments`
- `analytics`
- `reporting`
- `mobile`

## 9. Organization Module Settings

Organization module settings define which modules are enabled for each organization.

Responsibilities:

- Enable or disable modules by organization.
- Store module-specific organization configuration.
- Support phased rollout.
- Support Providence as the initial pilot configuration.

| Field | Purpose |
| --- | --- |
| `id` | Stable setting identifier. |
| `organization_id` | Related organization. |
| `module_id` | Related module. |
| `enabled` | Whether the module is enabled. |
| `settings` | Structured module configuration. |
| `created_at` | Creation timestamp. |
| `updated_at` | Last update timestamp. |

Module settings should be validated against each module's documented configuration schema.

## 10. Configuration Management

Configuration management provides controlled settings for platform, organization, module, and theme behavior.

| Level | Purpose |
| --- | --- |
| Platform default | Shared default used across all organizations. |
| Environment | Development, test, UAT, or production-specific values. |
| Organization | Organization-specific overrides. |
| Module | Module behavior and feature settings. |
| Theme | Brand and presentation settings. |
| User preference | Optional personal preferences for administrative users. |

Configuration standards:

- Keep sensitive values out of committed files.
- Validate configuration types and allowed values.
- Track changes to important settings through audit logs.
- Document defaults and override behavior.
- Avoid using configuration to bypass required permissions.

## 11. Audit Logging

Audit logging records important changes and actions.

Audit logging should capture:

- Actor.
- Organization context.
- Action.
- Source module.
- Entity type and identifier.
- Previous and new values where appropriate and safe.
- Timestamp.
- Request identifier or correlation identifier.
- IP address or user agent where appropriate.

Candidate audited actions:

- Organization created or updated.
- User invited, disabled, or removed.
- Role assigned or removed.
- Permission changed.
- Module enabled or disabled.
- Configuration changed.
- CMS content published.
- Payment, order, or inventory state changed in future modules.
- Report exported when sensitive.

Audit logs should not store passwords, secret tokens, full payment details, or unnecessary personal information.

## 12. Event Registry

The event registry defines platform and module events that may feed analytics, reporting, audit, integrations, or notifications.

Event registry responsibilities:

- Define event names.
- Identify source module.
- Describe trigger conditions.
- Define required and optional properties.
- Identify entity references.
- Classify privacy or sensitivity.
- Support reporting and analytics governance.

Event naming pattern:

```text
module.entity.action
```

| Event | Meaning |
| --- | --- |
| `core.organization.created` | An organization was created. |
| `core.user.invited` | A user invitation was issued. |
| `core.role.assigned` | A role was assigned to a user. |
| `core.module.enabled` | A module was enabled for an organization. |
| `core.configuration.updated` | A configuration setting was changed. |

Events should be stable once reports or integrations depend on them.

## 13. Shared Services

Core should provide or coordinate shared services that modules can reuse.

| Service | Purpose |
| --- | --- |
| Organization Context Service | Resolve current organization and enforce scope. |
| Authorization Service | Evaluate roles, permissions, and policies. |
| Configuration Service | Resolve platform and organization settings. |
| Audit Service | Record important actions. |
| Event Service | Publish and validate platform events. |
| Module Registry Service | Register and query module availability. |
| Validation Utilities | Shared validation patterns. |
| Notification Foundation | Future common notification dispatch support. |
| Media Reference Foundation | Future shared media ownership conventions. |

Shared services should remain generic. Module-specific business logic should stay inside the owning module.

## 14. Core Database Entities

Initial Core database entities may include:

| Entity | Purpose |
| --- | --- |
| `organizations` | Retail organizations using the platform. |
| `users` | Platform and organization users. |
| `organization_users` | User membership in organizations. |
| `roles` | Assignable permission groups. |
| `permissions` | Fine-grained action grants. |
| `role_permissions` | Permissions assigned to roles. |
| `organization_user_roles` | Roles assigned to organization users. |
| `modules` | Available platform modules. |
| `organization_modules` | Organization-specific module enablement and settings. |
| `configuration_settings` | Platform, organization, module, or theme settings. |
| `audit_logs` | Important administrative and business actions. |
| `event_definitions` | Registered platform and module events. |

These entities should follow database standards from the Database Design and Module Development Guidelines documents.

## 15. Core API Candidates

Future Core APIs may include:

| Endpoint Area | Purpose |
| --- | --- |
| Organizations | Manage organizations and organization status. |
| Users | Manage users and invitations. |
| Memberships | Manage organization membership. |
| Roles | Manage roles and assignments. |
| Permissions | List and inspect permission definitions. |
| Modules | Register and inspect available modules. |
| Organization Modules | Enable, disable, and configure modules. |
| Configuration | Manage organization and module settings. |
| Audit Logs | Search and review audit history. |
| Event Definitions | Inspect event registry definitions. |

Core APIs must enforce authentication, authorization, organization scope, and the API standards defined in this documentation set.

## 16. Core Module Readiness Checklist

Before Core is considered implementation-ready, confirm:

- Organization model is defined.
- User and membership model is defined.
- Roles and permissions are documented.
- Module registry and enablement model are defined.
- Configuration hierarchy is documented.
- Audit log fields and retention expectations are defined.
- Event registry format is documented.
- Core database entities are reviewed.
- Organization boundary rules are testable.
- Providence can be represented as a pilot organization without special-case core logic.
