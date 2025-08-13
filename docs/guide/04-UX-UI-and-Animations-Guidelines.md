# `docs/guide/04-UX-UI-and-Animations-Guidelines.md` — UX, UI & Motion Guidelines (Template)

**Version:** `<v1.0>`
**Status:** `<Draft | Approved>`
**Owner:** `<Design/UX Org>`
**Last updated:** `<YYYY-MM-DD>`

> **How to use:** Duplicate this file in your repo, fill the placeholders, and link it in your README. Keep it **technology‑agnostic**. Everything here should be **testable** (a11y/perf checks in CI) and **observable** (UX events).

---

## 1) Design Principles

1. **Clarity over cleverness** — reduce cognitive load; clear hierarchy and copy.
2. **Consistency** — components and patterns behave predictably across contexts.
3. **Accessibility first** — WCAG 2.1 AA baseline is a hard requirement.
4. **Performance is a feature** — fast feedback; motion never blocks interaction.
5. **Motion with meaning** — animate to communicate state changes, not to decorate.
6. **Progressive enhancement** — work without JS/animations; respect user preferences.

---

## 2) Design Tokens & Theming

Tokens are the **source of truth** for look & feel. Store them as code (e.g., JSON/Style Dictionary) and consume in all platforms.

### 2.1 Token Set (example)

```json
{
  "color": {
    "bg": { "default": "#0B0C0F", "surface": "#12141A", "elevated": "#171923" },
    "fg": { "primary": "#FFFFFF", "muted": "#A0AEC0", "brand": "#5EEAD4", "danger": "#F87171" },
    "border": { "subtle": "#2D3748", "focus": "#5EEAD4" }
  },
  "radius": { "sm": 6, "md": 12, "lg": 16, "xl": 24 },
  "space": { "xs": 4, "sm": 8, "md": 12, "lg": 16, "xl": 24, "2xl": 32 },
  "shadow": { "sm": "0 1px 2px rgba(0,0,0,.2)", "md": "0 6px 16px rgba(0,0,0,.25)" },
  "font": { "sans": "Inter, system-ui", "mono": "ui-monospace" },
  "size": { "xs": 12, "sm": 14, "md": 16, "lg": 18, "xl": 24, "2xl": 32 },
  "motion": { "duration": { "fast": 120, "base": 200, "slow": 320 }, "ease": { "standard": "cubic-bezier(.2,.0,.2,1)", "accelerate": "cubic-bezier(.3,0,1,1)", "decelerate": "cubic-bezier(0,0,.2,1)" } }
}
```

> **Customization:** provide light/dark palettes; define semantic color roles (success, warning, info). Tokens must be versioned.

---

## 3) Layout & Responsiveness

* **Grid:** `<n>` columns; gutters: `<space>`; container max‑widths by breakpoint.
* **Breakpoints:** xs `<≤<px>>`, sm `<…>`, md `<…>`, lg `<…>`, xl `<…>`.
* **Spacing scale:** use tokenized spacing; avoid arbitrary pixel values.
* **Density:** touch targets **≥ 44×44 px**; table row heights consistent.
* **Safe areas:** account for notches/OS bars on mobile.

---

## 4) Component Standards

For every component, document **states**, **a11y**, and **events**.

### 4.1 Core States (apply broadly)

`idle`, `hover/focus`, `active/pressed`, `disabled`, `loading`, `success`, `error`, `empty`.

### 4.2 State Contract (example)

| State   | Visual                              | Behavior                                 | A11y                                 |
| ------- | ----------------------------------- | ---------------------------------------- | ------------------------------------ |
| focus   | outline `color.border.focus`, `2px` | keyboard focus visible                   | `tabindex`, focus order, skip links  |
| loading | skeleton/progress                   | actions non‑blocking; cancel if possible | `aria-busy=true`, label updated      |
| error   | title + guidance + retry            | retry is first action                    | `role=alert`, focus moves to message |

### 4.3 Forms

* Labels always visible; placeholders are not labels.
* Inline validation on blur/change; summary on submit.
* Error text: concise, human, action‑oriented.
* Keyboard order logical; describe required/optional fields.

### 4.4 Navigation

* Primary nav pattern (`tabs/drawer/sidebar`) consistent across app.
* Back/Close semantics: **Back** returns to previous state; **Close** dismisses overlay.
* Breadcrumbs for deep hierarchies.

---

## 5) Accessibility (WCAG 2.1 AA Baseline)

* Contrast: text **≥ 4.5:1** (normal), **≥ 3:1** (large).
* Focus indicators visible and non‑ambiguous.
* All interactive elements reachable and operable by keyboard.
* Labels/roles/names programmatically determinable (ARIA where needed).
* Media alternatives: captions/transcripts; avoid autoplay with sound.
* **Reduced motion:** respect `prefers-reduced-motion` (see §6.4).
* **RTL & i18n:** support right‑to‑left; locale‑aware formats (dates, numbers, currency).

**CI a11y checks (example policy):** run automated checks (axe/pa11y/lighthouse) on PR; block merge on `critical` violations.

---

## 6) Motion & Micro‑Interactions

### 6.1 Purpose

Use motion to explain **spatial/structural change**, **cause‑effect**, and **status**.

### 6.2 Durations & Easing (defaults)

* Small changes: **120–200ms**; view transitions: **200–320ms**.
* Easing: `ease.standard` for most; `decelerate` for entrances; `accelerate` for exits.

### 6.3 Performance & Techniques

* Prefer `transform` and `opacity`; avoid animating layout properties.
* Target **60 fps**; budget CPU/GPU on low‑end devices.
* Interruptible animations: user input cancels or jumps to end state.

### 6.4 Reduced Motion Policy

When `prefers-reduced-motion: reduce` is set:

* Disable parallax, large movements, and background loop animations.
* Replace transitions with instant state changes or **≤ 80ms** fades.
* Provide non‑motion affordances (color/state changes).

### 6.5 Spec Template (for each animation)

```
Animation: <Name>
Trigger: <user/system>
Elements: <targets>
Start State → End State: <properties>
Duration/Easing: <ms>/<curve>
Interruptibility: <yes/no + behavior>
Reduced‑motion Alternative: <description>
Performance Notes: <GPU/transforms/limits>
Events: <telemetry names>
```

---

## 7) Loading & Perceived Performance

* Use **skeletons** for content; show **determinate** progress when possible.
* **Optimistic UI** for quick actions (with rollback on error).
* Prioritize **content over chrome**; lazy‑load non‑critical assets.
* Keep primary actions within initial viewport.

---

## 8) Error, Empty & Offline States

**Error:** title (plain language) + cause (if safe) + primary action + secondary (help).
**Empty:** explain value + quick start steps + link to docs/templates.
**Offline:** read‑only where possible; queue changes; visible sync status.

**Template**

```
Title: <human, no codes>
Body: <what happened, what to do>
Primary Action: <label>
Secondary: <link/help>
Illustration (optional): <asset>
Telemetry: <event names>
```

---

## 9) Content & Language Standards

* Voice: `<confident, helpful, concise>`; avoid jargon.
* Sentence case; punctuation consistent; avoid ALL CAPS.
* Error copy: own the problem, offer recovery, avoid blame.
* Inclusive language by default; localization keys stable.

---

## 10) Telemetry & UX Metrics

Instrument these **event families** (rename to fit your domain):

* `view_opened`, `cta_clicked`, `form_submit`, `form_error`, `retry_clicked`
* `load_skeleton_shown`, `content_visible`, `animation_skipped_due_reduce_motion`

**Core Web/App metrics:** TTI/INP/LCP/CLS (web) or app launch time/TTU (native). Set budgets and alert thresholds.

---

## 11) Testing Strategy for UX

* **Automated a11y**: run on PR; fail on criticals.
* **Visual regression**: snapshots for key screens/states (responsive matrix).
* **Motion tests**: verify `prefers-reduced-motion` switches and event emission.
* **Perf audits**: budget checks (TTI/LCP/FPS) in CI nightly.

---

## 12) Acceptance Criteria (per feature)

* All required **states** implemented and reachable.
* A11y checks pass; focus order verified; contrast meets budget.
* Motion follows spec; reduced‑motion path implemented.
* Loading strategy defined; errors/empty/offline states present.
* Telemetry events implemented and visible on dashboards.

---

## 13) Customization Matrix (fill per project)

| Area               | Default                                         | Your Value |
| ------------------ | ----------------------------------------------- | ---------- |
| Breakpoints        | xs/sm/md/lg/xl = `<...>`                        |            |
| Target size        | ≥ 44×44 px                                      |            |
| Contrast           | 4.5:1 (text), 3:1 (large)                       |            |
| Motion durations   | 120–200ms (small), 200–320ms (views)            |            |
| Reduced‑motion alt | Instant/≤80ms fades                             |            |
| Perf budgets       | Web: TTI `<2s>`, LCP `<2.5s>`; Animations 60fps |            |
| A11y policy        | WCAG 2.1 AA; CI blocker                         |            |

---

## 14) Templates (copy‑paste)

**14.1 Component Contract**

```
Component: <Name>
States: idle, focus, active, disabled, loading, success, error, empty
Props: <list>
Keyboard: <tab order, shortcuts>
Screen Readers: <role, name, description>
Events: <event names>
Telemetry: <when/how>
```

**14.2 Animation Spec**

```
See §6.5; include triggers, durations, easing, reduced‑motion alt, events.
```

**14.3 Empty State Spec**

```
See §8; include title, body, primary/secondary actions, asset, telemetry.
```

---

> Keep this file **living**. When tokens or policies change, update here and propagate via automation (design tokens → packages). Add CI checks to prevent regressions (a11y/perf/visual).
