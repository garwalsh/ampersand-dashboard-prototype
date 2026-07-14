# Changelog

All notable changes to the prototype are recorded here. Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); this project uses [Semantic Versioning](https://semver.org/) at the 0.x level, where minor bumps carry feature and design iteration.

## v0.2.0 — 2026-07-13

Focus: sharpening the installation-detail experience, making the demo data internally consistent, and fixing hidden hardcoded behaviour in the operation slide-over.

### Added
- **Error-to-operation deep-link.** Clicking the "Most recent error" cell on an installation row now opens the operations view with the matching operation auto-selected in the slide-over — the same navigation pattern the alerts feed already uses. Row clicks still open the operations list.
- **Per-error log fixtures.** Named log entries for the four failure scenarios (Netsuite 400 invalid record, Netsuite 400 duplicate record, Hubspot 401 token expired, Salesforce 429 rate limit) with realistic request/response bodies.
- **Synthesized logs for success operations.** When a success operation is opened in the slide-over, a two-line log is generated on the fly from the operation's own data.

### Changed
- **Installation-table 7-day column redesigned** as a stacked green/red operations bar with an inline count row ("N successful · M failed"), replacing the previous "N synced / M failed" text. Colours and structure adapted from the internal integration-health design.
- **Column headers on the installation table** rewritten: `Provider` → `Integration`, `Connection` retained (an interim rename to `Status` was reverted because it collided with the operations page's Status column), `Last 7 days` → `Operations (7 days)`, `Last error` → `Most recent error`.
- **Data field `recordsSynced7d` renamed to `successfulOps7d`.** The value was always the successful-operations count; the previous name overloaded "synced," which at the operations level means records pulled in a single call.
- **Placeholder customer names replaced with realistic SaaS names.** `Demo Org` (twice), `demo-group`, and `demo-group-name-2` became Loom, Linear, Retool, and Vercel respectively. The Demo Org row referenced by the alerts feed was later renamed to Crunchbase after Notion was ruled out for colliding with earlier integration examples in the walkthrough.
- **Crunchbase Salesforce counts corrected** so Degraded actually looks degraded: 21 failures / 250 successes (previously 1 / 270). Alert-3 wording and type updated to match — "21 read failures" and `operation.repeated_failures`.
- **Customer-list subtext** "X of Y failing" → "X of Y with errors" to align with the "Most recent error" language.
- **Slide-over panel** now reads customer, provider, integration, log fixture, and accent colour from the actual clicked operation and its owning installation, rather than the hardcoded `Spreedly · Netsuite · readAndWriteNetsuite` and index-based log picker.
- **README** cleaned up: removed the "known quirk" note that documented the pre-fix slide-over behaviour.

### Fixed
- **Orphan `<th>Integration</th>`** on the installation-detail table header row that had no matching `<td>` in the body, silently shifting every column one slot to the left.
- **Wrong log fixtures on non-index-1 operations.** The slide-over previously showed `failure1` logs only when opening operation index 1, and `failure2` for everything else — so the Crunchbase 429 deep-link, the Hubspot 401 deep-link, and any other failure clicked from the operations list all showed the wrong content.

## v0.1.0 — 2026-07-09

Initial prototype covering both tasks in the take-home brief. Deployed as a single self-contained HTML file (React 18 + Babel via CDN, no build step) at https://garwalsh.github.io/ampersand-dashboard-prototype/.

### Added
- **Task 1 — Existing API data.** Health column on the customers list (Healthy / Degraded / Unhealthy) with a one-line failure summary; inline error context on installation rows (connection status, 7-day summary, most recent error with timestamp).
- **Task 2 — Proposed endpoints, wired into the UI.** Operation filtering by status, action type, and date range; full-text operation search with match highlighting; project-level alert feed with severity tiles and per-alert deep-linking into the exact operation.
- **README** with technical rationale (why single-file, no build), suggested walkthrough path, and a linked walkthrough video.
- **Placeholder customer names replaced** with Loom, Linear, Retool, Vercel, and Spreedly (the intentionally unhealthy scenario), plus one Demo Org row retained as the target of the alerts deep-link.
