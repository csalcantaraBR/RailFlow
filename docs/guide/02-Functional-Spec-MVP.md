# `docs/guide/02-Functional-Spec-MVP.md` — Functional Specification (MVP) Template **(Revised)**

**Version:** `<v1.1>`
**Status:** `<Draft | Approved>`
**Owner:** `<Team/Org>`
**Last updated:** `<YYYY-MM-DD>`

> **How to use:** Duplicate this file, fill in the placeholders, and link it in your README. Keep language implementation‑agnostic. Treat every statement as **testable**.

---

## 1) Purpose

Describe the **minimum viable** functional behavior so cross‑functional teams can build, test, and release with confidence. This spec is **technology‑agnostic** and optimized for **TDD-first** delivery.

## 2) Scope & Audience

* **Applies to:** web, mobile, backend services, CLIs, data pipelines, integrations.
* **Audience:** product, engineering, QA, design, security, SRE, analytics.

## 3) Goals & Non‑Goals

* **Goals:** `<measurable outcomes>` (e.g., *reduce task time by 30%*, *support 10k DAU*).
* **Non‑Goals:** `<out of scope items>` (e.g., *no offline mode in MVP*).

## 4) Glossary & Domain Map

Define unambiguous terms and relationships.

* **Actors:** `<EndUser>`, `<Admin>`, `<ExternalSystem>`
* **Core Entities:** `<EntityA>`, `<EntityB>` (key fields, identifiers)
* **Domain Rules (one‑liners):** invariants that always hold true.

> Tip: include a lightweight diagram or table of relationships if helpful.

## 5) Capability Map (what the product must do)

| Capability | Description | Primary Actor(s) | Priority (MoSCoW) | Acceptance (high‑level)         | Telemetry (event names)            |
| ---------- | ----------- | ---------------- | :---------------: | ------------------------------- | ---------------------------------- |
| `<C1>`     | `<what>`    | `<actor>`        |         M         | "<outcome>" observable via test | `c1_started`,`c1_done`,`c1_failed` |
| `<C2>`     |             |                  |         S         |                                 |                                    |

> Each capability below expands into flows + rules.

## 6) Critical Flows (end‑to‑end behavior)

Repeat per flow.

### 6.x `<Flow Name>`

**Narrative:** One paragraph from the user’s perspective.
**Preconditions:** `<state, permissions, data>`
**Happy Path (steps):** enumerated; stop where value is delivered.
**Alternate Paths:** `<A1, A2>`
**Errors & Recovery:** messages, retries, fallbacks.
**Timing & Cutoffs:** timeouts, grace periods, SLAs.
**UX Expectations:** loading/empty/error states; progress visibility.

**BDD examples:**

```
Given <initial state>
When <user/system action>
Then <observable outcome>
And <post-condition or event emitted>
```

## 7) Business Rules Library

Formalize rules so they are testable and unambiguous. Repeat the template.

### Rule `<R-###>` — `<short name>`

* **Intent:** `<why the rule exists>`
* **Inputs/Preconditions:** `<data, role, state>`
* **Computation/Logic:** `<formula/decision table>`
* **Output/Side‑effects:** `<state change, event>`
* **Invariants:** `<must always hold>`
* **Edge Cases:** `<limits, rounding, boundaries>`
* **Examples (BDD):** see flow BDD above.

> Prefer decision tables for complex branching; include boundary values and rounding policies.

## 8) Time‑Bound Behavior

Define all time semantics so tests can emulate them.

* **Timeouts & Retries:** values, retry policy (fixed/exponential), **backoff caps**.
* **Schedules / Windows:** cron rules, blackout windows.
* **Session & Cache Lifetimes:** TTLs, invalidation triggers.
* **Rate Limits & Quotas:** limits per identity, error behavior when exceeded.

## 9) Data Model & Privacy

Summarize data shape without locking to a DB vendor.

* **Identifiers:** `<id scheme>` (ULID/UUID, monotonic if required)
* **Entities & Relationships:** `<ER summary>`
* **Validation:** required fields, ranges, uniqueness, referential rules.
* **Privacy Classification:** P0 (sensitive), P1 (personal), P2 (anonymous).
* **Retention & Deletion:** TTLs, legal holds, right‑to‑erasure.

## 10) Contracts (APIs & Events)

* **External Interfaces:** list REST/GraphQL/gRPC/event topics.
* **Schema Source of Truth:** link to OpenAPI/GraphQL/Protobuf/Zod.
* **Versioning:** semver, deprecation windows, compatibility policy.
* **Idempotency:** keys and expected behavior.
* **Pagination/Sorting/Filtering:** standardized conventions.
* **Error Taxonomy:** codes/messages; mapping to HTTP/gRPC; **user‑safe phrasing** vs **internal details**.

## 11) Feature Flags (if any)

For each flag: **name, owner, scope, default state, rollout plan, metrics, removal date**.
**Testing matrix:** prove critical flows with the flag **ON** and **OFF**.

## 12) Accessibility & Internationalization

* **A11y:** WCAG 2.1 AA baseline: focus order, keyboard operability, contrast, ARIA labels.
* **i18n:** locales, date/number/currency formats, RTL support, copy length constraints.

## 13) Non‑Functional Requirements (NFRs)

Define budgets and SLOs the MVP must meet.

* **Performance:** e.g., p95 API latency `<N> ms`; Web TTI `<N> s`; animations 60 fps.
* **Reliability:** availability SLO, error budgets, degradation modes.
* **Security:** authn/authz model; data at rest/in transit; threat considerations.
* **Compliance:** GDPR/CCPA/PCI/etc. if applicable.
* **Scalability & Capacity:** expected peak; vertical/horizontal strategies.
* **Offline/Resilience (if applicable):** retry, queueing, local cache.

## 14) Telemetry Specification

Define events the system must emit — these are part of acceptance.

| Event        | When it fires | Properties (PII tags) | Sampling | Alerts   |               |
| ------------ | ------------- | --------------------- | -------- | -------- | ------------- |
| `<evt_name>` | `<trigger>`   | \`\<prop\:a           | b>\`     | `<rate>` | `<threshold>` |

> Mark properties with PII tags (P0/P1/P2) and redaction rules.

## 15) Acceptance Criteria (per capability)

* **Test‑First:** a failing test exists for each rule and flow before implementation.
* **Coverage:** meets `<threshold>` (overall and key modules).
* **Contracts:** verified against latest schemas; no breaking changes.
* **Telemetry:** specified events implemented and visible in dashboards.
* **UX States:** happy/empty/loading/error are all testable.
* **Flags:** critical flows proven **ON** and **OFF**.
* **A11y/Perf Budgets:** within agreed limits.

## 16) Traceability Matrix (link rules ↔ tests)

| Rule/Flow | Test ID   | Layer (Unit/Contract/Int/E2E) | Evidence (link)      |
| --------- | --------- | ----------------------------- | -------------------- |
| `<R-###>` | `<T-###>` | `<layer>`                     | `<PR/commit/report>` |

## 17) Open Questions & Assumptions

* **Assumptions:** `<list>`
* **Open Questions:** `<list>` with owners and due dates.

## 18) Customization Checklist

* [ ] Goals & Non‑Goals are measurable and agreed
* [ ] Actors, Entities, and Rules defined and unambiguous
* [ ] Flows documented with BDD examples
* [ ] Timeouts/retries/schedules specified
* [ ] Data model constraints & privacy classes set
* [ ] Contracts & versioning policy linked
* [ ] Feature flags with lifecycle & tests
* [ ] NFR budgets and SLOs
* [ ] Telemetry spec with PII tags and alerts
* [ ] Acceptance criteria & RTM completed

> **Process hooks:** When teams implement this spec under TDD, keep the test runner in **watch mode** and pair with your AI assistant (e.g., Cursor) to re‑run on every edit automatically.
