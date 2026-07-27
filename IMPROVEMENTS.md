# Improvements Backlog

_Last updated: 2026-07-27 · Status: proposed, not yet started_

Improvements to the Stock Research tool **beyond** determinism. (Determinism has its own plan in `DETERMINISM_PLAN.md`.)

Items are prioritized. The **Correctness** group is where the current framework can produce *wrong* answers, not just inconsistent ones — do these regardless of whether the code rewrite happens.

---

## 1. Correctness (highest priority)

### 1.1 Sector guards for financials / REITs / insurers
The core metrics in the framework (EV/EBITDA, FCF, gross margin) are meaningless for banks and insurers, and REITs must be valued on FFO/AFFO, not EPS. Today the framework will confidently misanalyze them. Add a sector check that either swaps in the right metrics or explicitly declines to score these sectors with the standard rubric.

### 1.2 Input reconciliation checks
Cheap identities that catch bad provider data before it corrupts the analysis:
- `market cap ≈ price × diluted shares`
- `enterprise value ≈ market cap + total debt − cash`

If these don't reconcile within a tolerance, halt and surface the discrepancy. Garbage in, garbage out.

### 1.3 Peer-set definition
The framework leans on "peer median P/E" and "peer median EV/EBITDA" but never defines how peers are chosen. Undefined peers = meaningless relative valuation. Add explicit selection rules (same sub-industry, comparable size and growth) or a helper that builds the peer set.

### 1.4 Auto-enforce confidence from data completeness
Compute `fields_present / fields_required` and **cap** the confidence level in code, rather than trusting the LLM to self-report. Directly addresses the risk of a confident verdict built on thin data.

---

## 2. Analytical upgrades (biggest quality lift)

### 2.1 Reverse-DCF + sensitivity grid
Add two things to the valuation step:
- **Reverse DCF:** compute the growth rate the *current price* implies. Far more robust than a forward point estimate — it reframes the question as "is the market's implied growth achievable?"
- **Sensitivity grid:** show implied fair value across a grid of discount rate × terminal growth. Exposes how fragile the single-number DCF is.

This is the single largest analytical improvement; the current single-number DCF is falsely precise.

### 2.2 Automate the red-flag scan
Several Section 3D red flags are computable and should not depend on the LLM noticing them:
- receivables growing faster than revenue
- inventory growing faster than revenue
- stock-based compensation > 10% of revenue
- FCF diverging from net income
- share-count dilution

Compute these in code and feed the findings to the LLM.

### 2.3 Normalized earnings for cyclicals
The framework names "trough P/E" but gives no method. Add a normalized-earnings approach (e.g. `normalized EPS = mid-cycle margin × current revenue`) so the Cyclical Recovery archetype isn't hand-waved.

---

## 3. Workflow / longevity

### 3.1 Machine-readable scorecard
Emit a JSON scorecard alongside the human report. Makes the README's batch-screening idea real and enables ranking and tracking. (Falls out of the code rewrite for free.)

### 3.2 Watchlist + over-time tracking
Persist each run; diff scores across quarters; flag score changes or hits on the stated stop-loss thesis / thesis-invalidation triggers.

### 3.3 Backtesting harness
Run the framework on historical snapshots and check whether "Strong Buy" actually outperformed. This is the only real way to know whether the framework works — and it's a natural consumer of the pinned-snapshot infrastructure from the determinism plan (Phase 2).

### 3.4 Version the framework
Put a semver version in the header of `stock_research_framework.md` and record it in every report, so results stay comparable when the rubric is edited.

---

## 4. Nice-to-have

- **Caching layer** for API calls, to respect free-tier rate limits (e.g. FMP 250/day, Alpha Vantage 25/day).
- **Portfolio-level view** — position sizing is currently per-stock; add a check for sector/factor concentration across the whole portfolio.
- **Currency & share-class handling** — explicit handling for ADRs, dual-class shares, and non-USD reporting to avoid silent errors.
- **Calibration tracking** — record predicted confidence vs realized outcomes over time to see whether confidence is well-calibrated.

---

## Suggested order

1. Correctness (§1) — cheap, prevents wrong answers.
2. Reverse-DCF + sensitivity (§2.1) — biggest analytical payoff.
3. Automated red flags + normalized earnings (§2.2, §2.3).
4. Machine-readable output + tracking + backtesting (§3) — build once the core is solid.

---

## See also

- `DETERMINISM_PLAN.md` — the plan to make the pipeline reproducible.
- `stock_research_framework.md` — the rubric these improvements target.
