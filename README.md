# ACC102 - Stock Analysis Tool: US & China Company Deep Dive

Description: A Python stock analysis tool that fetches real-time data from WRDS to analyze and compare 15 major US and Chinese companies. Features single-company deep analysis and two-company comparison with earnings season annotations, risk assessment, and investment insights.

---

## 1. Problem & User

### Problem Definition
Investors and finance learners often struggle to access and interpret professional-grade financial data. Most retail investment platforms offer limited analytical depth, while professional tools like Bloomberg Terminal are expensive and complex. There is a gap for a free, code-based tool that provides institutional-quality stock analysis with clear visual outputs.

### Target Audience
- **Primary User:** Individual investors making buy/hold/sell decisions on US and Chinese stocks
- **Secondary User:** Finance students learning quantitative portfolio analysis and risk assessment
- **User Need:** Quick comparison of stock performance, risk metrics, and financial health across multiple companies with intuitive charts

### Business Relevance
This tool transforms raw WRDS/CRSP/Compustat financial data into actionable investment intelligence. By automating data retrieval, metric calculation, and visualization, it saves users hours of manual spreadsheet work and reduces the risk of calculation errors. The tool is particularly valuable for cross-market analysis comparing US and Chinese equities.

---

## 2. Data

### Data Sources

| Source | Database | Tables Used | Purpose |
|--------|----------|-------------|---------|
| WRDS CRSP | Daily Stock File | `crsp.dsf`, `crsp.dsenames` | Stock prices, daily returns |
| WRDS CRSP | Daily Stock Index | `crsp.dsi` | S&P 500 benchmark returns |
| WRDS Compustat | Fundamentals Annual | `comp.funda`, `comp.company` | Financial statements, ratios |

### Key Variables

**Stock Data (CRSP):**
- `date` - Trading date
- `prc` - Closing price (adjusted for splits/dividends)
- `ret` - Daily total return

**Financial Data (Compustat):**
- `revt` - Total revenue
- `ni` - Net income
- `gp` - Gross profit
- `lt` / `at` - Long-term debt / Total assets (debt ratio)
- `epspx` - Earnings per share (basic, excluding extraordinary items)

**Market Data (CRSP):**
- `sprtrn` - S&P 500 daily return

### Time Period
**2021-01-01 to 2025-12-31** - This five-year window captures pre-COVID recovery, the 2022 bear market, the AI-driven tech rally of 2023-2024, and recent market conditions, providing a comprehensive view of performance across different market regimes.

### Companies Analyzed

**US Companies (10):**
| Ticker | Company | Sector |
|--------|---------|--------|
| AAPL | Apple Inc. | Technology |
| MSFT | Microsoft Corporation | Technology |
| AMZN | Amazon.com Inc. | Consumer/Technology |
| TSLA | Tesla Inc. | Automotive/Technology |
| V | Visa Inc. | Financial Services |
| NVDA | Nvidia Corporation | Technology/Semiconductors |
| META | Meta Platforms Inc. | Technology/Social Media |
| JPM | JPMorgan Chase & Co. | Banking |
| WMT | Walmart Inc. | Retail |
| COST | Costco Wholesale Corp. | Retail |

**China Companies (5 - US-listed ADRs):**
| Ticker | Company | Sector |
|--------|---------|--------|
| BABA | Alibaba Group | E-commerce/Technology |
| JD | JD.com Inc. | E-commerce |
| BIDU | Baidu Inc. | Technology/Search |
| NTES | NetEase Inc. | Technology/Gaming |
| TCEHY | Tencent Holdings | Technology/Social Media |

### Data Access Note
This project connects to WRDS via the `wrds` Python package. Data is fetched in real-time during analysis. Users must have their own WRDS account credentials. No local data files are stored in this repository.

---

## 3. Methods

### Analysis Workflow
