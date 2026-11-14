# Awesome-QA-Team-Role-Based-Access-Control-RBAC

# Awesome QA Team Role-Based Access Control (RBAC)

> A curated list of top Role-Based Access Control (RBAC) platforms for QA teams — both SaaS/Hosted commercial identity and access management solutions and open-source alternatives for enforcing granular permissions, team isolation, and least-privilege access across testing infrastructure.
> 🌟 Star this repo if you find it useful! 🌟

---

## 📋 Table of Contents

- [SaaS / Hosted Platforms](#-saas--hosted-platforms)
  - [BrowserStack Role-Based Access Control](#browserstack-role-based-access-control)
  - [Okta](#okta)
  - [Microsoft Entra ID](#microsoft-entra-id)
  - [CyberArk Identity](#cyberark-identity)
  - [JumpCloud](#jumpcloud)
  - [OneLogin](#onelogin)
  - [Auth0](#auth0)
- [Open-Source Software](#-open-source-software)
  - [Keycloak](#keycloak)
  - [Casbin](#casbin)
  - [OpenRbac](#openrbac)
  - [go-iam](#go-iam)
  - [maintainerd/auth](#maintainerd-auth)
  - [Casdoor](#casdoor)
  - [Ory Kratos](#ory-kratos)
- [Comparison at a Glance](#-comparison-at-a-glance)
- [Contributing](#-contributing)

---

## 🟦 SaaS / Hosted Platforms

Commercial, cloud-hosted RBAC and Identity platforms. These are not open-source but are included as reference points for feature comparison.

### BrowserStack Role-Based Access Control
**Type:** Project-scoped RBAC for QA teams with API management
BrowserStack provides RBAC through its **Project API**, allowing admins to manage access controls at the project level . Key capabilities for QA team governance include:
- **Project role overrides**: Users and teams can receive explicit roles (Admin, User, Viewer) on specific projects, overriding their org-wide role 
- **Team-based access**: Teams can be granted project access with role overrides that apply to all members (excluding global viewers and users with explicit access) 
- **Custom roles**: BrowserStack supports custom roles (e.g., "ABC DEF custom role") assignable at project scope 
- **Bulk updates**: Grant or revoke access for multiple users and teams in a single request 
- **Access verification**: Check effective roles and access status for specific users or teams 

This makes it well-suited for QA organizations that need to isolate test environments by project while maintaining centralized team management.
**Best for:** QA teams already using BrowserStack wanting granular, project-scoped access control with team-level role inheritance and API automation.

### Okta
**Type:** Enterprise IAM with standard and custom admin roles
Okta provides standard admin roles (Super Admin, Org Admin, App Admin, Group Admin, Read-Only Admin) with predefined permissions, plus custom roles that bundle granular permissions like `okta.apps.manage` and `okta.users.appAssignment.manage` [citation:2]. Role assignments can be scoped to specific resources for least-privilege access. IAM-based standard roles (Access Certifications Admin, Access Requests Admin, Workflows Admin) are immutable but bundle preset permissions. For QA teams, this means test environment access can be isolated per team or per application.

### Microsoft Entra ID
**Type:** Identity platform with built-in and custom roles
Entra ID provides built-in roles and custom roles assignable at Tenant, Administrative Unit, or specific resource scope (groups, enterprise apps, app registrations) [citation:3]. A role assignment consists of three elements: security principal, role definition, and scope. Scope can be a Microsoft Entra resource (group, enterprise app, app registration), granting permissions over that resource only. PIM enables just-in-time role activation with approval workflows. For QA, this allows testers to be granted access only to specific app registrations or groups relevant to their testing scope.

### CyberArk Identity
**Type:** Identity administration with role-based rights and dynamic membership
CyberArk Identity uses roles as the unit of access control, assigning application access, administrative rights, and policies to roles rather than individual users [citation:4]. **Static roles** require manual member addition, while **dynamic roles** automatically add members based on attribute logic (e.g., all users from a specified country) written in JavaScript [citation:13]. Administrative rights control specific functionality like creating roles, running reports, and managing users. For QA, dynamic roles could auto-assign testers to environments based on department or location attributes.

### JumpCloud
**Type:** Cloud directory with custom admin roles
JumpCloud supports custom admin roles built from permission categories including Access Management, Application Management, Core Administration, and Device Management [citation:7]. Each category has granular sub-permissions (Full Access, View, No Access, or Create/Update/Delete). A maximum of **20 custom roles** is supported, and only Admins with Billing (Super Admin) can create and edit roles. For QA, custom roles can limit testers to device management and command execution without granting directory administration.

### OneLogin
**Type:** IAM with app-centric roles and attribute mappings
OneLogin roles are collections of apps; assigning a user to a role grants access to all apps in that role . Mappings can automatically assign users to roles based on directory attributes (e.g., Department = QA), with manual vs. mapping-based assignments managed consistently. For QA, a "QA Team" role can bundle the test management, CI/CD, and device cloud apps needed, with automatic provisioning via directory attributes.

### Auth0
**Type:** Developer IAM with API-level RBAC
Auth0 enables RBAC for APIs through Dashboard or Management API, controlling the intersection of requested and assigned permissions in the access token's `scope` claim [citation:10]. This allows QA teams to build custom tooling where API access is gated by role, ensuring test automation scripts can only access the endpoints their role permits.

---

## 🟩 Open-Source Software

Self-hosted, community-driven RBAC and Identity tools. The open-source ecosystem spans **full IAM platforms with SSO and federation**, **authorization libraries embeddable in applications**, and **lightweight standalone RBAC servers**.

### Keycloak
**Stack:** Java | **License:** Apache-2.0
Keycloak is the most widely deployed open-source IAM platform, providing realm-level roles, client roles, composite roles, and group-based role mapping [citation:18][citation:29]. Fine-grained authorization services allow defining resources, scopes, and policies (role, user, client, time, aggregate) for complex access decisions. It supports SSO, identity brokering, user federation (LDAP/AD), and social login. With 35k+ GitHub stars, it is the "old guard" of open-source IAM with the broadest protocol and enterprise integration support [citation:24].
**Best for:** QA organizations wanting a proven, full-featured IAM foundation with SSO, LDAP integration, and fine-grained authorization policies for test infrastructure access.

### Casbin
**Stack:** Go / Multi-language | **License:** Apache-2.0
Casbin is a powerful open-source access control library supporting ACL, RBAC, ABAC, and RESTful models, with implementations across Go, Java, Node.js, Python, PHP, .NET, and Rust [citation:28][citation:30]. Models and policies are defined in configuration files, allowing flexible role inheritance and custom access logic. It is used by Intel, IBM, Tencent Cloud, VMware, and Red Hat [citation:28]. **Important:** Casbin does NOT handle authentication (verifying passwords) or manage user/role lists—it is an authorization library embedded in your application [citation:30]. For QA, this means building custom test tooling where permissions are enforced in code.
**Best for:** Developers building custom QA tooling who want a flexible, embeddable authorization engine with model-driven RBAC/ABAC policies, accepting that user management and authentication must be handled separately.

### OpenRbac
**Stack:** Python / SQLite or MySQL | **License:** Open Source
OpenRbac is an online RBAC and centralized configuration tool accessible from a terminal or local web page, with **no JavaScript bundle** [citation:11]. Features include data scope control (all, custom departments, this department, this department and children), a unified permission check across menu, command line, and web surfaces, and audit logging. It uses SQLite by default or MySQL/MariaDB, with passwords stored in encrypted files under `0600` permissions. The first `db-init` seeds an administrator account [citation:11].
**Best for:** Small QA teams or individual testers wanting a lightweight, self-hosted RBAC tool for centralized configuration and access control without a heavy IAM platform.

### go-iam
**Stack:** Go / MongoDB / Redis / React | **License:** Apache-2.0 (Community)
go-iam is a lightweight, multi-tenant IAM server built in Go, providing authentication and fine-grained authorization for modern applications [citation:16]. Features include multi-tenancy with strict data isolation, Google/Microsoft/GitHub OAuth login, multiple clients per project with shared credentials, and custom role-based access control. The React-based Admin UI manages projects, users, roles, resources, and clients. Deploys via Docker Compose [citation:16].
**Best for:** QA teams wanting a modern, Go-based multi-tenant IAM with OAuth integration and a clean admin UI, accepting a younger ecosystem than Keycloak.

### maintainerd/auth
**Stack:** Go / PostgreSQL / Redis | **License:** Apache-2.0
maintainerd/auth is an open-source, self-hostable identity platform delivering full OAuth 2.0 + OIDC, MFA (TOTP, WebAuthn/passkeys), federation (Google, Microsoft, GitHub), and **fine-grained RBAC with granular permissions** [citation:22]. It supports multi-tenant isolation, service-to-service authorization with IAM policy bundles, audit logging with PII redaction, and pluggable secret management (Vault, AWS Secrets Manager, etc.). Deploys via Docker Compose [citation:22].
**Best for:** QA organizations wanting a modern, Apache-2.0 IAM with strong MFA, federation, and service-to-service authorization for test infrastructure, accepting a newer project than Keycloak.

### Casdoor
**Stack:** Go / React | **License:** Apache-2.0
Casdoor is a UI-first IAM platform built on the Casbin permission framework, supporting OAuth, OIDC, SAML, LDAP, MFA, and many third-party login plugins [citation:24]. It is described as the "best companion" if your project already uses Casbin, providing a web UI for managing roles, permissions, and users on top of the Casbin engine [citation:24]. With 13.9k GitHub stars, it is actively maintained.
**Best for:** QA teams already using Casbin for authorization who want a ready-made IAM UI and user management layer on top.

### Ory Kratos
**Stack:** Go | **License:** Apache-2.0
Ory Kratos is a "headless" identity management system, responsible for user authentication and identity management, while Ory Hydra handles OAuth2/OIDC [citation:24]. Its design philosophy is decoupling—it provides no forced UI, letting you fully control the frontend. It is well-suited for microservices architectures where you integrate only the components you need.
**Best for:** QA teams building custom test portals or internal tools who want headless identity management with full UI control, accepting that RBAC must be layered on top via Ory Keto or a similar engine.

---

## 📊 Comparison at a Glance

| Software | License | Stack | Project/Resource Scoping | Dynamic Roles | Admin UI | Self-Hosted | Best For |
|----------|---------|-------|--------------------------|---------------|----------|-------------|----------|
| **BrowserStack** | Proprietary | Cloud | ✅ Project-level | (Team inheritance) | ✅ | ❌ | QA project isolation |
| **Okta** | Proprietary | Cloud | ✅ Resource targets | (Custom roles) | ✅ | ❌ | Enterprise IAM |
| **Entra ID** | Proprietary | Cloud | ✅ Object scope | (Custom roles) | ✅ | ❌ | Microsoft ecosystem |
| **CyberArk** | Proprietary | Cloud | (Check) | ✅ JavaScript logic | ✅ | ❌ | Identity admin |
| **JumpCloud** | Proprietary | Cloud | (Check) | (Custom roles) | ✅ | ❌ | SMB cloud directory |
| **Keycloak** | Apache-2.0 | Java | ✅ Realm/client roles | (Composite) | ✅ | ✅ | Full IAM foundation |
| **Casbin** | Apache-2.0 | Multi-language | (Model-defined) | (Policy) | ❌ (Library) | ✅ | Embeddable authorization |
| **OpenRbac** | Open Source | Python/SQLite | ✅ Data scope | ❌ | ✅ (Web + CLI) | ✅ | Lightweight RBAC |
| **go-iam** | Apache-2.0 | Go/MongoDB | ✅ Multi-tenant | (Custom roles) | ✅ | ✅ | Modern multi-tenant IAM |
| **Casdoor** | Apache-2.0 | Go/React | ✅ (Casbin-based) | (Casbin) | ✅ | ✅ | Casbin UI layer |

---

## 🤝 Contributing

Contributions are welcome! If you know of an open-source RBAC or QA-specific access control tool that should be listed here, please open a pull request with:

- Project name and URL
- Brief description (1–2 sentences)
- Stack and license
- Why it stands out

Please ensure the project is actively maintained (releases within the last 12 months) and genuinely open-source. Note that the open-source ecosystem **splits** between **full IAM platforms** (Keycloak, go-iam, maintainerd/auth) that handle authentication, federation, and RBAC in one system, **authorization libraries** (Casbin) that are embedded in your application and leave user management to you, and **lightweight standalone RBAC tools** (OpenRbac) for simple centralized configuration. For QA teams specifically, **BrowserStack's Project API** offers the most purpose-built project-scoped access control, while **Keycloak** remains the strongest open-source foundation for self-hosted QA infrastructure access management.

---

## 📄 License

This list is released under CC0 1.0 Universal (Public Domain). The software projects listed retain their respective licenses.

---

⭐ **If this list helped you discover a useful tool, please star the repo!** ⭐
