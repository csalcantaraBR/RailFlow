# `docs/guide/05-Status-Report.md` — Status Report Template (Reusable)

**Version:** `<v1.0>`
**Status:** `<Draft | Approved>`
**Owner:** `<Team/Org>`
**Period Covered:** `<YYYY-MM-DD → YYYY-MM-DD>`
**Last updated:** `<YYYY-MM-DD>`

> **How to use:** Duplicate this file per reporting period. Keep it short but **evidence‑backed**. Everything should link to PRs, dashboards, or artifacts. Prefer **facts & links** over prose. Sections are modular—delete what doesn’t apply.

---

## 1) Executive Summary (≤ 8 bullets)

* **Overall RAG:** `<Green | Amber | Red>` — brief reason.
* **Highlights:** 2–3 concrete outcomes delivered.
* **Risks/Issues:** top 2 with mitigation status.
* **On‑track?** `<Yes/No>` for scope, schedule, quality.
* **Next period:** top 3 priorities (outcomes, not tasks).

### 1.1 Health Snapshot (RAG)

| Area            | Status    | Why                                    | Owner    |
| --------------- | --------- | -------------------------------------- | -------- |
| Scope           | `<G/A/R>` | `<delta vs plan>`                      | `<name>` |
| Schedule        | `<G/A/R>` | `<milestone slips or ahead>`           | `<name>` |
| Quality         | `<G/A/R>` | `<tests/coverage/contracts/a11y/perf>` | `<name>` |
| Risk            | `<G/A/R>` | `<open Highs / trend>`                 | `<name>` |
| Budget/Capacity | `<G/A/R>` | `<burn vs plan / hiring>`              | `<name>` |

---

## 2) Milestones & Delivery

### 2.1 Timeline

| Milestone | Baseline | Forecast |     Actual |     Delta | Owner    | Notes               |
| --------- | -------: | -------: | ---------: | --------: | -------- | ------------------- |
| `<M1>`    | `<date>` | `<date>` | `<date/—>` | `+/-days` | `<name>` | `<risk/dependency>` |
| `<M2>`    |          |          |            |           |          |                     |

### 2.2 Delivered This Period (Evidence)

* `<Feature/Capability>` — link: `<PR/Demo/Doc>`
* `<Refactor/Infra>` — link: `<PR>`
* `<Decision (ADR)>` — link: `docs/adr/ADR-xxxx.md`

### 2.3 Planned vs Done (by Workstream)

| Workstream | Planned (pts/OKRs) |  Done | Delta | Evidence                          |
| ---------- | -----------------: | ----: | ----: | --------------------------------- |
| `<WS-A>`   |              `<N>` | `<N>` | `+/-` | PRs: `#123,#126` — Demo: `<link>` |
| `<WS-B>`   |                    |       |       |                                   |

---

## 3) Quality & Delivery Metrics (CI‑sourced)

> Keep numbers **machine‑verifiable**. Link to reports.

### 3.1 TDD & Testing

| Metric                              |               Current |        Target |       Trend      | Evidence                    |                          |
| ----------------------------------- | --------------------: | ------------: | :--------------: | --------------------------- | ------------------------ |
| **TDD Cycles (RED→GREEN→REFACTOR)** |                 `<N>` |           `↑` | `<up/down/flat>` | `relatorio‑tdd.json#cycles` |                          |
| **Watch Mode Usage**                | `<Yes/No/% sessions>` |       **Yes** |                  | PR template checks          |                          |
| **Test Pass Rate**                  |                 `<%>` |        `100%` |                  | CI run `<link>`             |                          |
| **Coverage (overall/key pkgs)**     |             `<%>/<%>` | `<threshold>` |     `<trend>`    | Coverage report `<link>`    |                          |
| **E2E Flakiness**                   |                 `<%>` |         `≤2%` |     `<trend>`    | Flaky dashboard `<link>`    |                          |
| **Contract Tests (breaking)**       |                  \`<0 |          N>\` |        `0`       |                             | Contract verify `<link>` |

### 3.2 Performance & Accessibility

| Metric                             |   Current |      Target |   Trend   | Evidence            |
| ---------------------------------- | --------: | ----------: | :-------: | ------------------- |
| Web TTI / INP / LCP / CLS          |  `<s/ms>` | `<budgets>` |           | Lighthouse `<link>` |
| Animations FPS                     |   `<fps>` |        `60` |           | Perf runs `<link>`  |
| A11y Violations (critical/serious) | `<N>/<N>` |       `0/0` | `<trend>` | Axe/Pa11y `<link>`  |

### 3.3 Reliability & Throughput

| Metric                      |         Current |  Target | Trend | Evidence               |
| --------------------------- | --------------: | ------: | :---: | ---------------------- |
| SLO Attainment              |           `<%>` |   `<%>` |       | SLO board `<link>`     |
| Error Budget Burn (period)  |           `<%>` | `≤100%` |       | Alerts `<link>`        |
| DORA: Deployment Frequency  | `</day or /wk>` |     `↑` |       | CI/CD `<link>`         |
| DORA: Lead Time for Changes |         `<h/d>` |     `↓` |       | VCS analytics `<link>` |
| DORA: Change Failure Rate   |           `<%>` |   `≤X%` |       | Incident log `<link>`  |
| DORA: MTTR                  |           `<h>` |  `≤X h` |       | Incident log `<link>`  |

### 3.4 Security & Supply Chain

| Metric                     |    Current | Target | Evidence                |
| -------------------------- | ---------: | -----: | ----------------------- |
| Open Vulns (Critical/High) |  `<N>/<N>` |  `0/0` | Dependabot/SCA `<link>` |
| Secrets in Repo (findings) |      `<N>` |    `0` | Secret scan `<link>`    |
| SBOM Generated             | `<Yes/No>` |  `Yes` | Pipeline `<link>`       |

---

## 4) Risks, Issues & Dependencies

> Keep each item **single‑sentence**, **impact‑first**. Use IDs for traceability.

### 4.1 Risks (open)

| ID      | Description (impact‑first)     |  Prob.  |  Impact |    RAG    | Owner    | Mitigation | Due      |
| ------- | ------------------------------ | :-----: | :-----: | :-------: | -------- | ---------- | -------- |
| `R‑001` | `<what could happen & effect>` | `L/M/H` | `L/M/H` | `<G/A/R>` | `<name>` | `<plan>`   | `<date>` |

### 4.2 Issues (blocking)

| ID      | Description            |  Severity  | Owner    |      ETA | Workaround   |
| ------- | ---------------------- | :--------: | -------- | -------: | ------------ |
| `I‑014` | `<what is broken now>` | `M/H/Crit` | `<name>` | `<date>` | `<temp fix>` |

### 4.3 External Dependencies

| ID      | Dependency      | Status                |    Risk   | Owner    | Next Step  |
| ------- | --------------- | --------------------- | :-------: | -------- | ---------- |
| `D‑003` | `<vendor/team>` | `<on‑track/slipping>` | `<L/M/H>` | `<name>` | `<action>` |

---

## 5) Decisions Since Last Report

| ADR       | Title              |           Date | Status                | Impact       |
| --------- | ------------------ | -------------: | --------------------- | ------------ |
| `ADR‑012` | `<short decision>` | `<YYYY‑MM‑DD>` | `<proposed/accepted>` | `<who/what>` |

---

## 6) Flags, Experiments & Kill‑Switches

* **Feature Flags:** list key flags with rollout %, success metrics, and **removal dates**.
* **Experiments (A/B):** variants, hypothesis, and current readout.
* **Kill‑switches:** any activations this period (when/why/outcome).

---

## 7) Incidents & Postmortems (if any)

| ID               |          When | Impact               | Root Cause | Fix       | Prevention       |
| ---------------- | ------------: | -------------------- | ---------- | --------- | ---------------- |
| `INC‑2025‑08‑XX` | `<date/time>` | `<scope & duration>` | `<5‑whys>` | `<patch>` | `<action items>` |

---

## 8) Next Period Plan (Outcomes)

* **O1:** `<outcome>` — measurable target, owner, due.
* **O2:** `<outcome>` — measurable target, owner, due.
* **O3:** `<outcome>` — measurable target, owner, due.

### 8.1 Gate Changes Proposed (if any)

* `<add/remove/raise thresholds>` with rationale and risk assessment.

---

## 9) Appendices & Links

* CI pipelines & last successful run: `<link>`
* Coverage & test reports: `<link>`
* Contract verification: `<link>`
* Perf/A11y dashboards: `<link>`
* SLO/Error budget dashboards: `<link>`
* Security/SBOM reports: `<link>`
* Release notes: `<link>`

---

## 10) Machine‑Readable Summary (optional)

> Include a JSON block to feed automated dashboards.

```json
{
  "period": "<YYYY-MM-DD to YYYY-MM-DD>",
  "rag": { "scope": "G", "schedule": "A", "quality": "G", "risk": "A", "budget": "G" },
  "milestones": [ { "id": "M1", "baseline": "<date>", "forecast": "<date>", "actual": null } ],
  "quality": {
    "tdd_cycles": <N>,
    "watch_mode_usage": "<Yes|No|%>",
    "pass_rate": <percent>,
    "coverage": { "overall": <percent>, "key": <percent> },
    "e2e_flakiness": <percent>,
    "contracts_breaking": <N>
  },
  "perf_a11y": { "tti": <sec>, "lcp": <sec>, "fps": <num>, "a11y_critical": <N> },
  "reliability": { "slo": <percent>, "error_budget_burn": <percent> },
  "dora": { "deploy_freq": "</day>", "lead_time": "<h>", "change_fail_rate": <percent>, "mttr": "<h>" },
  "security": { "vulns_critical": <N>, "vulns_high": <N>, "secrets_findings": <N>, "sbom": true },
  "top_risks": ["<R-001>","<R-002>"]
}
```

---

## 11) Completion Checklist

* [ ] Executive summary filled with **RAG** and outcomes
* [ ] Milestones (baseline/forecast/actual) updated
* [ ] **Evidence links** for everything claimed
* [ ] Quality & delivery metrics pulled from CI
* [ ] Risks/issues/dependencies triaged
* [ ] Decisions/flags/incidents documented
* [ ] Next period outcomes defined
* [ ] Machine‑readable summary (if used) updated

> Keep this report ruthlessly factual. Teams should be able to recreate every number from linked sources in minutes.
