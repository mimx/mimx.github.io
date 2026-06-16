---
layout: post
title: "IGTS - Identity Governance Tagging Standard"
date: 2026-06-16
categories: [concepts]
tags: [iga, identity-governance, igts, sod, compliance, metadata, standard, tagging]
excerpt: "A protocol for embedding machine-readable governance metadata directly into identity objects at the source — shifting classification from after-the-fact inference to owner-attested declaration."
---

# IGTS — Identity Governance Tagging Standard
### A Protocol for Machine-Readable Governance at the Source

**Author:** Mohamed Mahmoud  
**Date:** 2026-06-16  
**Domain:** IAM Architecture / Identity Governance  
**Status:** Draft

---

## The Problem This Solves

Every IGA programme eventually runs into the same wall.

If you want the foundation first, read [IGA Compliance, SoD, and the Role Metadata Model](https://mimx.github.io/2026/06/16/iga-compliance-sod-deep-dive/). This article starts where that one ends: how to make governance metadata available at the source.

You connect 500 applications to your governance platform. Each one has roles, groups, entitlements, permission sets. You need to classify every object: Is it SOX-critical? How sensitive is it? What business function does it represent? Does granting it alongside something else create a Segregation of Duties conflict?

The honest answer to "how do we classify all of this?" has always been: *slowly, manually, expensively, and incompletely.* The metadata drifts. New entitlements appear and go unclassified. Auditors find gaps. The cycle repeats.

LLMs improved the situation — you can now infer classification from entitlement names and descriptions at scale. But inference is still inference. An LLM reading `role-12847` in some internal tool has nothing to work with. Even reading `FI-AP-INVOICE-POST` produces 0.75 confidence, not 0.98. And confidence matters when SOX audit evidence is on the line.

The root cause is structural: **governance metadata is defined outside the identity objects, after the fact, by people who didn't create them.** The system owner who created the AD group or SAP role has the knowledge. It never gets captured.

IGTS fixes this by moving the classification to the source — embedded in the object itself, by the person who created it, before it goes live.

---

## What IGTS Is

**IGTS (Identity Governance Tagging Standard)** is a protocol that requires every system owner to embed a structured, machine-readable governance tag directly into any privileged access object they create — an AD group, Okta group, SAP role, AWS IAM role, Salesforce permission set, GitHub team, or any other identity object.

The tag is a `/`-separated string with fixed segments and a controlled vocabulary. It is placed in a designated attribute of the identity object at the source system. IGA reads it on import. LLMs use it as ground truth.

---

## The Tag Format

```
igts/v1/<domain>/<process>/<sensitivity>/<sox>/<function>/<data-class>/<lifecycle>
```

The `igts` prefix identifies the protocol. The remaining eight slash-separated fields define the v1 tag. All fields are lowercase. Fixed vocabulary per field. No wildcards. No optional fields — every field must be explicitly defined.

The `v1` version field makes the format versioned from day one. When the standard evolves, parsers detect the version and handle tags written against older formats without breaking.

---

## Segment Definitions

### Segment 1: `v1`

Format version. Always `v1` for tags written against this specification. Parsers use this to select the right segment schema.

---

### Segment 2: `domain`

The business domain the system serves.

```
finance | hr | legal | tech | ops | security | sales | marketing | data | platform | external
```

Domain immediately signals regulatory scope. `domain=finance` → SOX at minimum Medium. `domain=hr` → GDPR / PII relevance. `domain=external` → partner/contractor access patterns.

---

### Segment 3: `process`

The business control process within the domain.

```
accounts-payable | accounts-receivable | vendor-master | general-ledger | payroll | treasury |
procurement | revenue | reporting | recruiting | offboarding | identity-admin |
deployment | incident | data-access | audit | config-management | user-lifecycle
```

**This is the key addition for SoD precision.** Domain alone is too broad. Without process, a GL posting and a vendor master approval are both `finance`, which produces false SoD positives. With process, the conflict engine evaluates at the correct granularity:

```
finance/accounts-payable/post + finance/accounts-payable/approve → likely SoD conflict
finance/general-ledger/post  + finance/vendor-master/approve    → not the same control process
```

If the process does not fit an existing value, use a short kebab-case slug. The goal is to scope the business control process, not enumerate every possible process in advance.

---

### Segment 4: `sensitivity`

The IT/security sensitivity tier of the access object.

```
critical | high | medium | low | public
```

Maps directly to **Sys Criticality** in Saviynt:

| Tag value | Sys Criticality | Typical examples |
|-----------|----------------|-----------------|
| `critical` | Critical | Production financial systems, security infrastructure, admin-level access |
| `high` | High | Internal systems with sensitive data, production write access |
| `medium` | Medium | Standard internal tools, non-sensitive data |
| `low` | Low | Read-only, reporting, low-impact access |
| `public` | None | Publicly accessible resources |

---

### Segment 5: `sox`

Explicit SOX audit scope declaration.

```
inscope | outofscope
```

The system owner declares whether this access object is in SOX audit scope. If uncertain, the protocol defaults to `inscope`. This is an owner-attested claim — see the **Tag Claims and Validated State** section for how compliance can confirm, dispute, or override it.

`inscope` → SOX Criticality minimum Medium, reviewed quarterly, stricter SoD rules.
`outofscope` → SOX Criticality none/low unless overridden by domain signals or compliance validation.

---

### Segment 6: `function`

The business action this access object enables. This is the most critical segment — it maps directly to SoD Functions in the IGA platform.

```
create | approve | post | release | change | delete | read | admin | audit | configure | execute | transfer
```

**This segment maps directly to SoD Functions in the IGA platform**, scoped by domain and process. IGTS makes SoD governance computable by shifting the work from manually mapping thousands of entitlements to maintaining a smaller governed conflict matrix across domains, processes, and functions.

The conflict logic evaluates at `domain + process + function`:

> Two entitlements in the same `domain` and `process` where one has `function` in `{create, post, transfer, change}` and the other has `function` in `{approve, release}` are a SoD conflict candidate.

The IGA platform still maintains a governed conflict matrix — but it operates against a small, stable vocabulary rather than thousands of raw entitlement names. That is the shift IGTS enables.

---

### Segment 7: `data-class`

The classification of data this access object touches.

```
pii | financial | confidential | internal | public | restricted | health | legal
```

Feeds the `Confidentiality` field in Saviynt and determines regulatory framework applicability:

- `pii` → GDPR relevant, annual certification minimum
- `financial` → SOX relevant, confirms inscope signal
- `health` → HIPAA relevant
- `restricted` → highest certification frequency, most restrictive SoD rules

---

### Segment 8: `lifecycle`

The expected **grant assignment pattern** for this access object — not the lifetime of the object itself.

```
permanent | temporary | emergency | seasonal
```

An emergency access group may permanently exist as an object, but individual membership should expire after 24 hours. A production deployment role may be a permanent object, while grants to users should be temporary. The tag captures the intended grant pattern, not whether the group or role itself is long-lived.

- `permanent` → standard approval, annual certification
- `temporary` → auto-expire after configured period (default 90 days), manager re-approval required
- `emergency` → time-limited (default 24h), mandatory mitigating control, full audit trail
- `seasonal` → recurs on defined schedule (e.g. year-end close access)

---

## Complete Tag Examples

### SAP — Accounts Payable Invoice Posting

```
igts/v1/finance/accounts-payable/critical/inscope/post/financial/permanent
```

- SOX: High (finance + inscope)
- Sys Critical: Critical
- SoD: post in finance/accounts-payable
- Conflicts with `igts/v1/finance/accounts-payable/*/inscope/approve/*/`  — same domain + process, opposing functions

---

### AD Group — Okta Admin Access

```
igts/v1/security/identity-admin/critical/outofscope/admin/internal/permanent
```

- SOX: Not in scope
- Sys Critical: Critical
- High-privilege admin — quarterly certification, strict approval chain

---

### Okta Group — Read-Only HR Data

```
igts/v1/hr/user-lifecycle/medium/outofscope/read/pii/permanent
```

- SOX: No
- Sys Critical: Medium
- PII access → GDPR relevant, annual certification minimum
- `read` function — no SoD conflict risk with other read-only access in same process

---

### AWS IAM Role — Production Deployment

```
igts/v1/tech/deployment/high/outofscope/execute/internal/temporary
```

- Sys Critical: High
- `lifecycle=temporary` → grant assignment expires, requires re-approval; the role object itself is permanent
- Execute function — conflicts with approve in same domain/process if present

---

### SAP Role — Approve Vendor Payments

```
igts/v1/finance/accounts-payable/critical/inscope/approve/financial/permanent
```

- SOX: High
- Conflicts with `igts/v1/finance/accounts-payable/*/inscope/post/*/` or `.../create/*/` — same process, opposing functions
- Does NOT automatically conflict with `igts/v1/finance/general-ledger/*/inscope/post/*/` — different process

---

### Salesforce — Delete Customer Records

```
igts/v1/sales/data-access/high/outofscope/delete/pii/emergency
```

- Sys Critical: High
- PII data + delete function → highest sensitivity
- `lifecycle=emergency` → individual grants expire after 24h; mandatory mitigating control

---

### GitHub Team — Infrastructure Admin

```
igts/v1/tech/config-management/critical/outofscope/admin/internal/permanent
```

- Sys Critical: Critical
- Admin function in config-management → quarterly certification
- Not SOX scope but high blast radius

---

## Where Each System Embeds the Tag

Storage pattern depends on the source system. In free-text attributes, store the full portable string: `igts/v1/...`. In systems that already use `igts` as the tag key or annotation key, the value starts at `v1/...`.

| System | Recommended attribute | Notes |
|--------|----------------------|-------|
| **Active Directory** | `description` on group, or `extensionAttribute1` | `description` is readable by all LDAP consumers |
| **Azure AD / Entra** | `description` or `extensionAttribute` on group | Syncs to on-prem AD if hybrid |
| **Okta** | Group `description` | Visible in Okta admin and API |
| **SAP** | Role long description or classification text field | T-code PFCG, description tab |
| **AWS IAM** | Tag on role: `Key=igts`, `Value=v1/tech/deployment/high/outofscope/execute/internal/temporary` | Native AWS tagging — searchable via Resource Explorer |
| **Salesforce** | Permission Set `Description` field | API-accessible via Tooling API |
| **GitHub** | Team `description` | Readable via GitHub API |
| **Workday** | Security group `description` | Importable into Saviynt on sync |
| **Google Workspace** | Group `description` | Accessible via Admin SDK |
| **ServiceNow** | Role `description` or custom field | |
| **Kubernetes** | RBAC ClusterRole/Role `annotations`: `igts: v1/tech/config-management/high/outofscope/admin/internal/permanent` | Native annotation pattern |

---

## How IGA Processes the Tag

### On Import

```
System connector imports identity object
    ↓
IGA parser reads designated attribute
    ↓
Validates tag format (v1: 8 segments, controlled vocabulary)
    ↓
Valid tag?
    ├── Yes → parse segments, auto-populate:
    │         soxcritical     ← from sox + domain segments
    │         syscritical     ← from sensitivity segment
    │         confidentiality ← from data-class segment
    │         sod_function    ← from domain + process + function segments
    │         lifecycle       ← from lifecycle segment (grant assignment pattern)
    │
    │         set provenance fields:
    │         source_tag              = raw igts string
    │         classification_source   = "igts-tag"
    │         classification_status   = "confirmed"  (owner-declared, pending validation)
    │         validation_status       = "needs_review"  ← IGA/compliance must confirm
    │         classification_confidence = 0.98
    │
    └── No / Missing → object flagged needs_review
                       default to highest risk: igts/v1/unknown/unknown/high/inscope/admin/confidential/permanent
                       owner notified to add tag within 30 days
```

Note that `classification_status = confirmed` means the tag is syntactically valid and parseable. `validation_status = needs_review` means compliance has not yet confirmed the owner's claims — see the next section.

### Tag Claims and Validated Governance State

The IGTS tag is an **owner-attested claim**, not automatically final governance truth. A system owner can declare `sox=outofscope`, but SOX scope must ultimately be confirmed against the application inventory, audit scope, and compliance decisions.

IGA treats the tag as high-quality source evidence. Compliance validates it — confirming, disputing, or overriding as needed.

The validation model alongside every IGTS-classified object:

```yaml
source_tag:        igts/v1/finance/accounts-payable/critical/inscope/post/financial/permanent
validation_status: confirmed | disputed | overridden | expired | needs_review
validated_by:      iam | compliance | audit | automated-policy | app-owner
validated_at:      2026-06-16T10:00:00Z
override_reason:   (optional — required when validation_status = overridden)
```

This distinction matters most for SOX scope. An owner who declares `outofscope` for a financial system should be challenged. Compliance can set `validation_status = disputed` and trigger a review without touching the tag itself. If the tag is confirmed wrong, `validation_status = overridden` with an override reason creates the audit trail.

The stronger framing: **IGTS provides high-quality source evidence. The IGA platform turns that evidence into validated governance state.** Both are necessary. Neither replaces the other.

For a v1 pilot, this model can be kept simple — a manual compliance review of all `sox=outofscope` tags in high-risk domains is enough to start. Full automated validation is a later maturity step.

---

### LLM Role with IGTS

With tags in place, the LLM's job transforms from inference to interpretation and cross-validation:

**Without IGTS:**
> Input: entitlement name `FI-AP-INVOICE-POST`
> LLM infers: "probably finance, probably SOX, probably AP posting" → confidence 0.72

**With IGTS:**
> Input: name `FI-AP-INVOICE-POST` + tag `igts/v1/finance/accounts-payable/critical/inscope/post/financial/permanent`
> LLM confirms tag is consistent with name and context → confidence 0.98
> LLM checks: user already has `igts/v1/finance/accounts-payable/critical/inscope/approve/financial/permanent`
> → same domain + same process + post vs approve → SoD conflict flagged
> LLM produces audit explanation: *"Tag attested by system owner. Post + Approve in finance/accounts-payable is a standard AP SoD conflict. Validation status: needs_review — compliance confirmation pending."*

With IGTS, the LLM's primary role shifts from classifier to **reviewer and inconsistency detector**:
- Compare tag against object name and description — flag suspicious under-classification
- Detect when `sensitivity=low` appears on a system called `PAYMENT-APPROVAL-PROD`
- Explain SoD conflicts in plain business language for certifiers
- Assist legacy backfill where no tag exists

Confidence climbs from 0.60–0.85 (inference) to 0.95–0.99 (tag-grounded validation).

### SoD Conflict Detection with Process Granularity

IGTS makes SoD governance computable. The conflict matrix operates against a small, stable vocabulary rather than thousands of raw entitlement names:

```python
# Pseudo-logic — illustrates how domain + process + function reduce conflict matrix size
def is_sod_conflict(tag_a, tag_b):
    a = parse_igts(tag_a)  # version, domain, process, sensitivity, sox, function, data_class, lifecycle
    b = parse_igts(tag_b)

    # Different domain → no conflict
    if a.domain != b.domain:
        return False

    # Different process → not the same control process, no automatic conflict
    if a.process != b.process:
        return False

    create_functions = {"create", "post", "transfer", "change"}
    approve_functions = {"approve", "release", "authorize"}

    if a.function in create_functions and b.function in approve_functions:
        return True
    if b.function in create_functions and a.function in approve_functions:
        return True

    if "admin" in {a.function, b.function}:
        return True  # admin + anything in same domain/process = flag for review

    return False
```

The IGA platform still maintains a governed conflict matrix — but it works across a controlled vocabulary of domains, processes, and functions, not thousands of entitlement names. That is the shift IGTS enables: the configuration work shrinks from mapping entitlements to maintaining a policy.

---

## The Governance Protocol

IGTS only works if it is mandated and enforced. The protocol has six rules:

### Rule 1: Tag Before Activation

No privileged group, role, or entitlement goes live without a valid IGTS tag. The IGA import job validates format on import. Invalid or missing tag → object flagged `needs_review`, excluded from SoD evaluation, and owner notified.

### Rule 2: Tag Accuracy is the Owner's Responsibility

The system owner attests the tag is correct. In certification campaigns, the first question to the owner is: *"Confirm the IGTS tag on this entitlement is accurate."* Incorrect tags are a governance finding, not just a data quality issue.

### Rule 3: Tag Changes Require Approval

Changing a tag from `outofscope` to `inscope`, or from `medium` to `critical`, is a governance event requiring a change ticket and compliance team approval. Prevents quiet downgrading of sensitive access classifications.

### Rule 4: No Wildcards

Every segment must be explicitly defined. `igts/v1/finance/accounts-payable/*/inscope/*/financial/permanent` is invalid. Forces the owner to commit to a specific process, sensitivity, and function.

### Rule 5: Legacy Grace Period, Then Conservative Default

Existing objects without a tag get 90 days to be tagged. After that: `igts/v1/unknown/unknown/high/inscope/admin/confidential/permanent` — the most conservative possible default. This forces owners to correct it or accept the most restrictive governance treatment.

### Rule 6: Tag Validation Tool Is Mandatory

Every system owner must have access to a simple tag validator before they can submit a new object. The validator confirms:
- Format is correct (`v1`: 8 segments)
- Each segment value is in the controlled vocabulary
- Policy consistency checks: `data-class=financial` + `sox=outofscope` triggers a justification prompt; `function=admin` with `sensitivity=low` is rejected
- Returns: ✅ valid, or ❌ "fix segment 4 — valid values: create | approve | post | ..."

The validator is a 2-hour build. Its existence removes the "I didn't know the format" excuse.

---

## What This Changes

| Problem | Without IGTS | With IGTS |
|---------|-------------|-----------|
| SOX classification | LLM infers from name (0.70 confidence) | Tag explicit (0.98 confidence) |
| Sys criticality | Manual per entitlement or inherited from system | From `sensitivity` segment, set at source |
| SoD function mapping | Manual — IGA admin maps each entitlement to a function | `function` segment IS the SoD function |
| Cross-system SoD | Nearly impossible at scale | `domain/process/function` combination spans systems with precision |
| New entitlement onboarding | Days of manual classification | Minutes — owner writes the tag |
| Audit evidence | "We believe this is SOX critical" | "Tag attested by system owner on [date], confirmed by IGA import" |
| LLM classification confidence | 0.60–0.85 on inference | 0.95–0.99 on tag parsing |
| Data drift | Continuous problem — new entitlements go unclassified | Structural prevention — untagged = blocked |
| Certification quality | Certifiers review without context | Certifiers see tag, understand what they're reviewing |

---

## The Honest Challenge

**System owners won't do this voluntarily.** Nobody wakes up wanting to write a governance tag. The protocol only works if:

1. **IGA blocks untagged objects** — no tag, no activation. This is the enforcement mechanism.
2. **The validator is frictionless** — copy-paste examples for common cases, a dropdown-based builder for first-timers.
3. **Leadership mandates it** — written into the system onboarding checklist, not optional.
4. **There's a visible benefit** — system owners who tag correctly spend less time in certification campaigns because reviewers understand their access. That's the incentive.

The tagging tool is easy to build. The governance protocol is the hard part. But once it's established, the IGA data quality problem — the problem that has plagued every IGA programme for 20 years — becomes **structurally solved** rather than perpetually managed.

---

## Extending the Standard

The base IGTS v1 tag covers the most common governance dimensions and is deliberately kept simple for pilot adoption. Optional extensions can be appended as semicolon-separated key-value pairs, keeping the core tag intact and parseable:

```
igts/v1/<domain>/<process>/<sensitivity>/<sox>/<function>/<data-class>/<lifecycle>;key=value;key=value
```

### Regulation Extension

For systems subject to specific regulations beyond SOX:

```
igts/v1/hr/user-lifecycle/high/outofscope/read/health/permanent;reg=hipaa
```

### Cross-System Link

For entitlements that are part of a known cross-system SoD pair — explicitly linking the SAP create function to its approval counterpart in another system:

```
igts/v1/finance/accounts-payable/critical/inscope/create/financial/permanent;xsys=coupa-vendor-approval
```

### Owner Attribution

```
igts/v1/finance/accounts-payable/critical/inscope/post/financial/permanent;owner=team-finance-ops
```

These extensions are optional and not required for a v1 pilot. The core tag stands alone. Extensions add richness when the governance programme matures.

---

## Relationship to Existing IGA Metadata

IGTS does not replace existing IGA metadata fields. It populates them at source with higher confidence. In Saviynt, for example:

| IGTS segment | Saviynt field populated |
|-------------|------------------------|
| `sox` + `domain` | `soxcritical` |
| `sensitivity` | `syscritical` |
| `data-class` | `confidentiality` |
| `domain` + `process` + `function` | SoD Function assignment (process granularity prevents false positives) |
| `lifecycle` | Approval workflow selection, certification frequency (grant assignment pattern) |
| full tag | `risk` score (composite) |
| provenance | `source_tag`, `classification_source`, `validation_status` |

The tag is the input. The IGA platform's governance mechanisms are the output. Compliance validation turns owner-declared claims into confirmed governance state.

---

## Maturity Roadmap

IGTS is designed to start small and grow. A v1 pilot requires only three things:

1. **A source-side tag** — system owners write the tag into the designated attribute
2. **A validator** — a simple tool that checks format and vocabulary (2-hour build)
3. **IGA parsing** — the import layer reads and maps the tag to governance fields

That is the complete v1 pilot. No registry. No evidence store. No multi-platform enforcement automation. Prove the value with one or two systems first.

As the programme matures:

| Phase | Addition |
|-------|---------|
| **v1 — Pilot** | Tag + validator + IGA import parsing on 1–2 systems |
| **v2 — Expand** | Controlled vocabulary registry, compliance validation workflow, backfill of high-risk legacy objects |
| **v3 — Enforce** | Source-system enforcement gates (ServiceNow catalog, Terraform policy checks, AD provisioning automation) |
| **v4 — Audit-grade** | Evidence store (tag history, validation audit trail), LLM inconsistency detection, cross-system SoD automation |

The architecture described in this article is the end state. The pilot is deliberately lean.

---

## Summary

IGTS is a simple, enforceable naming convention that embeds governance intelligence directly into identity objects at the point of creation. The full portable string is the `igts` protocol prefix plus eight v1 fields: version, domain, process, sensitivity, SOX scope, function, data classification, and lifecycle.

It does not require AI to work. It makes AI dramatically more effective when applied. It does not require a new platform. It works with whatever IGA system you have. It does not require a big-bang migration. You start with new objects, backfill over time. A v1 pilot needs only a tag, a validator, and IGA parsing.

The tag is an owner-declared claim. Compliance validates it. Together they produce audit-grade governance state without a manual classification programme.

The tag is small. The protocol is straightforward. The impact — on classification confidence, SoD precision, audit evidence quality, and LLM-assisted governance — is structural.

```
igts/v1/finance/accounts-payable/critical/inscope/post/financial/permanent
```

Protocol prefix plus eight fields. Everything IGA needs to govern it correctly. Written by the person who knows what it does.
