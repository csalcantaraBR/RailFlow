# `docs/guide/01-System-TDD-First.md` — TDD-First System Playbook (Template)

**Version:** `<v1.0>`
**Status:** `<Draft | Adopted | Deprecated>`
**Owner:** `<Team/Org>`
**Last updated:** `<YYYY-MM-DD>`

## 1) Purpose

Establish a lightweight, enforceable **TDD-first** workflow where **tests lead design**, quality is **automated by CI gates**, and delivery stays fast and safe across any stack (web, backend, data, mobile, infra).

## 2) Scope & Audience

* Applicable to: application services, libraries, frontends, CLIs, data pipelines, infra modules.
* Audience: engineers, QA, tech leads, SRE, and AI copilots (e.g., Cursor).

## 3) Principles

1. **Write a failing test first.** Behavior is specified before code exists.
2. **Small bets.** Short **Red → Green → Refactor** loops reduce risk.
3. **Transparent mocks.** Mocks unblock flow but are **time-boxed, disclosed, and tracked**.
4. **Contracts at boundaries.** APIs/protocols are **versioned** and **verified by tests**.
5. **Quality by default.** CI gates are **policy**, not suggestions.
6. **Watch everything during development.** Test runner stays in **watch mode** for zero-friction loops.
7. **Performance & Accessibility are requirements.** Not “nice-to-have.”
8. **Observability from day one.** Minimal logs/metrics/traces exist for key flows.
9. **Decisions are recorded, reversible.** **ADRs** are short and dated.
10. **AI is a copilot, not a pilot.** Humans own assertions, risk, and scope.

---

## 4) The TDD-First Workflow

### 4.1 Lifecycle

**RED → GREEN → REFACTOR → (CONTRACT?) → MERGE**

* **RED:** Add a test that fails for a **user-visible behavior** or **invariant**.
* **GREEN:** Implement the **minimum** to pass **only that test**.
* **REFACTOR:** Improve design with tests staying green.
* **CONTRACT (when applicable):** Validate consumer/provider **contract tests**.
* **MERGE:** Only if **all CI gates** pass.

### 4.2 Always-on Watch (mandatory)

Run tests in **watch mode** for the entire session so the loop re-executes automatically.

**Examples (adjust to your stack):**

* **Node (Vitest/Jest):** `pnpm test --watch` · `npx vitest --watch` · `npx jest --watch`
* **Python (pytest):** `ptw -- -q` (pytest-watch) or your team’s watcher script
* **Go:** `reflex -r '\.go$' -s -- sh -c 'go test ./...'`
* **Java (JUnit/Maven):** `mvn -Dtest=*Test -DfailIfNoTests=false -q -T1C test` + file watcher (IDE/Gradle continuous build)
* **.NET:** `dotnet watch test`

**Cursor prompt (paste at session start):**

> Open a persistent terminal and run the test runner in **watch** mode. Do not stop the process. Every time I edit tests or code, let the watcher re-run and show only the latest failures.

---

## 5) Source Control Conventions

### 5.1 Commit Messages (Conventional Commits + TDD tags)

* `test(red): <case>` — new failing test
* `feat|fix(green): <behavior>` — change that makes RED pass
* `refactor(core): <detail>` — refactor, no behavior change
* `docs(adr): <decision>` — add/update ADR
* `chore(ci): <gate>` — CI/pipeline quality gate work

### 5.2 Branching & PRs

* Model: `<Trunk-based | GitFlow>` (choose one).
* PRs: small, focused, include **TDD evidence** (link to RED/GREEN/REFACTOR commits).

---

## 6) Test Taxonomy & Targets

| Layer           | Goal (what to prove)                       | Speed | Data      | Target Coverage\* |
| --------------- | ------------------------------------------ | :---: | --------- | :---------------: |
| **Unit**        | Pure logic; invariants; no IO              |  Fast | In-mem    |  ≥ **80%** (pkg)  |
| **Contract**    | Client ↔ Service/API; schemas and versions |  Med  | Simulated |  **100%** surface |
| **Integration** | Real module + minimal deps                 |  Med  | Realistic |   Explicit list   |
| **E2E**         | Critical user flows                        |  Slow | Seeded    |  Per flow (check) |
| **Non-func.**   | Perf, a11y, visual snapshots, security     |  Med  | N/A       |   Budgets & SLOs  |

\*Define **per repository**; CI enforces thresholds.

**Flakiness budget:** ≤ **2%** failures per job; auto-quarantine flaky tests and open an issue.

---

## 7) Transparent Mocks Policy

Every mock must include:

* **ADR** (`docs/adr/`) with **purpose**, **scope**, **owner**, **expiry date**.
* **Replacement plan** (real integration) and **contract test** (if boundary).
* Label `mock:temporary` on issues/PRs; list in release notes until removed.

**Risk:** “happy-path mocks” that hide failure modes — classify as **High** and time-box strictly.

---

## 8) Contract Testing Policy

* Prefer **consumer-driven contracts** where feasible.
* Contracts are **versioned** (semver) and **backward-compatible** by default.
* **Breaking change protocol:** ADR + deprecation window + migration guide.
* CI job `contracts:verify` must fail on any break without an explicit ADR.

---

## 9) Feature Flags Governance

For each flag:

* **Name / Owner / Scope** (code paths)
* **Rollout** (cohorts/%), **metrics** to watch
* **Removal criterion/date** (set at creation)
  Tests must cover **ON** and **OFF** for critical flows.

---

## 10) CI/CD Gates (Minimum Policy)

**Required gates (block merge if failing):**

1. **Tests:** all layers configured for the repo
2. **Coverage:** ≥ `<threshold>` overall and per key package
3. **Typecheck/Lint:** zero errors
4. **Security:** dep scan + secret scan; no *High/Critical* (unless ADR-accepted)
5. **Contracts:** no breaking changes
6. **Budgets:** `<a11y | perf | bundle>` (if applicable)

### 10.1 GitHub Actions — Example Matrix (illustrative)

```yaml
name: quality
on: [pull_request]
jobs:
  build-test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [18, 20]   # adapt per stack
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: ${{ matrix.node }} }
      - run: pnpm i --frozen-lockfile
      - run: pnpm typecheck && pnpm lint
      - run: pnpm test:ci --reporter=junit
      - run: pnpm coverage:ci --min=${{ vars.COVERAGE_MIN || 80 }}
      - run: pnpm contracts:verify
      - run: pnpm security:audit
```

### 10.2 GitLab CI — Minimal Skeleton

```yaml
stages: [test, quality]
test:
  stage: test
  script:
    - npm ci
    - npm run test:ci
quality:
  stage: quality
  script:
    - npm run coverage:ci -- --min=$COVERAGE_MIN
    - npm run contracts:verify
    - npm run lint && npm run typecheck && npm run security:audit
```

---

## 11) Non-Functional Requirements (NFRs)

* **Performance budgets:** e.g., p95 latency `<N> ms` | TTI `<N> s` | 60 fps for animations
* **Accessibility:** WCAG 2.1 AA baseline (focus order, contrast, keyboard, SR labels)
* **Reliability:** SLOs (availability, error rates) and rollback policy
* **Privacy/Security:** data classification, retention, PII handling

---

## 12) Observability Minimum Viable (OMV)

Instrument key events from day one:

* **Domain events (examples):** `flow_started`, `flow_succeeded`, `flow_failed`, `retry`, `timeout`
* **Correlate:** requestId/sessionId/userId (if applicable)
* **Logs:** structured, with levels and redaction rules
* **Metrics:** latency, throughput, error rate
* **Traces:** critical edges (ingress → core → egress)

Dashboards: link in README. Alerts: page on SLO breaches.

---

## 13) Definitions of Ready & Done

**Definition of Ready (DoR)**

* Clear acceptance criteria + test strategy
* Telemetry & flag plan (if any)
* Data/contracts identified

**Definition of Done (DoD)**

* Demonstrated **RED → GREEN → REFACTOR**
* Tests pass locally and in CI; coverage meets threshold
* Contract tests green (if boundary touched)
* NFR budgets met (a11y/perf where applicable)
* ADR added for mocks/flags/architectural decisions
* README / release notes updated

---

## 14) Templates (Copy-Paste)

**14.1 ADR (short)**

```
# ADR-<ID>: <Decision>
Date: <YYYY-MM-DD>
Context:
Decision:
Consequences:
Rollback Plan:
Expiry (if mock/flag):
```

**14.2 PR Template (quality)**

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

**14.3 Test Case (Given/When/Then)**

```
Given <initial state>
When <action>
Then <observable outcome>
Notes: invariants | data | boundaries
```

---

## 15) Customization Guide (Fill this table per repo)

| Parameter           | Default / Example        | Your Value |
| ------------------- | ------------------------ | ---------- |
| Coverage threshold  | 80% overall; 80% per pkg |            |
| Contract test scope | Public APIs & events     |            |
| Perf budget         | p95 < 300ms / TTI < 2s   |            |
| A11y budget         | WCAG 2.1 AA              |            |
| Flakiness budget    | ≤ 2% per job             |            |
| Flag removal window | ≤ 2 sprints              |            |
| Mock expiry window  | ≤ 1 sprint               |            |

**Quick start**

1. Start **watch mode**. 2) Write a **failing test**. 3) Make it pass with minimal code.
2. Refactor safely. 5) Verify **contracts**. 6) Merge when **all gates** are green.

