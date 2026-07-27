# Determinism Plan

_Last updated: 2026-07-27 · Status: proposed, not yet started_

A plan to make the Stock Research tool as deterministic and reproducible as possible.

---

## Context

Today the tool is two Markdown files:

- `README.md` — usage guide (how to gather data and prompt the LLM)
- `stock_research_framework.md` — the rubric the LLM follows

All work is done by an LLM reading the rubric. That means the tool is **not deterministic**: the same stock, run twice — or run on two different models — can produce different archetypes, sub-scores, DCF values, and verdicts. The main sources of non-determinism are:

1. **LLM sampling** — temperature > 0 varies output run-to-run; even temperature 0 is not bit-identical across model versions/hardware.
2. **Model differences** — different training data and reasoning produce different judgments.
3. **Arithmetic done by the model** — DCF, weighted-score, and expected-value math computed "in the model's head" is error-prone and varies. This is the biggest leak.
4. **Judgment steps** — archetype, moat (0/1/2), 1–5 sub-scores, bull/base/bear assumptions, probabilities are all opinions.
5. **Qualitative training knowledge** — differs by model and knowledge cutoff.
6. **Input data drift** — if data comes from live web/API instead of a fixed snapshot, the numbers themselves change over time and by source.

## Honest framing (what "deterministic" can mean here)

You **can** make the pipeline reproducible: same inputs + same pinned model + same version → same output. You **cannot** make the judgment layer model-invariant. So the realistic target is:

- **Fully deterministic** math, aggregation, and lookups (given a fixed input snapshot).
- **Pinned and low-variance** judgment (given a fixed model + version).
- **Fully auditable** runs (every output is reproducible from a recorded manifest).

Everything below serves those three goals.

---

## Core move

Split the tool along one line:

> **Code owns everything deterministic (data, math, lookups, aggregation). The LLM owns only judgment.** A strict JSON contract sits between them.

The arithmetic and the data drift — where non-determinism actually lives — leave the LLM entirely.

Recommended language: **Python** (yfinance / pandas / pydantic — the finance + structured-output ecosystem is there; Go would fight this).

## Target architecture

```
stockresearch/
  framework/stock_research_framework.md   # the rubric = LLM prompt source (versioned)
  src/stockresearch/
    schema.py     # pydantic: FinancialData, Judgments, Scorecard, Verdict
    fetch.py      # providers (yfinance / FMP) -> normalized FinancialData
    snapshot.py   # freeze/load input data keyed by (ticker, as_of_date)
    compute.py    # ALL math: ratios, CAGR, DCF, weighted score, blend, EV, sizing
    classify.py   # deterministic threshold checks (PEG < 1.5, coverage > 5x, ...)
    llm.py        # model call: temp 0, pinned version, returns validated Judgments
    render.py     # Scorecard + narrative -> human report + JSON
    cli.py        # stockresearch analyze AAPL --asof 2026-07-27
  snapshots/      # pinned input data per (ticker, date)
  runs/           # output + run manifest per analysis
  tests/
```

Data flow:

```
fetch -> snapshot -> llm(judgments) -> compute(scorecard) -> render
```

The LLM step sits in the middle and **never touches a number** — it reads normalized data and returns only opinions (as validated JSON).

---

## Determinism levers, ranked by impact

1. **Move all arithmetic to `compute.py`.** DCF, weighted score, weight blend, EV/equity bridge, expected value, and position sizing become pure, unit-tested functions. Biggest single win — removes the largest error class (LLM math).
2. **Pin the inputs (`snapshots/`).** Fetch once, write a snapshot keyed by `(ticker, as_of)`; every re-run reads the snapshot. Live/API data changing between runs is the #1 hidden source of "different answers."
3. **Strict I/O contract.** The LLM returns *only* a `Judgments` object: archetype(s), moat 0/1/2 per source, six sub-scores (1–5), DCF assumptions (growth, chosen discount rate `r`), scenario values + probabilities. Force valid output via JSON-schema / tool-calling / `response_format`, validate with pydantic, and retry on invalid.
4. **Pin the model.** `temperature = 0`, and record the exact model id + version in every run. Caveat: temp 0 is not bit-identical across provider versions, and some providers (e.g. Anthropic) do not expose a seed — so pinning the version string and snapshotting outputs is how you get real reproducibility, not a seed parameter.
5. **Deterministic aggregation + lookups in code.** The weight-blend algorithm, the decision-matrix band lookup, and the position-sizing table become pure functions with total-coverage tests (guards against the score-gap bug from the earlier review regressing).
6. **Run manifest per analysis.** Record: framework-MD hash, prompt hash, model + version, snapshot hash, library versions, timestamp. Makes any report exactly reproducible and auditable.
7. **(Optional) variance control via self-consistency.** Run the judgment step N times and take the *median* sub-score. Trades per-run determinism for lower cross-run variance; costs N×. Use only if sub-scores are observed swinging.

---

## Phasing (each phase is independently useful)

- **Phase 0 — Freeze the contract.** Define `schema.py` (`FinancialData`, `Judgments`, `Scorecard`, `Verdict`). Load-bearing; everything keys off it.
- **Phase 1 — `compute.py` + `classify.py` + unit tests.** Pure math, no LLM. Validate the DCF against a hand-worked example. Biggest determinism payoff, zero API dependency.
- **Phase 2 — `fetch.py` + `snapshot.py`.** Reproducible inputs.
- **Phase 3 — `llm.py`.** Temp 0, pinned model, structured output + validation/retry.
- **Phase 4 — `cli.py` + `render.py` + manifest.** Wire it together; emit human + JSON report.
- **Phase 5 — variance controls + golden tests.**

**Recommended starting point:** Phases 0–1. They remove the biggest determinism leak (LLM arithmetic) with no API dependency and no LLM calls — pure, testable code. Port the exact formulas and lookups from `stock_research_framework.md` so code and rubric stay in sync.

---

## How to test determinism

- **Golden tests:** fixed snapshot + fixed judgments → `compute.py` must produce an exact scorecard. Fully deterministic, fully testable.
- **Property tests:** weights sum to 100 after any blend; every sub-score ∈ [1, 5]; the verdict bands cover the full 1.00–5.00 range with no gaps (regression test for the earlier score-gap bug).
- **Cross-model variance report:** run the same snapshot through 2–3 models and measure sub-score spread. A QA metric (how model-dependent the judgments are), not a pass/fail gate.

---

## What stays non-deterministic (be honest)

The qualitative judgments themselves will still vary across different models. Anchors (added to the rubric), temperature 0, and optional ensembling **bound** the variance but do not eliminate it. Determinism is achievable for the math and lookups given a fixed input snapshot + fixed model; the judgment layer is "low-variance," not "identical across models."

---

## See also

- `IMPROVEMENTS.md` — non-determinism-related improvements to the tool.
- `stock_research_framework.md` — the rubric these formulas and lookups are ported from.
