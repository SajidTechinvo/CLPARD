# Post-Demo Discussion - Security Module

## Core Critique

The current security module is **primarily a frontend/UI kit**. It needs to be coupled with **server-side enforcement** to be a real security solution, not just a presentation layer.

## What's Being Asked: A Lite IAM Platform

Consolidate everything into a full-stack security framework across the following layers:

---

### 1. Authentication & User Lifecycle (IAM Layer)

- Username/email + password login, email verification, account recovery
- MFA support: OTP, authenticator apps, third-party IDPs
- SSO integration (OIDC/SAML)
- Anti-automation (bot protection)
- Future-proof: passkeys, passwordless
- **External Directory Connectors**: LDAP bind authentication and attribute lookup, Active Directory connector (LDAP/Kerberos), federated directory sync (user/group import), directory group-to-role mapping

### 2. Session & Token Management

- Server-managed sessions (cookie-based) + JWT support
- OAuth flows
- Token attributes: TTL, refresh rate, revocation
- Secure storage + TLS transport
- "Logout everywhere" + per-device/session visibility
- CSRF enforcement for cookie-based sessions

### 3. Authorization Services

- RBAC (roles) + ABAC (attributes)
- Configurable policies for login, verification, password strength, resets, lockout
- **Deny-by-default** for all rules/policies
- Versioning + approval workflows for policy changes

### 4. Middleware Layer

- HTTPS security headers, CSRF protection
- Rate limiting + request validation
- Lockout rules

### 5. Logging & Audit Pipeline

- Not just a log viewer — a full **end-to-end audit pipeline**
- Standardized event taxonomy (e.g., `auth.login.success`, `token.revoked`)
- **Admin action audit trail** — all admin operations are first-class auditable events, not optional logging; required taxonomy: `admin.user.unlock`, `admin.user.disable`, `admin.user.invite`, `admin.user.mfa_reset`, `admin.role.assigned`, `admin.role.removed`, `admin.policy.changed` (with before/after snapshot), `admin.key.rotated`, `admin.config.changed`; each event captures actor identity, target entity, timestamp, source IP, and before/after state where applicable
- **Data masking in logs** — sensitive fields (passwords, tokens, PII, card numbers) must never appear in plaintext; define a sensitive field registry that the logging pipeline scrubs before write
- Tamper-proof logs with unique log IDs
- Access-controlled, with retention policies
- Alerting + SIEM integration

### 6. Encryption & Key Management

- Manage API keys, DB credentials, signing keys
- Key lifecycle management
- Encrypted storage for sensitive data
- KMS integration

### 7. API Security

- Default secure headers
- HTTPS-only enforcement — disallow unencrypted HTTP; disable weak and obsolete TLS versions (SSLv2, SSLv3, TLS 1.0, TLS 1.1) and deprecated cipher suites; enforce strong cipher negotiation

- **API response data masking** — sensitive fields in API responses masked or omitted based on caller role (e.g., partial email masking, token truncation)

### 7b. Service-to-Service Authentication

- mTLS for internal service communication (mutual certificate validation)
- Service accounts with scoped, non-human credentials
- JWT propagation between services — forwarding user context across service boundaries with audience/scope validation
- Zero-trust posture: no implicit internal network trust between services

### 8. Password Security

- Secure password vault/storage
- Store old hashes to prevent password reuse

### 9. Admin Console

- User management (create, invite, disable, unlock, reset MFA)
- View risk events
- Manage all policies and rules centrally
- Every admin action performed via the console must emit a corresponding audit log event — no silent admin operations

### 10. Multi-Tenancy Support

> Broader than pure security, but critical when hosting multiple organizations on a shared platform.

- Tenant provisioning and isolation (row-level, schema-level, or separate DB depending on strategy)
- Per-tenant configuration: policies, password rules, lockout thresholds, MFA requirements, allowed IDPs
- Scoped RBAC — roles and permissions resolved within a tenant boundary, no cross-tenant leakage
- Tenant-aware audit logs — events always carry a tenant context identifier
- Cross-tenant admin access with explicit elevated privilege model (super-admin vs. tenant-admin distinction)

### 11. Data Privacy & Consent Management

> Relevant for GDPR and any regulation requiring user control over their data.

- **Consent capture** — collect and store user consent at registration and for specific data processing activities
- **Versioned consent records** — when privacy policy or terms change, re-consent is required; event is logged with policy version reference
- **Consent withdrawal** — users can revoke consent; system must trigger data deletion or anonymization (right to erasure)
- **Consent audit trail** — who consented to what, when, and under which policy version
- **Link to compliance presets** — e.g., GDPR preset auto-enables consent gates at registration and data export

---

## Additional UI Components Called Out

- Email/Phone verification flows
- Password reset (token validation + set)
- MFA challenge + recovery codes
- Device/passkey management (WebAuthn)
- SSO login selectors
- Access denied + request access patterns
- Unified security settings page

## Nice-to-Have

- **Compliance presets** — pre-built configuration profiles for regulatory frameworks like **NESA, ISO 27001, DESC** (Dubai), FedRAMP, etc., so teams can apply a compliance profile and get policy defaults automatically.

---

## Bottom Line

The current work is seen as a **UI component library** and needs to be elevated into a **lightweight IAM framework** with real backend enforcement, audit capabilities, encryption management, and compliance readiness. The frontend components are a piece of it, but they need to be backed by server-side logic to be meaningful.

---

## Must-Have Priority List

> **Purpose:** These are the non-negotiable items that must ship for the security module to be considered production-grade. Everything else can be deferred without introducing security risk. Use this list to focus team effort and discussion.

### Pillar 1: Server-Side Hardening *(from Phase 1)*

> Directly addresses the core critique — turns the UI kit into a real security layer.

| # | Item | Why It's Non-Negotiable | Source |
|---|------|------------------------|--------|
| 1 | Security headers middleware (HSTS, CSP, X-Content-Type, X-Frame-Options) | Without these you're exposed to clickjacking, MIME sniffing, and XSS | Phase 1 |
| 2 | CSRF protection for cookie-based flows | Cookie-based sessions without CSRF = account takeover vector | Phase 1 |
| 3 | Rate limiting middleware | Without it, brute-force attacks on login/register are trivial | Phase 1 |
| 4 | HTTPS-only + disable weak/obsolete TLS | Plaintext credentials over HTTP is a dealbreaker for any security review | Phase 1 |
| 5 | Deny-by-default on all policies | The single most important authorization principle — fail-open is a critical flaw | Phase 1 |
| 6 | Tighten CORS (currently allows all origins) | Open CORS = any site can make authenticated requests on behalf of users | Phase 1 |
| 7 | Password reuse prevention | The `password_histories` table already exists — just needs the check wired up | Phase 1 |
| 8 | Request validation middleware | Unvalidated input is the root cause of injection attacks | Phase 1 |

### Pillar 2: Authentication Completeness *(from Phase 2)*

> The login system exists but has gaps that make it incomplete for production use.

| # | Item | Why It's Non-Negotiable | Source |
|---|------|------------------------|--------|
| 9 | Email verification enforcement | The flag exists but the flow doesn't — unverified accounts are a security hole | Phase 2 |
| 10 | Anti-automation (CAPTCHA/rate-limit on login/register) | Without this, credential stuffing attacks are wide open | Phase 2 |
| 11 | Recovery codes for MFA | Users locked out of MFA with no recovery = support nightmare and potential data loss | Phase 2 |
| 12 | Configurable password/lockout policies (from DB, not hardcoded) | Hardcoded policies can't adapt to different client security requirements | Phase 2 |
| 13 | Admin ability to disable, unlock users, and reset MFA | Without these, admins have no way to respond to compromised accounts | Phase 2 |

### Pillar 3: Audit Trail Essentials *(from Phase 3)*

> You don't need the full SIEM pipeline yet, but you **must** have a credible audit trail.

| # | Item | Why It's Non-Negotiable | Source |
|---|------|------------------------|--------|
| 14 | Standardized event taxonomy (`auth.login.success`, `auth.login.failure`, `token.revoked`, etc.) | Without consistent event naming, logs are useless for investigation | Phase 3 |
| 15 | Admin action audit trail (`admin.*` namespace) | Every compliance framework requires this — no silent admin operations | Phase 3 |
| 16 | Data masking in logs (sensitive field registry) | Passwords/tokens in plaintext logs = a breach waiting to happen | Phase 3 |
| 17 | Unique log IDs (GUID per event) | Can't trace or correlate events without them | Phase 3 |
| 18 | Append-only logs + integrity hash chain (tamper-proof) | If logs can be tampered with, the audit trail has zero credibility | Phase 3 |

### Pillar 4: Encryption Basics *(from Phase 5)*

> Not the full KMS — just the essentials to keep secrets safe.

| # | Item | Why It's Non-Negotiable | Source |
|---|------|------------------------|--------|
| 19 | Encrypted storage for sensitive config (API keys, DB creds, signing keys) | Secrets in plaintext config = one file read away from full compromise | Phase 5 |
| 20 | Signing key rotation support | If a JWT signing key is compromised and can't be rotated, every session is compromised | Phase 5 |
| 21 | API key lifecycle (create, rotate, revoke) | Without revocation capability, a leaked API key is a permanent exposure | Phase 5 |

### Pillar 5: Admin Operations Core *(from Phase 6)*

> Not the full dashboard — just the operational minimum to manage security incidents.

| # | Item | Why It's Non-Negotiable | Source |
|---|------|------------------------|--------|
| 22 | User lifecycle management (create, invite, disable, unlock, reset MFA) | Admins need to respond to security incidents in real-time | Phase 6 |
| 23 | All admin actions emit audit events | Directly tied to Pillar 3 — no silent operations anywhere in the system | Phase 6 |

---

### What Can Be Deferred (no immediate security risk)

| Category | Deferred Items |
|----------|---------------|
| **Advanced Auth** | SSO/OIDC/SAML, LDAP/AD connectors, passkeys/WebAuthn, cookie-based sessions alongside JWT, device management UI |
| **Service-to-Service** | mTLS, service accounts, JWT propagation, zero-trust internal posture |
| **Advanced Authorization** | ABAC engine, policy versioning with approval workflows |
| **Full Audit Pipeline** | SIEM export, alerting service, retention policies, advanced log access controls |
| **Full KMS** | KMS integration interface (Azure/AWS/HashiCorp), centralized key management service |
| **Compliance** | Compliance presets (NESA, ISO 27001, DESC, FedRAMP) |
| **Multi-Tenancy** | Tenant isolation, per-tenant policies, scoped RBAC, tenant-aware logs |
| **Consent Management** | Consent capture, versioning, withdrawal, erasure, GDPR presets |
| **UI Extras** | SSO login selectors, access denied patterns, unified settings page, API response field masking |
| **OTP Channels** | SMS/email OTP (TOTP already works as the primary MFA method) |

> **Bottom line:** 23 must-have items across 5 pillars. Ship these first — they transform the module from a UI kit into a credible, production-grade security framework. Everything in the deferred list is valuable but can follow once the foundation is solid.

---

## Implementation Summary

### What's Already Built vs. What's Needed

| Area                              | Already Done                                          | Gap                                                                                                   |
| --------------------------------- | ----------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Authentication**                | Login, register, password reset, TOTP MFA             | SSO, third-party IDPs, OTP via SMS/email, anti-automation, passkeys                                   |
| **External Directory Connectors** | None                                                  | LDAP connector, Active Directory (LDAP/Kerberos), directory sync, group-to-role mapping               |
| **Session & Tokens**              | JWT + refresh tokens, per-device tracking, revocation | Cookie-based sessions, OAuth flows, CSRF, mobile token best practices                                 |
| **Authorization**                 | RBAC (roles + permissions), lockout                   | ABAC, deny-by-default enforcement, policy versioning/approval workflows                               |
| **Middleware**                    | Auth/authz middleware, CORS                           | Security headers, CSRF, rate limiting, request validation                                             |
| **Audit & Logging**               | Basic audit log + viewer                              | Tamper-proof, log IDs, retention, SIEM, alerting, event taxonomy, admin action taxonomy, data masking |
| **Encryption/KMS**                | BCrypt, JWT signing                                   | Key lifecycle, KMS integration, encrypted storage                                                     |
| **API Security**                  | JWT-protected endpoints                               | Secure headers, HTTPS-only/obsolete TLS disablement, API response masking                             |
| **Service-to-Service Auth**       | None                                                  | mTLS, service accounts, inter-service JWT propagation                                                 |
| **Password Security**             | Hashing, history table exists                         | Reuse prevention check, configurable policies                                                         |
| **Admin Console**                 | User list, role CRUD                                  | Full admin (invite, disable, unlock, risk events, policy mgmt), all actions emit audit events         |
| **Multi-Tenancy**                 | None                                                  | Tenant isolation, per-tenant policies, scoped RBAC, tenant-aware audit logs                           |
| **Data Privacy & Consent**        | None                                                  | Consent capture, versioning, withdrawal, erasure, consent audit trail                                 |

---

### Proposed Implementation Phases

#### Phase 1: Harden What Exists (Middleware + API Security)

- Add security headers middleware (HSTS, CSP, X-Content-Type, X-Frame-Options)
- Add rate limiting middleware (ASP.NET RateLimiter)
- Add CSRF protection for cookie-based flows
- Add request validation middleware
- Tighten CORS (currently allows all origins)
- Enforce HTTPS-only; disable weak/obsolete TLS versions and cipher suites at server configuration level
- Enforce deny-by-default on all policies
- Add password reuse prevention (check against password_histories table)

#### Phase 2: Complete the IAM Core (Authentication Enhancements)

- Email verification enforcement (flag exists, needs flow)
- OTP via email/SMS (extend MFA beyond just TOTP)
- Recovery codes generation and storage
- Anti-automation: add CAPTCHA/rate-limit on login/register endpoints
- Configurable password/lockout/login policies stored in DB (not hardcoded)
- Admin ability to invite, disable, unlock users, reset MFA

#### Phase 3: Logging & Audit Pipeline

- Standardize event taxonomy (auth.login.success, auth.login.failure, token.revoked, etc.)
- Define and implement admin action event taxonomy (`admin.*` namespace: `admin.user.unlock`, `admin.user.disable`, `admin.policy.changed`, `admin.role.assigned`, `admin.key.rotated`, etc.)
- Enforce that all admin command handlers emit audit events (pipeline behavior/decorator pattern); capture before/after state for policy changes
- Implement sensitive field registry and log-level data masking before write (PII, tokens, secrets never in plaintext logs)
- Add unique log IDs (GUID per event)
- Make audit logs append-only + add integrity hash chain (tamper-proof)
- Add retention policies (configurable purge/archive)
- Add log access controls (beyond just audit:read)
- Build alerting service (webhook/email on critical events)
- Add SIEM export interface (syslog/CEF format or webhook)

#### Phase 4: Advanced Auth (SSO, OAuth, Passkeys)

- OAuth 2.0 / OIDC server support (token issuance)
- SSO integration with external IDPs (OIDC/SAML)
- LDAP connector for external directory authentication and attribute lookup
- Active Directory connector (LDAP bind or Kerberos pass-through)
- Directory group-to-role mapping for federated directories
- Cookie-based session support alongside JWT
- WebAuthn/passkey support (FIDO2)
- Device management UI (list, rename, revoke credentials)
- Service account model (non-human identities with scoped tokens)
- mTLS setup for internal service-to-service calls
- JWT propagation pattern for user context across microservice boundaries

#### Phase 5: Encryption & Key Management

- Centralized key management service
- API key lifecycle (create, rotate, revoke)
- Encrypted storage for sensitive config (Data Protection API or Azure Key Vault)
- KMS integration interface (pluggable: Azure Key Vault, AWS KMS, HashiCorp Vault)
- Signing key rotation support

#### Phase 6: Admin Console + Authorization Enhancements + Compliance

- ABAC engine (attribute-based rules alongside RBAC)
- Policy versioning with approval workflows
- Full admin dashboard: user lifecycle, risk events, policy management
- Unified security settings page (frontend)
- Access denied + request access patterns
- Compliance presets (NESA, ISO 27001, DESC, FedRAMP) — pre-configured policy bundles
- SSO login selector components
- API response field masking based on caller role and context
- **Multi-tenancy foundation**: tenant isolation model (row-level or schema-level), per-tenant policy scoping, tenant-aware RBAC resolution, tenant context on all audit events, cross-tenant super-admin access model
- **Consent management**: consent capture and versioning at registration, consent withdrawal + data deletion/anonymization request handling, consent audit trail integrated into the audit pipeline, GDPR preset triggers consent gates automatically

---

### Delivery Schedule (Mar 4 – Apr 3, 2026)

> **Start date:** March 4, 2026 | **Target end:** April 3, 2026 | **Total working days:** ~23
>
> Phases 2, 3, and 5 run in parallel across two tracks to compress the timeline.
> Phase 6 is the heaviest phase and begins at the month boundary — delivery extends ~1.5 weeks into April.

#### Sprint Overview

| Sprint | Dates | Track A | Track B |
| ------ | ---------------------- | -------- | ----------------------- |
| **1** | Mar 4 – Mar 13 | Phase 1 | — *(all hands on Phase 1)* |
| **2** | Mar 16 – Mar 25 | Phase 2 | Phase 3 + Phase 5 |
| **3** | Mar 26 – Apr 3 | Phase 4 | — |
| **4** | Apr 6 – Apr 14* | Phase 6 | — |

#### Phase Delivery Dates

| Phase | Parallel With | Start | Delivery | Working Days |
| --------------------------------------- | ------------------- | ---------- | ---------- | ------------ |
| **Phase 1** — Harden Middleware & API | — | Mar 4 | Mar 13 | 8 days |
| **Phase 2** — IAM Core Enhancements | Phase 3, Phase 5 | Mar 16 | Mar 25 | 8 days |
| **Phase 3** — Logging & Audit Pipeline | Phase 2, Phase 5 | Mar 16 | Mar 21 | 5 days |
| **Phase 5** — Encryption & Key Mgmt | Phase 2, Phase 3 | Mar 16 | Mar 25 | 8 days |
| **Phase 4** — Advanced Auth (SSO, LDAP, Passkeys, mTLS) | — | Mar 26 | Apr 3 | 7 days |
| **Phase 6** — Admin Console, ABAC, Compliance, Multi-tenancy, Consent | — | Apr 6* | Apr 14* | ~7 days* |

> \* Phase 6 slightly exceeds the 1-month window. It is the largest phase (ABAC engine, full admin dashboard, multi-tenancy model, compliance presets, consent management) and cannot start until Phase 4 delivers.

#### Timeline Visualization

```
Mar  4 ──────────────────────────────────────────── Apr 14
         │
         ├─ Phase 1 ─────────────┤ Mar 13
         │                        │
         │                        ├─ [Track A] Phase 2 ──────────────┤ Mar 25
         │                        ├─ [Track B] Phase 3 ────────┤ Mar 21
         │                        ├─ [Track B] Phase 5 ──────────────┤ Mar 25
         │                                                             │
         │                                                             ├─ Phase 4 ──────────┤ Apr 3
         │                                                                                   │
         │                                                                                   ├─ Phase 6 ──────┤ Apr 14*
```

---

### Technical Impact Estimate

| Aspect               | Impact                                                                                         |
| -------------------- | ---------------------------------------------------------------------------------------------- |
| **Backend**          | ~15-20 new CQRS commands/handlers, 3-4 new services, 5-6 new middleware classes, DB migrations |
| **Frontend**         | ~8-10 new components, updates to existing 11 components                                        |
| **Database**         | ~5-8 new tables (policies, key store, compliance profiles, device credentials, etc.)           |
| **New Dependencies** | Rate limiter (built-in), FIDO2 library, SAML library, CAPTCHA provider                         |
| **Architecture**     | Stays the same — clean architecture + CQRS pattern works well for all of this                  |

---

## Team's Recommendation

Start with Phase 1 (hardening) — it directly addresses the manager's core critique that it's "just frontend" and gives you quick backend wins to demonstrate. Then move through the phases sequentially. Each phase produces a demonstrable deliverable.
