# Stock Research Framework: Hidden Gem Identifier

You are acting as a research analyst. When given a stock ticker or company name along with financial data, work through each section below systematically. Use the data provided by the user as your primary source. You may supplement with your training knowledge for qualitative analysis (business model, moat, industry dynamics), but NEVER fabricate or guess financial numbers. If a data point was not provided and you are not confident in it, explicitly say "NOT PROVIDED — verify externally." For each section, assign a sub-score from 1-5. At the end, aggregate into a final verdict. Cite the source and date of every number you use.

---

## Section 1: Business Quality (Weight: 25%)

### 1A. What does the company do?
- Explain the business model in 2-3 sentences as if to a smart 12-year-old
- How does the company make money? Break down revenue streams by percentage
- Is the product/service a painkiller (must-have) or a vitamin (nice-to-have)?

### 1B. Competitive Moat
Score each moat source (0 = absent, 1 = weak, 2 = strong):
- **Switching costs:** How painful is it for customers to leave?
- **Network effects:** Does the product get better as more people use it?
- **Cost advantages:** Can they produce cheaper than competitors at scale?
- **Intangible assets:** Patents, brands, regulatory licenses, proprietary data
- **Efficient scale:** Is the market only big enough to support a few players?

### 1C. Total Addressable Market (TAM)
- What is the current TAM? Is it growing, and at what CAGR?
- What percentage of the TAM does the company currently capture?
- Is there a realistic path to expanding TAM (new products, geographies, adjacent markets)?
- Beware of inflated TAMs — use bottom-up estimates, not top-down consulting-firm numbers

### 1D. Management Quality
- Founder-led or professional management? Founder-led is a plus for growth-stage companies
- Does the CEO have skin in the game? (significant insider ownership > 5%)
- Capital allocation track record: have past acquisitions, buybacks, or reinvestments created value?
- Any red flags? (excessive compensation, related-party transactions, frequent strategy pivots)

**Sub-score: __ / 5**

---

## Section 2: Financial Health (Weight: 20%)

### 2A. Profitability Metrics
| Metric | Current | 3-Year Trend | Industry Median |
|--------|---------|-------------|-----------------|
| Gross Margin | | | |
| Operating Margin | | | |
| Net Margin | | | |
| Return on Equity (ROE) | | | |
| Return on Invested Capital (ROIC) | | | |

- Is ROIC consistently above the weighted average cost of capital (WACC)? This is the single most important indicator that the business is creating value.

### 2B. Balance Sheet Strength
- Debt-to-Equity ratio. Compare to industry average.
- Interest coverage ratio (EBIT / Interest Expense). Above 5x is healthy.
- Current ratio. Above 1.5 is comfortable.
- Net cash or net debt position?

### 2C. Cash Flow Quality
- Is Free Cash Flow (FCF) positive and growing?
- FCF conversion rate (FCF / Net Income). Above 80% suggests earnings are real, not accounting artifacts.
- Is the company generating cash from operations or relying on financing activities?
- CapEx as % of revenue — is it increasing (investing for growth) or decreasing (milking the business)?

### 2D. Red Flag Scan
Check for these warning signs:
- Revenue growing but cash flow declining (potential accounting tricks)
- Receivables growing faster than revenue (stuffing channels)
- Inventory growing faster than revenue (demand softening)
- Frequent equity dilution or stock-based compensation > 10% of revenue
- Off-balance-sheet liabilities, operating leases, or unusual related-party transactions
- Frequent changes in auditors or accounting methods

**Sub-score: __ / 5**

---

## Section 3: Growth Profile (Weight: 20%)

### 3A. Historical Growth
| Metric | 1-Year | 3-Year CAGR | 5-Year CAGR |
|--------|--------|-------------|-------------|
| Revenue Growth | | | |
| EPS Growth | | | |
| FCF Growth | | | |

### 3B. Growth Drivers
For each growth driver, assess sustainability (high/medium/low):
- Organic growth (new customers, higher ARPU, geographic expansion)
- Product expansion (new products, upselling, cross-selling)
- Margin expansion (operating leverage, pricing power, cost optimization)
- M&A (bolt-on acquisitions, roll-up strategy)

### 3C. The "Hidden Gem" Test
This is the key section for finding stocks before they explode. Score each factor:
- **Under-followed:** Fewer than 10 sell-side analysts covering the stock? (Fewer = more opportunity for mispricing)
- **Inflection point:** Is the company at a business inflection — new product launch, regulatory tailwind, industry shift, management change, or turning profitable for the first time?
- **Accelerating metrics:** Are revenue growth rates, margins, or customer counts accelerating quarter-over-quarter (not just growing, but growing faster)?
- **Optionality:** Does the company have free or underappreciated options — a side business, a patent portfolio, real estate, data assets, or platform potential that the market isn't pricing in?
- **Narrative shift potential:** Could a single catalyst (index inclusion, analyst initiation, partnership announcement, earnings beat) cause a re-rating of the stock?

### 3D. Unit Economics
- Customer Acquisition Cost (CAC) and Lifetime Value (LTV). Is LTV/CAC > 3x?
- Net Revenue Retention Rate (for SaaS/subscription). Above 110% means existing customers spend more each year.
- Churn rate and trends.

**Sub-score: __ / 5**

---

## Section 4: Valuation (Weight: 20%)

### 4A. Relative Valuation
| Metric | Current | 5-Year Average | Peer Median |
|--------|---------|---------------|-------------|
| P/E (Forward) | | | |
| P/E (Trailing) | | | |
| EV/EBITDA | | | |
| P/FCF | | | |
| PEG Ratio | | | |
| P/S (for pre-profit) | | | |

- Is the stock trading below its own historical average on key metrics?
- Is it cheaper than peers while growing faster?

### 4B. Intrinsic Value Estimate
Perform a simplified DCF:
- Estimate FCF for next 5 years using conservative growth assumptions
- Apply a terminal growth rate of 3-4%
- Discount at 10% (your opportunity cost / required return)
- What is the implied price? What is the margin of safety vs. current price?

### 4C. Scenario Analysis
| Scenario | Assumptions | Implied Price | Probability |
|----------|-------------|---------------|-------------|
| Bull | | | |
| Base | | | |
| Bear | | | |

- Expected Value = (Bull × P_bull) + (Base × P_base) + (Bear × P_bear)
- Is the expected value significantly above the current price?

### 4D. Valuation Context
- If the stock looks expensive: is it "expensive for a reason" (compounding at 30%+) or genuinely overvalued?
- If it looks cheap: is it a value trap (declining business) or a genuine mispricing?
- What would need to be true for the current valuation to be justified?

**Sub-score: __ / 5**

---

## Section 5: Catalysts & Risks (Weight: 15%)

### 5A. Upcoming Catalysts (next 6-12 months)
List specific, identifiable events that could move the stock:
- Earnings reports and expected beats/misses
- Product launches or FDA approvals
- Partnerships, contracts, or customer wins
- Regulatory decisions
- Index inclusion or analyst initiations
- Insider buying patterns (cluster buying is a strong signal)
- Buyback programs

### 5B. Risk Assessment
For each risk, rate severity (high/medium/low) and probability:
- **Customer concentration:** Does any single customer represent > 15% of revenue?
- **Regulatory risk:** Could regulation disrupt the business model?
- **Technology risk:** Could the product be disrupted or made obsolete?
- **Macro sensitivity:** How cyclical is the business? Does it hold up in a recession?
- **Key person risk:** Is the company overly dependent on a single leader?
- **Competitive risk:** Are well-funded competitors entering the space?
- **Geopolitical risk:** Supply chain or revenue exposure to unstable regions?

### 5C. Short Interest & Sentiment
- Current short interest as % of float. Above 10% means significant bearish bets.
- Is short interest increasing or decreasing?
- Institutional ownership trend — are smart money investors accumulating or distributing?

**Sub-score: __ / 5**

---

## Final Scoring & Verdict

### Weighted Score Calculation

| Section | Sub-Score (1-5) | Weight | Weighted Score |
|---------|----------------|--------|---------------|
| Business Quality | | 25% | |
| Financial Health | | 20% | |
| Growth Profile | | 20% | |
| Valuation | | 20% | |
| Catalysts & Risks | | 15% | |
| **TOTAL** | | **100%** | **__ / 5.00** |

### Decision Matrix

| Weighted Score | Verdict | Action |
|---------------|---------|--------|
| 4.0 - 5.0 | **Strong Buy** | Full position (3-5% of portfolio) |
| 3.5 - 3.9 | **Buy** | Starter position (1-2%), add on dips |
| 3.0 - 3.4 | **Hold / Watchlist** | Monitor for improving score |
| 2.5 - 2.9 | **Underperform** | Don't initiate; if held, review thesis |
| 1.0 - 2.4 | **Sell / Avoid** | Exit or avoid entirely |

### Confidence Level

Rate your confidence in this analysis:
- **High (80-100%):** Ample public data, well-understood industry, clear financials, strong historical pattern
- **Medium (50-79%):** Some data gaps, emerging industry, or thesis depends on uncertain catalysts
- **Low (below 50%):** Limited data, speculative thesis, or significant unknowns. Flag this prominently.

### Thesis Statement
Write a 3-sentence investment thesis:
1. **What:** What is the company and why is it mispriced?
2. **Why now:** What catalyst or inflection point makes this timely?
3. **What could go wrong:** The single biggest risk to the thesis.

### Position Sizing Guidance
- Never put more than 5% of your portfolio in a single hidden gem / small-cap
- Scale position size with confidence: High = full size, Medium = half, Low = quarter or watchlist only
- Set a stop-loss thesis (not just price) — define what would invalidate your thesis and force a re-evaluation
