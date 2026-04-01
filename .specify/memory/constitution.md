<!--
SYNC IMPACT REPORT
==================
Version change:      (unversioned template) → 1.0.0
Bump type:           MINOR — first ratification; all sections and principles added.

Modified principles: N/A (initial ratification)

Added sections:
  - Core Principles (5 principles: Zero-Dependency, Browser-Native Storage,
    Performance & Weight Budget, Accessibility, Scope Discipline)
  - Technical Constraints
  - Development Workflow
  - Governance

Removed sections:    N/A

Templates reviewed:
  - .specify/templates/plan-template.md   ✅ No outdated references; no changes needed
  - .specify/templates/spec-template.md   ✅ No outdated references; no changes needed
  - .specify/templates/tasks-template.md  ✅ No outdated references; no changes needed
  - .github/agents/*.md                   ✅ No agent-specific (CLAUDE-only) references found
  - .github/prompts/*.md                  ✅ No outdated references found

Deferred TODOs:      None — all placeholders resolved.
-->

# DailyDo Constitution

## Core Principles

### I. Zero-Dependency (NON-NEGOTIABLE)

The entire application MUST live in a single `index.html` file. No npm packages,
no node_modules, no bundler, no build step, no CSS frameworks (Tailwind, Bootstrap, etc.),
and no JavaScript frameworks (React, Vue, Angular, etc.) are permitted. The only
permitted external resource is one optional web font (single font, single weight).
Implementation MUST use vanilla HTML5, CSS3 with custom properties, and ES6+
JavaScript exclusively. No modular UI libraries used.

**Rationale**: The project's core value proposition is zero-installation, browser-native
operation. Any build tooling or third-party dependency introduces installation overhead,
version drift, and complexity that directly contradicts the product vision.

### II. Browser-Native Storage Only

All data persistence MUST use `localStorage` exclusively. IndexedDB, cookies,
sessionStorage, and any form of server-side or cloud storage are prohibited. Data
MUST be keyed by local calendar date using the format `dailydo_YYYY-MM-DD`. No
personally-identifiable or sensitive information shall be stored. The application MUST
function fully offline after the initial load. Old date keys from prior days are
silently ignored and never archived.

**Rationale**: The explicit non-goals of cloud sync and user accounts are architectural
decisions, not deferred features. Restricting storage to `localStorage` enforces the
single-user, privacy-first, zero-backend constraint at the implementation level.

### III. Performance & Weight Budget (NON-NEGOTIABLE)

All of the following hard limits MUST be met at all times:

| Metric | Hard Limit |
|---|---|
| Total file size | < 50 KB |
| First Contentful Paint | < 300 ms |
| Time to Interactive | < 500 ms |
| External HTTP requests | ≤ 1 (optional font only) |
| JavaScript (minified) | < 5 KB |

Any change that would breach these limits MUST be explicitly justified and approved
via a constitution amendment before implementation.

**Rationale**: The lightweight constraint is a first-class product feature. Users choose
DailyDo because it is instantaneous and minimal. Exceeding the budget is a regression,
not an acceptable trade-off.

### IV. Accessibility (WCAG AA Minimum)

All interactive elements MUST be keyboard-navigable. Semantic HTML elements (`<ul>`,
`<li>`, `<button>`, `<input>`, `<label>`) MUST be used throughout; generic `<div>`
or `<span>` elements are prohibited as interactive controls. Focus indicators MUST
remain visible — removing `outline` via CSS is prohibited. Color contrast MUST meet
WCAG AA minimum ratios. Icon-only buttons (e.g., the delete × control) MUST carry
descriptive `aria-label` attributes. All checkbox inputs MUST be linked to their
visible labels via matching `for`/`id` pairs.

**Rationale**: Accessibility is non-negotiable quality, not a stretch goal. Violations
of semantic HTML or keyboard navigation are defects that make the app inaccessible to
keyboard users and assistive technologies, and are triaged with the same severity as
functional bugs.

### V. Scope Discipline — YAGNI Enforced

Features not enumerated in the MVP feature set (task input, task list, task completion,
task deletion, daily reset, progress indicator, localStorage persistence) are
**automatically out of scope for v1.0**. The following are permanently excluded:
user accounts, cloud sync, collaboration, sub-tasks, tags, priorities, calendar
integration, notifications, dark/light mode toggle, drag-and-drop reordering,
recurring tasks, and analytics or reporting.

New feature additions MUST NOT be implemented inline. They require a new constitution
revision, a version bump, and explicit documentation before any code changes may occur.

**Rationale**: Scope creep is the primary risk for a small, polished single-file
application. Scope discipline preserves the product identity — "one day at a time,
one task at a time" — and prevents the codebase from exceeding the maintainability
ceiling of a self-contained single-file architecture.

## Technical Constraints

- **Tech stack**: HTML5, CSS3 (custom properties), Vanilla JavaScript (ES6+). No exceptions.
- **File structure**: `dailydo/index.html` (entire app) + `README.md` (documentation only).
  Two files maximum.
- **Browser support**: Chrome 90+, Firefox 88+, Safari 14+, Edge 90+, iOS Safari,
  Android Chrome. Internet Explorer is explicitly unsupported.
- **Data model**: `localStorage` key `dailydo_YYYY-MM-DD`; value is a JSON object
  with `date` (string) and `tasks` array. Each task carries: `id` (UUID string),
  `text` (string, max 120 characters), `done` (boolean), `createdAt` (ISO 8601
  timestamp).
- **ID generation**: MUST use `crypto.randomUUID()` with a timestamp+random fallback
  for environments that do not support it.
- **CSS transitions**: State changes (task completion, hover effects) MUST use CSS
  transitions of 200–300 ms duration. JavaScript animation libraries are prohibited.
- **Responsive design**: Layout MUST be functional at a minimum viewport width of 320 px.
- **Task ordering**: Incomplete tasks MUST appear before completed tasks in the list at
  all times.

## Development Workflow

1. **Implement only what is specified** — every code change must map directly to an
   MVP feature item or an explicitly tracked task. Opportunistic additions are prohibited.
2. **Validate the weight budget after every change** — verify total file size remains
   under 50 KB before committing any modification.
3. **Manual browser testing is the acceptance gate** — since there is no build pipeline,
   acceptance is confirmed by opening `index.html` directly in all supported browsers
   and verifying the applicable success criteria.
4. **Constitution gates plan approval** — before beginning any feature plan or task
   breakdown, verify alignment with the five Core Principles. Any conflict detected
   during planning MUST be resolved by amending the constitution first, not by exception
   or workaround.
5. **No inline scope creep** — if a user story or task introduces a feature outside
   the MVP list, stop, record it as a deferred backlog item, and proceed only with
   in-scope work.

## Governance

This constitution supersedes all other practices, preferences, and agreements for the
DailyDo project. Any amendment requires:

1. A documented reason for the change.
2. A version bump per the versioning policy below.
3. An updated `Last Amended` date (ISO 8601 format: YYYY-MM-DD).
4. A propagation review of all dependent templates and agent guidance files.

**Versioning policy**: `MAJOR.MINOR.PATCH`.
- MAJOR: Backward-incompatible governance changes (principle removal, non-goal reversal,
  hard limit relaxation).
- MINOR: Additive changes (new principle, new section, new constraint category).
- PATCH: Non-semantic refinements (wording clarification, typo correction, formatting).

**Compliance review**: All implementation plans and task lists MUST include a
"Constitution Check" section confirming alignment with the five Core Principles
before development work begins.

**Version**: 1.0.0 | **Ratified**: 2026-03-20 | **Last Amended**: 2026-03-20
