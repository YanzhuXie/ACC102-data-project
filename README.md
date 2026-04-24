# ACC102 - Stock Analysis Tool: US & China Company Deep Dive

Description: A Python stock analysis tool that fetches real-time data from WRDS to analyze and compare 15 major US and Chinese companies. Features single-company deep analysis and two-company comparison with earnings season annotations, risk assessment, and investment insights.
## 1. Problem & User

**Problem:** Investors and finance learners need a quick way to compare and deeply analyze stock performance across US and Chinese companies, understanding not just returns but also risk, drawdowns, financial health, and market correlations.

**Target User:** 
- Individual investors making buy/hold/sell decisions
- Finance students learning portfolio analysis
- Anyone wanting data-driven stock comparisons with clear visual outputs

**Business Relevance:** This tool transforms raw WRDS/CRSP/Compustat financial data into actionable investment insights, helping users make informed decisions based on quantitative analysis rather than emotion.

---

## 2. Data

| Item | Details |
|------|---------|
| **Source** | WRDS (Wharton Research Data Services) |
| **Databases** | CRSP Daily Stock File (`crsp.dsf`, `crsp.dsenames`), Compustat Fundamentals (`comp.funda`, `comp.company`), CRSP Daily Stock Index (`crsp.dsi`) |
| **Access Date** | April 2026 |
| **Key Variables** | |
| | Stock: `date`, `prc` (close price), `ret` (daily return) |
| | Financial: `revt` (revenue), `ni` (net income), `lt/at` (debt ratio), `epspx` (EPS), `gp` (gross profit), `oibdp` (operating income) |
| | Market: `sprtrn` (S&P 500 daily return) |
| **Time Period** | 2021-01-01 to 2025-12-31 |
| **Companies** | 15 companies (10 US + 5 China): AAPL, MSFT, AMZN, TSLA, V, NVDA, META, JPM, WMT, COST, BABA, JD, BIDU, NTES, TCEHY |

**Note:** This project uses WRDS API to fetch data in real-time. No local dataset is stored. Users need their own WRDS credentials.

---

## 3. Methods

### Core Python Workflow
