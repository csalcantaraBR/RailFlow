# `docs/guide/03-Project-Brief-and-Standards.md` — Project Brief & Engineering Standards (Template)

**Version:** `<v1.0>`
**Status:** `<Draft | Approved>`
**Owner:** `<Team/Org>`
**Last updated:** `<YYYY-MM-DD>`

> **How to use:** Duplicate this file into your repo, fill the placeholders, and link it in your README. Keep it short, opinionated, and **technology‑agnostic**. Everything here is intended to be **testable and enforceable** (via CI/CD).

---

## 1) Project Brief

* **Project Name:** `<ProjectName>`
* **Problem Statement (1–2 paragraphs):** Describe the business/user problem and why it matters now.
* **Vision (1 paragraph):** What the product will enable when successful.
* **Success Criteria (measurable):** `<KPI_1>`, `<KPI_2>`, `<KPI_3>` with target dates.
* **Non‑Goals / Out of Scope:** Explicitly list what is excluded in this phase.
* **Primary Users & Stakeholders:** Product, Engineering, QA, Design, Security, Operations, Data/Analytics.
* **Assumptions & Constraints:** `<budget>`, `<timeline>`, `<tech>` (e.g., must run on `<cloud>`), compliance.
* **Top Risks (with mitigations):** 3–5 bullets linking to issues/ADRs.

### 1.1 KPIs & Guardrails

| KPI                 |   Baseline |      Target | Owner     | Measurement Source  |
| ------------------- | ---------: | ----------: | --------- | ------------------- |
| `<Adoption/DAU>`    |      `<N>` |       `<N>` | `<Owner>` | Analytics Dashboard |
| `<Reliability/SLO>` |  `<99.5%>` |   `<99.9%>` | `<Owner>` | SLO Board           |
| `<Time‑to‑Change>`  | `<X days>` | `<Y hours>` | `<Owner>` | DORA Metrics        |

---

## 2) Operating Model

* **Team Topology:** `<Stream‑aligned | Platform | Enabling | Complicated‑Subsystem>`; list owners per domain/component.
* **RACI / Ownership:** Provide a simple RACI or ownership table per area.
* **Cadence:** sprint length, planning, demos, retro, incident reviews.
* **Decision Records:** All architectural decisions captured as **ADRs** in `docs/adr/` (short, dated, reversible).
* **Ways of Working:** Async‑first updates, PR reviews within `<24–48h>`, pair/mob for complex changes.

### 2.1 RACI (example)

| Area     | R            | A            | C              | I                |
| -------- | ------------ | ------------ | -------------- | ---------------- |
| Core API | `<TechLead>` | `<HeadEng>`  | `<QA, SRE>`    | `<Stakeholders>` |
| Frontend | `<FE Lead>`  | `<TechLead>` | `<Design, QA>` | `<Support>`      |

---

## 3) Technology Baseline

* **Languages & Frameworks:** `<Lang>/<Framework>`; LTS versions; minimal supported version matrix.
* **Repo Model:** `<Monorepo | Multi‑repo>`; naming conventions; ownership per package/service.
* **Package & Build:** `<pm>`, lockfiles required; reproducible builds; artifact retention policy.
* **Environments:** **dev → stage → prod** with promotion strategy and change management.
* **Configuration:** 12‑factor style; config via environment; no secrets in repo.
* **Secrets Management:** `<Vault/KMS/Secret Manager>`; rotation policy; least privilege.
* **Data Stores:** `<DB(s)>` with backup/restore RPO/RTO; migration strategy (e.g., `<Flyway/Prisma/DBT>`).
* **Third‑party / Integrations:** inventory with SLAs; sandbox vs production separation.

---

## 4) Engineering Standards

### 4.1 Code Quality

* **Style & Linters:** Enforce via CI (e.g., ESLint/Detekt/Flake8), auto‑format (Prettier/Black/go fmt).
* **Reviews:** Minimum `<1–2>` approvers; small PRs; block on requested changes.
* **Docs in Code:** Public APIs/types documented; high‑level diagrams in README/`/docs`.

### 4.2 Testing (TDD‑first, with watch mode)

* **Approach:** Tests precede code; keep the runner in **watch** to maintain fast loops.
* **Layers:** Unit, **Contract** (boundaries), Integration, E2E, plus Non‑functional (perf/a11y/visual).
* **Coverage Thresholds:** default **≥80%** overall and per key package (set your value below).
* **Flakiness Budget:** ≤ **2%** per job; quarantine & track.

### 4.3 Accessibility & Performance

* **A11y:** WCAG 2.1 AA; keyboard/focus order; contrast; screen‑reader labels.
* **Perf Budgets:** Set per app type (API p95 latency, Web TTI/INP, Animations 60fps).

### 4.4 Security & Supply Chain

* **SAST & Dependency Scans:** run in CI; no High/Critical vulns unless waived via ADR with expiry.
* **Secrets:** scanner in CI; pre‑commit hooks; zero secrets in code or logs.
* **SBOM & Provenance:** generate SBOM (e.g., CycloneDX); sign artifacts if required.

### 4.5 Observability & Reliability

* **Logs:** structured; PII redaction; correlation IDs.
* **Metrics:** latency, throughput, error rate; business KPIs.
* **Traces:** instrument critical paths.
* **SLOs & Error Budgets:** define per service; alert on burn rate; rollback policy documented.
* **Resilience:** timeouts, retries (with backoff caps), circuit breakers, fallbacks; feature flags & kill‑switches.

### 4.6 Data Management

* **Schema Changes:** backward‑compatible by default; migrations reviewed and reversible.
* **Backups & DR:** RPO `<X>` / RTO `<Y>` documented and tested.
* **Privacy:** data classification (P0/P1/P2); retention & deletion; auditability requirements.

---

## 5) Branching, Releases & Versioning

* **Branching Model:** `<Trunk‑based | GitFlow>` (state choice + rationale).
* **Commit Convention:** Conventional Commits + TDD tags (`test(red)`, `feat(green)`, `refactor`).
* **PR Hygiene:** small diffs, linked issues, TDD evidence.
* **Releases:** semantic versioning; release notes with breaking changes; tags signed when applicable.
* **Deployment Strategy:** `<blue‑green | canary | rolling>`; automated rollback on SLO breach.

---

## 6) CI/CD Quality Gates (Minimum Policy)

**Block merge if any fails.**

1. **Tests:** all layers configured for the repo; **watch** during dev, **CI mode** on PR.
2. **Coverage:** ≥ `<threshold>` overall and per key package.
3. **Typecheck/Lint:** zero errors.
4. **Contracts:** consumer/provider verification; fail on breaks.
5. **Security:** dependency scan, secret scan; SBOM generated.
6. **Budgets:** a11y/perf (where applicable) within limits.
7. **Artifacts:** build outputs + test reports uploaded.

### 6.1 Example CI Skeleton (illustrative)

```yaml
name: quality
on: [pull_request]
jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: <setup toolchain>
      - run: <install deps with lockfile>
      - run: <typecheck> && <lint>
      - run: <test:ci> && <coverage:ci --min=${{ vars.COVERAGE_MIN || 80 }}>
      - run: <contracts:verify>
      - run: <security:scan && sbom:generate>
      - run: <upload artifacts/reports>
```

---

## 7) Required Documentation (per repo)

* `README.md`: quickstart, architecture sketch, CI commands, links to dashboards.
* `.env.example`: non‑secret defaults; link to secret manager for real values.
* `docs/adr/`: ADR index and short decisions.
* `docs/guide/`: this document and other core artifacts.
* `SECURITY.md`: vulnerability disclosure & triage.
* `CODE_OF_CONDUCT.md` (if applicable).

---

## 8) Definitions of Ready & Done

**Definition of Ready (DoR)**

* Acceptance criteria and **test strategy** documented.
* Telemetry & flag plan specified.
* Contracts identified (if any) and versioning approach agreed.

**Definition of Done (DoD)**

* TDD loop **RED → GREEN → REFACTOR** demonstrated; tests run in **watch** during dev.
* All CI gates green; coverage threshold met.
* Contracts verified; NFR budgets respected.
* ADRs updated (mocks, flags, or major decisions).
* Docs & release notes updated.

---

## 9) Customization Matrix (fill per project)

| Area                | Default                            | Your Value |
| ------------------- | ---------------------------------- | ---------- |
| Coverage threshold  | 80% overall & per key pkg          |            |
| Branching model     | Trunk‑based                        |            |
| Perf budgets        | API p95 `<300ms>` / Web TTI `<2s>` |            |
| A11y baseline       | WCAG 2.1 AA                        |            |
| Flakiness budget    | ≤ 2% per job                       |            |
| DR targets          | RPO `<24h>` / RTO `<4h>`           |            |
| Flag removal window | ≤ 2 sprints                        |            |
| Mock expiry window  | ≤ 1 sprint                         |            |

---

## 10) Templates (copy‑paste)

**10.1 ADR (short)**

```
# ADR-<ID>: <Decision>
Date: <YYYY-MM-DD>
Context:
Decision:
Consequences:
Rollback Plan:
Expiry (if mock/flag):
```

**10.2 PR Template (quality)**

```
## What changes
-

## TDD Evidence
- RED → GREEN → REFACTOR (links to commits/tests)

## Quality
- [ ] Tests pass     - [ ] Coverage >= <threshold>  - [ ] Typecheck  - [ ] Lint
- [ ] A11y budget    - [ ] Perf budget              - [ ] Contracts verified
- [ ] Watch mode used during TDD (paired with Cursor)

## Risks & Mitigations
-
```

---

## Appendix — Example Defaults by Project Type (illustrative)

| Type          | Coverage | Perf Budget            | Contract Tests      | Release Strategy        |
| ------------- | -------- | ---------------------- | ------------------- | ----------------------- |
| API Service   | 85%      | p95 < 250ms            | Required            | Canary + auto‑rollback  |
| Web App       | 80%      | TTI < 2s / INP < 200ms | Recommended         | Rolling + feature flags |
| Data Pipeline | 75%      | SLA < X min per batch  | Required on schemas | Scheduled + backfills   |

> Keep this file **living**: update when standards evolve. Short over comprehensive; link to deeper docs when necessary.
