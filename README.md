# Ampersand Dashboard — Troubleshooting Redesign

An interactive React prototype exploring how [Ampersand's](https://www.withampersand.com/) dashboard could surface integration failures faster, plus three proposed API endpoints to close the remaining gaps. Built as a product take-home.

## View it

Live: https://garwalsh.github.io/ampersand-dashboard-prototype/

Or open `index.html` locally. No install, no build step.

## What's in the prototype

**Task 1 — better troubleshooting from existing API data**

- Health column on the customers list (Healthy / Degraded / Unhealthy) with a one-line failure summary
- Inline error context on installation rows: connection status, 7-day sync + failure counts, last error message with timestamp

**Task 2 — proposed new endpoints, wired into the UI**

- Operation filtering by status, action type, and date range
- Full-text operation search with match highlighting
- Project-level alert feed (sidebar tab with unacknowledged badge, severity tiles), where each alert deep-links to the exact operation

**Suggested walkthrough:** Customers → Spreedly → Netsuite hits most of the surfaces at once. Alerts → "View details →" demonstrates the cross-page deep-link.

## Technical approach

Single-file HTML, React 18 + Babel via CDN, no build step. State and demo data are top-level consts at the top of `index.html`.

**Why:** for a prototype meant to be reviewed by a small team, "open the file, it runs" beats "clone, install, dev-run" every time. The same principle compresses the loop between a spec and a PR — which matters for a PM working alongside a small eng team.

**What I'd change for production:**

- Vite + TypeScript, components split into files, data layer extracted
- Mock the proposed endpoints with MSW returning the JSON shapes from the exercise brief, rather than filtering flat arrays in-component
- Tests on the filter and search logic
- Fix a known quirk in `SlideOverPanel` where log fixtures are keyed off op index — any non-index-1 op currently shows the same failure logs

## Data model

All demo data lives at the top of `index.html`:

- `CUSTOMERS` — six orgs; Spreedly is the intentionally unhealthy scenario
- `INSTALLATIONS` — keyed by customer name; only Spreedly and Demo Org have entries
- `OPERATIONS` — flat array; each row has a `searchContent` string that powers the fake full-text index
- `ALERTS` — each carries `customerRef` + `installationIdx` + `operationIdx` so "View details →" can jump into the right operation
