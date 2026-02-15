# SLA Advisor

> Interactive SLA calculator — answer a few questions about your service requirements and get a recommended SLA tier with technical, financial and operational implications.

🔗 **[Try it live](https://sladvisor.dev)**

## Why?

When a client or PM asks for "99.99% availability", they don't always realize what it takes:
- **99.9%** → ~8h45min downtime/year — a single server with monitoring may suffice
- **99.99%** → ~52 minutes/year — multi-AZ, auto-scaling, blue-green deployments required
- **99.999%** → ~5 minutes/year — active-active multi-region, 10-50x cost multiplier

This tool bridges the gap between business needs and technical reality.

## Features

- SLA tier recommendation based on 6 criteria (criticality, tolerance, support, budget, RTO, RPO)
- Precise downtime calculation (yearly, monthly, weekly)
- Architecture implications for each tier
- Inconsistency warnings (low budget + high SLA)
- Side-by-side tier comparison
- Dark mode

## Stack

Intentionally minimal:
- Single **HTML** file
- **[Alpine.js](https://alpinejs.dev/)** for reactivity (~15kb, via CDN)
- **[Tailwind CSS](https://tailwindcss.com/)** for styling (Play CDN)
- Zero build, zero dependencies, zero backend

## Development

```bash
git clone git@github.com:clementgineste/sladvisor.git
cd sladvisor

# Open directly in browser
open index.html
# or
python3 -m http.server 8080
```

No `npm install`, no build step. Edit `index.html`, refresh the browser.

## Deployment

GitHub Pages from the `main` branch, root `/`. Custom domain: `sladvisor.dev`.

## License

MIT
