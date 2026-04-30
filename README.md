# Stock Research Guide

How to use the [Stock Research Framework](stock_research_framework.md) with LLMs to analyze stocks.

---

## Data Gathering (Do This Before Prompting the LLM)

LLMs do not have reliable access to live financial data. **You provide the data, the LLM does the analysis.** The fastest way is to upload PDFs directly — no need to restructure anything.

### Option A: Upload PDFs Directly (Recommended — 5 minutes)

Modern LLMs (Claude, ChatGPT, Gemini) can read uploaded PDFs and extract the numbers themselves. Just download and upload these documents:

**From SEC EDGAR (free, go to the company's filing page):**
- Latest **10-Q** (quarterly report) — has income statement, balance sheet, cash flow, and management discussion
- Latest **10-K** (annual report) — same but for the full year, plus risk factors and business overview
- Latest **DEF 14A** (proxy statement) — executive compensation, insider ownership

**From the company's Investor Relations page:**
- Latest **earnings press release** — cleaner than the 10-Q for headline numbers
- Latest **earnings call transcript** — richest source for management tone, guidance, and strategic direction
- **Investor presentation** (if available) — useful but LLMs can struggle with chart-heavy slides

**What you still need to type in manually** (not available in company PDFs, takes 2 minutes on Yahoo Finance or Finviz):

```
CURRENT PRICE: $[X] | MARKET CAP: $[X]B
P/E (TTM): [X] | P/E (Forward): [X] | EV/EBITDA: [X] | P/FCF: [X] | PEG: [X]
5-Year Avg P/E: [X]
Peer Median P/E: [X] | Peer Median EV/EBITDA: [X]
Short interest: [X]% of float
Number of analysts covering: [X]
Insider buying/selling (last 6 months): [summary from OpenInsider]
Institutional ownership: [X]%
```

### Option B: Copy-Paste Key Numbers (If Your LLM Doesn't Support File Uploads)

If you can't upload files, pull numbers from free sources and paste them in this format:

| Data | Free Source |
|------|------------|
| Financial statements (3 years) | SEC EDGAR, Macrotrends, or Wisesheets |
| Valuation multiples | Yahoo Finance "Statistics" tab or Finviz |
| Analyst estimates & earnings surprises | Yahoo Finance "Analysis" tab |
| Insider transactions | OpenInsider or SEC Form 4 filings |
| Institutional ownership & short interest | Finviz or WhaleWisdom |
| Earnings call transcript | The Motley Fool (free) or SeekingAlpha |

**Paste format:**
```
COMPANY: [Name] ([TICKER])
DATA AS OF: [Date]
CURRENT PRICE: $[X] | MARKET CAP: $[X]B

--- INCOME STATEMENT (Annual, last 3 years) ---
[Paste table or key numbers]

--- BALANCE SHEET (Latest quarter) ---
[Paste table or key numbers]

--- CASH FLOW (Annual, last 3 years) ---
[Paste table or key numbers]

--- VALUATION MULTIPLES ---
P/E (TTM): X | P/E (Forward): X | EV/EBITDA: X | P/FCF: X | P/S: X | PEG: X
5-Year Avg P/E: X
Peer Median P/E: X | Peer Median EV/EBITDA: X

--- GROWTH ---
Revenue CAGR (3yr): X% | EPS CAGR (3yr): X%
Analyst consensus next year revenue: $X | EPS: $X
Number of analysts covering: X

--- OWNERSHIP & SENTIMENT ---
Insider buying/selling (last 6 months): [summary]
Institutional ownership: X%
Short interest: X% of float

--- RECENT EARNINGS CALL HIGHLIGHTS ---
[2-3 key quotes or bullet points from management]

--- RECENT NEWS ---
[Bullet points of material events]
```

### If You're Using an LLM with Web Search

Some LLMs (ChatGPT with browsing, Perplexity, Claude with web search tools) can fetch live data. Even so:
- Explicitly instruct the LLM to cite the URL and date for every number
- Cross-check at least the big numbers (revenue, EPS, P/E) yourself
- Web search is useful for qualitative data (news, transcripts) but unreliable for precise financial tables

---

## Where Each Framework Section Gets Its Data

| Framework Section | Covered by PDF Uploads? | Still Need to Type In? |
|---|---|---|
| Step 1: Archetype Classification | LLM does this from all provided data | No |
| Sec 2: Business Quality | Yes (10-K business overview, risk factors, proxy) | No |
| Sec 3: Financial Health | Yes (10-Q / 10-K financials) | No |
| Sec 4: Growth Profile | Yes (10-K multi-year + earnings call) | No |
| Sec 5A: Relative Valuation | No — filings don't contain market multiples | **Yes — P/E, EV/EBITDA, peers** |
| Sec 5B-D: DCF & Scenarios | Yes (FCF from filings) | Yes — current price |
| Sec 6A: Catalysts | Yes (earnings call, press release) | Partially — upcoming events |
| Sec 6B: Risk Assessment | Yes (10-K risk factors + LLM knowledge) | No |
| Sec 6C: Short Interest | No — not in company filings | **Yes — short %, institutional %** |
| Sec 7: Archetype Fit Test | Partially (LLM uses all uploaded data) | Yes — analyst count, insider buying |

**Rule of thumb:** PDFs cover ~80% of what you need. The remaining 20% is market-priced data (multiples, short interest, insider activity) that you type in manually from Yahoo Finance or Finviz in ~2 minutes.

---

## Prompt Templates

### Full Analysis with PDF Uploads (Recommended)

```
I've uploaded [COMPANY]'s latest 10-Q, 10-K, and earnings call transcript.
Here is the market data as of [DATE]:

CURRENT PRICE: $[X] | MARKET CAP: $[X]B
P/E (TTM): [X] | P/E (Forward): [X] | EV/EBITDA: [X] | P/FCF: [X] | PEG: [X]
5-Year Avg P/E: [X]
Peer Median P/E: [X] | Peer Median EV/EBITDA: [X]
Short interest: [X]% of float
Number of analysts covering: [X]
Insider buying/selling (last 6 months): [summary]
Institutional ownership: [X]%

Using the Stock Research Framework linked/attached below, analyze this
company. Extract all financial data from the uploaded documents — income
statement, balance sheet, cash flow, growth rates, risk factors,
management commentary. Use my market data above for valuation multiples
and sentiment. If any data point is missing from both the documents and
my market data, say "NOT FOUND IN PROVIDED DOCUMENTS" rather than guessing.

At the end, provide the weighted score, verdict, confidence level,
and a 3-sentence thesis.

[LINK OR PASTE FRAMEWORK]
```

### Full Analysis without PDF Uploads (Copy-Paste Approach)

```
Using the Stock Research Framework linked/attached below, analyze [TICKER].

Here is the financial data I've gathered:

[PASTE YOUR DATA BLOCK FROM OPTION B ABOVE]

Work through every section of the framework using the data above.
Use the numbers I provided — do not substitute your own.
If a data point is missing from what I provided and you are not
confident in it, mark it as "NOT PROVIDED" rather than guessing.
At the end, provide the weighted score, verdict, confidence level,
and a 3-sentence thesis.

[LINK OR PASTE FRAMEWORK]
```

### Quick Screen (for narrowing a watchlist before full deep-dive)

```
I'm screening these tickers for investment potential. For each one,
do a quick pass on ONLY these steps from my framework:
- Step 1 (Archetype Classification — which archetype fits?)
- Section 2A (What does the company do?)
- Section 5A (Relative Valuation — use the multiples I provide)

Tickers and current data:
[TICKER1]: P/E [X], P/S [X], # analysts [X]
[TICKER2]: P/E [X], P/S [X], # analysts [X]
...

For each ticker, state the archetype, a one-line thesis, and
whether it deserves a full deep-dive. Rank them.
```

### Earnings Call Analysis (just upload the transcript)

```
I've uploaded the latest earnings call transcript for [TICKER].

Using the framework, analyze this transcript. Focus on:
1. Which archetype(s) does this company fit based on the call?
2. Is management guiding for accelerating or decelerating growth?
3. Any new products, markets, or partnerships mentioned?
4. What risks or headwinds did they acknowledge?
5. What's the overall tone — confident, cautious, or defensive?
```

---

## Usage Notes

1. **PDF uploads make this fast.** Download 2-3 PDFs from SEC EDGAR (~3 minutes), type in market multiples from Finviz (~2 minutes), upload everything with the framework. Total: ~5 minutes of your time, then the LLM does hours of analysis work.

2. **The archetype classification is the most important step.** It tells the LLM what to emphasize. A Quality Compounder analysis focuses on moat durability and ROIC consistency; a Turnaround analysis focuses on catalysts and balance sheet survival; a Hidden Gem analysis focuses on discovery and inflection points. Without the archetype, every stock gets the same generic treatment.

3. **Stocks can fit multiple archetypes — that's often a strong signal.** A company that's both a Hidden Gem and GARP (under-followed but growing fast at a reasonable price) or a Turnaround and Deep Value (beaten down and trading below book) often represents the best risk/reward.

4. **Batch screening idea:** Use the Quick Screen prompt template above with 10-20 tickers. You only need to gather P/E, P/S, and analyst count for this pass (2 minutes on Finviz). Then do the full deep-dive with PDF uploads only for the 2-3 that pass the screen.

4. **Iteration:** After you use this on 5-10 stocks, you'll notice which sections you consistently find most useful and which feel like filler. Trim and customize accordingly — frameworks work best when they're yours.

5. **Best LLM setup for this workflow:** Use an LLM that supports file uploads and has a large context window. Claude (200K context), ChatGPT (128K), and Gemini (1M+) all work. A 10-Q is typically 30-60 pages, a 10-K is 80-200 pages — make sure your LLM can handle the file size. If the 10-K is too large, upload just the 10-Q (latest quarter) + earnings call transcript instead.

6. **Automate the market data with a financial API.** Instead of manually typing in price, multiples, short interest, and analyst data, you can hook up a financial data API to fetch it programmatically. A small script that takes a ticker and outputs the formatted market data block saves you the 2 minutes of manual lookup per stock.

   **Free APIs (no cost, good for individual investors):**
   - **Yahoo Finance (`yfinance` Python library)** — no API key needed. Covers price, P/E, EV/EBITDA, P/S, short interest, analyst estimates, insider transactions, and institutional holders. Most popular choice. Unofficial but reliable.
   - **Financial Modeling Prep** — free tier gives 250 requests/day. Covers ratios, DCF, earnings surprises, institutional holders. Requires free API key.
   - **Alpha Vantage** — free tier gives 25 requests/day. Covers fundamentals, earnings, balance sheet. Requires free API key.

   **Paid APIs (more reliable, richer data):**
   - **Polygon.io** — starts at $29/month. Institutional-grade data with real-time quotes.
   - **Quandl / Nasdaq Data Link** — comprehensive historical fundamentals and alternative data.
   - **Intrinio** — good for SEC filings + financial data in structured format.

   **How to use it:** Write a Python script that takes a ticker, calls the API, and prints the exact market data block from the Data Gathering section (price, multiples, short interest, insider activity, analyst consensus). Then just copy-paste the output into your LLM prompt alongside the uploaded PDFs. This turns the manual 2-minute Finviz lookup into a 5-second command.
