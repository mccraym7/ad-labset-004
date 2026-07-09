# ServiceNow ITSM Lab — Incident, Problem, Change & Service Catalog

> A hands-on IT Service Management (ITSM) build on a free **ServiceNow Personal Developer Instance (PDI)** — modeling how an enterprise IT org intakes, routes, works, and closes IT work in a way that is **consistent, controlled, and auditable**.

![Platform](https://img.shields.io/badge/Platform-ServiceNow_PDI-62D84E?style=flat-square)
![ITIL](https://img.shields.io/badge/Framework-ITIL_4_Foundation-2E6DA4?style=flat-square)
![Certs](https://img.shields.io/badge/Aligned-CompTIA_A%2B_%C2%B7_Network%2B-C0392B?style=flat-square)
![Cost](https://img.shields.io/badge/Cost-%240-2E7D32?style=flat-square)
![Status](https://img.shields.io/badge/Status-Complete-success?style=flat-square)

---

## Overview

When users report IT problems, those problems need to be tracked, routed, prioritized, assigned, worked, and resolved — consistently and auditably. Without a structured system, things fall through the cracks: a server goes down, three people report it to three different team members, nobody knows who owns it, and a one-hour fix takes four.

This lab uses **ServiceNow** — one of the most widely deployed enterprise ITSM platforms in the world — to solve that problem the way real IT organizations do. It enforces process: incidents follow an incident workflow, changes require approval, and service requests come from a catalog with predefined fulfillment steps. Every action is timestamped and attributed.

The full step-by-step build guide (with field-value tables and copy-paste notes) lives in [`docs/ServiceNow_ITSM_Lab_Documentation.docx`](docs/ServiceNow_ITSM_Lab_Documentation.docx).

### Why this matters for a security career

From a security standpoint, ITSM is where **change control**, **access request approval**, and the **CMDB (asset inventory)** live. You cannot govern what is not inventoried, and you cannot prove control without an audit trail. The approval gates built in this lab map directly to controls that auditors test against **SOC 2**, **ISO 27001**, and **NIST 800-53 (CM — Configuration Management)**.

---

## Architecture

End users interact only with the **self-service portal**. Everything else — the four ITSM process pillars, the workflow engine, the CMDB, and reporting — lives inside the ServiceNow platform and is configured by the IT admin.

![image alt](https://github.com/mccraym7/ad-labset-004/blob/8fb07a64888ca700093666688dede618788d99a7/architecture_2.png)

**How to read it:** a request enters from the left (End User → Submit), lands in one of the four process pillars based on its type, and the Workflow & Automation Engine handles routing, approvals, SLA timers, and notifications. The CMDB is the single source of truth for assets; the reporting layer surfaces SLA and volume metrics.

---

## What I Built

| # | Deliverable | Process it demonstrates |
|---|-------------|-------------------------|
| 1 | **Worked incident** — created, triaged, resolved, closed | Core help-desk lifecycle, priority & SLA |
| 2 | **Service catalog item** — New Laptop Request with form variables | Self-service request design |
| 3 | **Change request** — approved with test + backout plans | Change control & approval gates |
| 4 | **Reports + dashboard** — volume, MTTR, open-by-agent | Operational metrics & SLA visibility |

---

## Lab at a Glance

| Field | Value |
|-------|-------|
| **Certification alignment** | CompTIA A+ · Network+ · ITIL 4 Foundation |
| **Platform** | ServiceNow Personal Developer Instance — free, no credit card, no expiration |
| **Time to complete** | 2–3 hours across multiple sessions |
| **Estimated cost** | $0 — the PDI is permanently free |
| **Career relevance** | IT Support · Help Desk · Sysadmin · ITSM Platform Administrator · Security/GRC |

---

## The Four Process Pillars

| Pillar | Purpose | ServiceNow location |
|--------|---------|---------------------|
| 🔴 **Incident** | Restore a broken service as fast as possible (break/fix) | `Service Desk → Incidents` |
| 🟣 **Problem** | Eliminate the root cause behind recurring incidents | `Service Desk → Problems` |
| 🟢 **Change** | Implement a planned change with minimal risk and approval | `Change → Changes` |
| 🔵 **Service Catalog** | Let users request new things (access, hardware) via self-service | `Service Catalog → Catalogs` |

---

## Step 1 — Create & Work an Incident

An **incident** is an unplanned interruption to a service. The objective is to restore service quickly — the single most common task in every IT support role.

![Incident Lifecycle](docs/images/incident-lifecycle.png)

**Scenario used:** a user cannot access Outlook (`Cannot connect to the Exchange server`), one user affected, workaround available (webmail) → **Priority 3 — Moderate**, assigned to the **Service Desk** queue.

The ticket is worked through the full state flow — `New → Assigned → In Progress → Resolved → Closed` — with a clear split between the **work note** (internal, technical, honest) and the **resolution note** (customer-facing, clean). That separation is what keeps an audit trail useful without leaking internal detail to the requester.

---

## Step 2 — Build a Service Catalog Item

A **service request** is a user asking for something new — access, hardware, information — not a break/fix. Catalog items let users self-serve these through a portal, which cuts ticket volume and speeds fulfillment.

Built a **New Laptop Request** item (category: Hardware, fulfillment group: IT Hardware Team) with these form variables:

| Variable | Type | Mandatory |
|----------|------|-----------|
| Requester Name | Single Line Text | Yes |
| Business Justification | Multi Line Text | Yes |
| Required By Date | Date | Yes |
| Laptop Model Preference | Select Box (Standard / Developer / Executive) | No |

---

## Step 3 — Create an Approval Workflow (Change)

A **change** is a planned modification to infrastructure. Change requests require manager/CAB approval before work begins — a core ITIL control that prevents uncoordinated modifications from causing outages.

![image alt](https://github.com/mccraym7/ad-labset-004/blob/30f48840c4ee97a9b6db22ff950a34afa8ad6f36/approval-workflow.png)

**Change used:** deploy security patch **MS24-001** to all Windows workstations (addresses CVE-2024-0001, CVSS 7.8) — Risk: Low, Impact: Medium, with a defined maintenance window, **test plan**, and **backout plan**. Moved through `New (Standard) → Pending Approval → Scheduled`.

> A change with a documented risk rating, maintenance window, test plan, backout plan, and recorded approval is exactly the artifact a security auditor wants to see — it demonstrates the CM control family in action.

---

## Step 4 — Build Reports & a Dashboard

| Report | Type / grouping | Why it matters |
|--------|-----------------|----------------|
| Incident Volume by Priority — Last 30 Days | Bar chart, grouped by Priority | Shows demand shape and severity mix |
| Mean Time to Resolution (MTTR) | Bar chart, grouped by Assignment Group | Core SLA health metric per team |
| Open Incidents by Assigned Agent | Bar chart, grouped by Assigned to | Surfaces workload imbalance |

All three pinned to a single **dashboard** for one-screen operational visibility.

---

## ITIL Concepts Reference

These terms appear in every IT support interview and are tested in ITIL 4 Foundation.

| ITIL Term | Definition | ServiceNow Module |
|-----------|------------|-------------------|
| **Incident** | Unplanned interruption to a service. Goal: restore service fast. | `Service Desk → Incidents` |
| **Problem** | Root cause of one or more incidents. Goal: eliminate it permanently. | `Service Desk → Problems` |
| **Change** | Planned modification to infrastructure/apps. Goal: minimal-risk implementation. | `Change → Changes` |
| **Service Request** | A user request for something new (access, hardware, info). Not break/fix. | `Service Catalog` |
| **SLA** | Committed response & resolution time per priority level. | `SLA → SLA Definitions` |
| **CMDB** | Configuration Management Database — every IT asset and its relationships. | `Configuration → CIs` |
| **Knowledge Base** | Articles documenting known issues & fixes; cuts repeat volume. | `Knowledge → Articles` |

> **The distinction interviewers probe:** An *incident* is "the thing is broken, fix it now." A *problem* is "why does this keep breaking?" A *change* is "we are going to modify something on purpose." An incident may spawn a problem; a problem may be resolved by a change.

---

## Reproduce This Lab

1. Go to **[developer.servicenow.com](https://developer.servicenow.com)** and create a free account (email + password, no credit card).
2. Click **Request Instance** → select the latest stable release → **Request**. Provisioning takes 10–15 minutes.
3. Check your email for the instance URL (`devXXXXX.service-now.com`) and admin credentials.
4. Follow the full build guide in [`docs/ServiceNow_ITSM_Lab_Documentation.docx`](docs/ServiceNow_ITSM_Lab_Documentation.docx).

> ⚠️ **Keep your instance alive.** ServiceNow hibernates a PDI after **10 days** of inactivity and reclaims one after **30 days**. Log in at least once a week, and export/screenshot your deliverables as you go.

---

## Repository Structure

```
.
├── README.md
└── docs/
    ├── ServiceNow_ITSM_Lab_Documentation.docx   # Full step-by-step build guide
    └── images/
        ├── architecture.png                     # Platform architecture diagram
        ├── incident-lifecycle.png               # Incident state flow
        └── approval-workflow.png                # Change approval workflow
```

---

## Skills Demonstrated

`ITSM` · `ITIL 4` · `Incident Management` · `Problem Management` · `Change Management` · `Service Catalog Design` · `Approval Workflows` · `SLA & Reporting` · `CMDB Concepts` · `Change Control` · `IT Support Operations`

---

<sub>Part of an ongoing IT & cloud security portfolio. Lab documentation authored for reproducibility and interview readiness.</sub>
