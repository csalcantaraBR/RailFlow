# RailFlow — Rails‑First Delivery Templates (README)

> **RailFlow** — _Build the rails, then flow._ These templates put ChatGPT into **RailFlow** mode: once the **rails** (tests, templates, CI gates) are laid, you mostly **monitor** and make **small corrections**, while quality runs automatically.
> 
> This README is in English to match the templates; adapt as needed.

---

## 1) What's inside (Templates)

Reusable, tech‑agnostic files under `docs/guide/`:

1. **01‑System‑TDD‑First.md** — TDD‑first playbook (workflow, watch mode, mocks/ADRs, contracts, CI gates).
2. **02‑Functional‑Spec‑MVP.md** — Functional spec (capability map, flows with BDD, rules, time semantics, contracts, NFRs, telemetry).
3. **03‑Project‑Brief‑and‑Standards.md** — Project brief + engineering standards (KPIs, quality bar, branching, docs).
4. **04‑UX‑UI‑and‑Animations‑Guidelines.md** — UX, a11y, and motion guidelines (states, reduced‑motion policy, loading, errors).
5. **05‑Status‑Report.md** — Evidence‑backed status report (metrics, links to CI/dashboards, JSON summary).

---

## 2) Benefits (Why RailFlow)

* **Faster feedback, less rework** — short **Red → Green → Refactor** loops with the test runner in **watch**.
* **Quality by policy** — CI gates (tests, coverage, contracts, a11y/perf/security) block merges when the bar isn't met.
* **Traceability** — ADRs + templates link decisions → tests → code → CI → reports.
* **AI as a copilot** — clear prompts turn ChatGPT into a **RailFlow** that drafts/updates the right docs.
* **Reuse across teams** — consistent structure = easier onboarding, reviews, audits.

---

## 3) Quick Start (Repository Bootstrap)

Create the base structure and add the templates.

### 3.1 New repo (bash)

```bash
mkdir -p docs/guide docs/adr
printf "# <ProjectName>\n\nStarter repo." > README.md
```

### 3.2 New repo (PowerShell)

```powershell
New-Item -ItemType Directory docs/guide, docs/adr -Force | Out-Null
"# <ProjectName>`n`nStarter repo." | Out-File -FilePath README.md -Encoding UTF8
```

### 3.3 Place the templates

Copy the five `.md` files into `docs/guide/`. Keep their names to preserve cross‑references.

> Track decisions in `docs/adr/` using the ADR template referenced in **01**.

---

## 4) Using ChatGPT to Fill the Templates

Two workflows work well:

### Option A — **Single Master Prompt** (recommended to start)

Paste into a **new ChatGPT conversation**:

```
You are a Methodology Facilitator for the RailFlow (Rails‑First Delivery) method.
Goal: initialize a new project by filling five templates under docs/guide/ while keeping the process TDD‑first and quality‑gated.

My high‑level idea (refine with me):
<PASTE YOUR PROJECT IDEA>

Please work in this order:
1) 01-System-TDD-First.md (workflow, watch mode, gates, contracts)
2) 03-Project-Brief-and-Standards.md (brief, KPIs, quality bar)
3) 02-Functional-Spec-MVP.md (capabilities, flows, rules, contracts, NFRs)
4) 04-UX-UI-and-Animations-Guidelines.md (states, a11y, motion)
5) 05-Status-Report.md (first reporting skeleton)

Rules:
- Keep it implementation‑agnostic; write measurable, testable statements.
- Propose default thresholds (coverage, budgets) and allow me to edit them.
- Add a short "Customization Matrix" at the end of each file with placeholders.
- Link sections to each other when relevant (e.g., contracts in 02 referenced by 01/03).
- Surface open questions explicitly with owners and due dates.

Deliver each file one by one, asking for my edits before moving to the next.
Output format: Markdown only, with the exact filename as the first line.
```

### Option B — **Step‑by‑Step** (one file per thread)

Use these prompts individually in ChatGPT:

**03 — Project Brief & Standards**

```
Act as a Methodology Facilitator. Fill `docs/guide/03-Project-Brief-and-Standards.md` for the project idea below. Keep it technology‑agnostic and testable. Propose KPIs, branching model, CI gates, and a customization matrix. Ask me targeted questions if critical.

Idea: <PASTE>
```

**02 — Functional Spec (MVP)**

```
Fill `docs/guide/02-Functional-Spec-MVP.md` using my idea below. Include a capability map, BDD examples for critical flows, a Business Rules library, time‑bound behavior, contracts & versioning, NFR budgets, telemetry with PII tags, and a traceability matrix. Keep it testable.

Idea: <PASTE>
```

**04 — UX, UI & Motion**

```
Fill `docs/guide/04-UX-UI-and-Animations-Guidelines.md` with component states, accessibility policy (WCAG AA), motion specs (durations/easing), reduced-motion policy, loading strategy, and error/empty/offline templates. Add a customization matrix.

Idea: <PASTE>
```

**01 — TDD‑First Playbook**

```
Fill `docs/guide/01-System-TDD-First.md` keeping TDD-first and test runner in watch mode. Include commit/PR conventions, mocks policy with ADRs and expiries, contract testing policy, feature flags governance, CI gates, observability minimum, and templates (ADR/PR/test case).

Context: reference the contracts & budgets from 02 and the quality bar from 03.
```

**05 — Status Report**

```
Fill `docs/guide/05-Status-Report.md` for the first reporting period. Add machine‑readable JSON summary and link placeholders for CI, coverage, contracts, perf/a11y dashboards, and SLOs. Keep it evidence‑backed.

Context: refer to the KPIs from 03 and test metrics from 01/02.
```

---

## 5) Recommended Completion Order

1. **01 — TDD‑First**: lay the rails (workflow, watch mode, gates, contracts, observability).
2. **03 — Project Brief & Standards**: align purpose, KPIs, quality bar, repo norms.
3. **02 — Functional Spec (MVP)**: define capabilities, flows, rules, time & data contracts.
4. **04 — UX, UI & Motion**: lock states, a11y, motion policies.
5. **05 — Status Report**: establish the cadence and evidence links.

> This order lays down the rails first (TDD workflow & gates), then clarifies scope and standards, and only then details specs and UX before reporting.

---

## 6) Turning Templates into Enforceable Policy

* **Watch mode during dev:** keep your test runner in `--watch` for friction‑free **RED → GREEN → REFACTOR**.
* **CI gates:** enforce thresholds (coverage, a11y/perf, contracts, security) as **blocking checks**.
* **ADRs & Mocks:** any mock/flag must have an ADR with purpose, expiry, and replacement plan.
* **Contracts:** verify consumer/provider in CI; block on breaking changes.
* **Telemetry:** instrument the events listed in the Functional Spec (02) and verify on dashboards.

---

## 7) Example Repo Commands

Create a first commit with the templates:

```bash
git add README.md docs/guide/*.md docs/adr
git commit -m "docs: add RailFlow templates and project README"
```

If you maintain a mono‑repo, link to these docs from each package/service README.

---

## 8) Collaboration Tips with ChatGPT

* **Be specific:** provide the project idea, target users, constraints, and success metrics.
* **Iterate:** ask ChatGPT to revise sections (e.g., "tighten KPIs", "add error states").
* **Ground in tests:** request BDD examples and acceptance criteria for every capability.
* **Ask for matrices:** coverage thresholds, perf/a11y budgets, traceability tables.
* **Keep scope tight:** small iterations beat giant specs no one reads.

---

## 9) License & Attribution

Add your project's license here. Attribution to the **RailFlow — Rails‑First Delivery** templates is appreciated when reused.

---

### One‑Shot Prompt (copy/paste)

```
Help me initialize a new project using the RailFlow (Rails‑First Delivery) templates. I will provide a product idea. Please produce, in order and as separate Markdown files, the filled versions of:
- 01-System-TDD-First.md
- 03-Project-Brief-and-Standards.md
- 02-Functional-Spec-MVP.md
- 04-UX-UI-and-Animations-Guidelines.md
- 05-Status-Report.md

Use measurable acceptance criteria, propose default thresholds, surface open questions, and keep everything technology‑agnostic. Ask for clarifications between files.
Idea: <PASTE HERE>
```
