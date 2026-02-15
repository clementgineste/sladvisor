# SLA Advisor - Agent Instructions

## Project Overview

Single-page interactive SLA calculator/advisor. Users answer a few questions about their service requirements and get a recommended SLA tier with detailed implications (downtime, architecture, cost, penalties).

**Live URL**: `https://clementgineste.github.io/sladvisor/` (GitHub Pages)

## Tech Stack

- **Single `index.html` file** — everything lives here
- **Alpine.js** (v3, via CDN) — lightweight reactivity, no build step
- **Tailwind CSS** (Play CDN) — utility styling, no build step
- **No backend, no build pipeline, no package.json** — pure static site

### Why these choices

- GitHub Pages serves static files only → no SSR/build needed
- Alpine.js gives Vue-like reactivity in ~15kb without any tooling
- Tailwind Play CDN is fine for a single page (not for production at scale, but perfect here)
- Zero dependencies to maintain

## Architecture

All logic is client-side. The calculation engine is a pure JS function that takes user inputs and returns a recommendation object.

### Input variables

| Variable | Type | Values |
|---|---|---|
| `criticality` | enum | `low` / `medium` / `high` / `critical` |
| `downtimeTolerance` | enum | `hours` / `minutes` / `seconds` |
| `supportWindow` | enum | `business` / `extended` / `24x7` |
| `budgetLevel` | enum | `low` / `medium` / `high` |
| `rto` | enum | `4h` / `1h` / `15min` / `5min` |
| `rpo` | enum | `24h` / `1h` / `15min` / `realtime` |

### Output structure

```javascript
{
  tier: "Gold",                    // Bronze / Silver / Gold / Platinum
  slaPercent: 99.95,              // SLA percentage
  downtimeYear: "4h 22min",      // Calculated annual downtime
  downtimeMonth: "21min 54s",    // Calculated monthly downtime
  downtimeWeek: "5min 2s",       // Calculated weekly downtime
  architecture: [...],            // Required infra components
  costMultiplier: "3-5x",        // Relative cost vs baseline
  penalties: [...],               // Typical service credit structure
  warnings: [...],                // Inconsistency alerts
  cloudProviderSLAs: [...]        // Real-world reference SLAs
}
```

### SLA Tiers mapping

| Tier | SLA | Annual downtime | Use case |
|---|---|---|---|
| Bronze | 99.0% | 3d 15h 36min | Internal tools, dev envs |
| Silver | 99.9% | 8h 45min 36s | Standard business apps |
| Gold | 99.95% | 4h 22min 48s | Important customer-facing |
| Platinum | 99.99% | 52min 33.6s | Critical services |
| Diamond | 99.999% | 5min 15.4s | Life-safety, financial |

## Coding Guidelines

- **All code in `index.html`** — do not split into separate JS/CSS files
- **Alpine.js patterns**: use `x-data`, `x-model`, `x-show`, `x-text`, `x-bind`
- **Tailwind only** — no custom CSS unless absolutely necessary
- **French UI** — all user-facing text in French
- **English code** — variable names, comments in English
- **Accessible** — proper labels, ARIA attributes, keyboard navigation
- **Responsive** — mobile-first, works on all viewports
- **Dark mode** — support via Tailwind `dark:` classes and system preference

## Features (ordered by priority)

### MVP (v1)
1. Input form with the 6 variables above
2. SLA tier recommendation with downtime calculations
3. Architecture implications list
4. Warning system for inconsistent choices (low budget + high SLA)
5. Clean, professional UI

### v2
6. Reverse mode — "I have this budget/architecture, what SLA can I promise?"
7. Comparison table of all tiers side by side
8. Cloud provider real SLA references (AWS/GCP/Azure by service)
9. Export to Markdown/PDF
10. URL params to share a specific configuration

## Deployment

GitHub Pages from `main` branch, root `/`. No build step needed.

Settings: Repository → Settings → Pages → Source: Deploy from branch → `main` → `/ (root)`

## File conventions

- Single HTML file, sections marked with HTML comments: `<!-- SECTION: name -->`
- Alpine.js app data in a single `<script>` block at the end of `<body>`
- Keep the calculation engine as a separate named function for testability
