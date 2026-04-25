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

Step 1: Connect to WRDS database (requires user credentials)
Step 2: Fetch stock price data from CRSP (crsp.dsf + crsp.dsenames)
Step 3: Fetch financial statement data from Compustat (comp.funda + comp.company)
Step 4: Fetch S&P 500 benchmark data from CRSP (crsp.dsi)
Step 5: Calculate market performance metrics
Step 6: Calculate financial health metrics
Step 7: Perform risk assessment scoring
Step 8: Generate 4-panel visualization with earnings season annotations
Step 9: Provide investment reference based on quantitative analysis


### Calculated Metrics

**Market Performance:**
- **Cumulative Return (%):** Total return over the entire analysis period = (Final Price / Initial Price - 1) × 100
- **Annualized Volatility (%):** Standard deviation of daily returns × √252, measuring price fluctuation risk
- **Maximum Drawdown (%):** Largest peak-to-trough decline, measuring worst-case historical loss
- **Sharpe Ratio:** (Mean excess return over risk-free rate) / Volatility, measuring risk-adjusted return (risk-free rate = 2%)
- **Excess Return vs S&P 500 (%):** Cumulative return above/below the market benchmark

**Financial Health (Latest Fiscal Year):**
- **Revenue:** Total company revenue
- **Net Income:** Bottom-line profit
- **Gross Margin (%):** Gross Profit / Revenue × 100
- **Net Margin (%):** Net Income / Revenue × 100
- **Debt Ratio (%):** Long-term Debt / Total Assets × 100
- **PE Ratio (TTM):** Price / Earnings Per Share
- **EPS:** Earnings Per Share

**Risk Assessment Factors:**
- Volatility > 50% = "High volatility" | > 30% = "Moderate-high volatility"
- Max Drawdown < -40% = "Severe drawdown" | < -20% = "Significant drawdown"
- Debt Ratio > 70% = "High debt ratio" | > 50% = "Moderate debt ratio"

**Investment Reference Categories:**
- Return > 50%: "Strong uptrend - buy on dips, manage position size"
- Return 0-50%: "Steady growth - hold and monitor fundamentals"
- Return -20-0%: "Short-term pullback - accumulate if fundamentals intact"
- Return < -20%: "Deep decline - reassess fundamentals carefully"

### Two Analysis Modes

| Feature | Mode 1: Single Company | Mode 2: Two Company Comparison |
|---------|----------------------|-------------------------------|
| **Text Output** | Full performance report + risk assessment | Comparison table + scoring |
| **Chart 1** | Price trend with earnings season overlay | Normalized price comparison (Base=100) |
| **Chart 2** | Cumulative returns (green/red shaded) | Cumulative returns comparison |
| **Chart 3** | Drawdown analysis with max DD annotation | 60-day rolling volatility comparison |
| **Chart 4** | Daily returns distribution histogram | Return correlation scatter plot |
| **Scoring** | N/A | 3-point system (return, volatility, drawdown) |

### Earnings Season Annotation
All price and return charts include light gold background shading for estimated earnings season periods (mid-January, April, July, October ± 45 days). This helps users distinguish between earnings-driven volatility and other market movements.

### Python Libraries Used
- `wrds` - WRDS database connection and query execution
- `pandas` - Data manipulation, cleaning, merging
- `numpy` - Mathematical calculations
- `matplotlib` - Data visualization with GridSpec layout
- `datetime` - Date handling

---

## 4. Key Findings

### Sample Analysis Results

**Apple (AAPL) vs Microsoft (MSFT) Comparison:**
- **Winner:** Microsoft (Score: 3-0)
- **Correlation:** 0.699 (moderately strong positive)
- Both companies showed similar returns (~93%) but Microsoft had slightly lower volatility (26.1% vs 26.6%) and smaller drawdown (-75.2% vs -84.3%)

**Best Performer (among tested):**
- **Costco (COST):** +141.0% cumulative return with only 22.7% volatility

**US vs China Pattern:**
- US companies averaged significantly higher returns with lower volatility
- Chinese ADRs faced persistent headwinds, with BABA (-62.8%), JD (-59.8%), and BIDU (-61.1%) all experiencing substantial declines
- Nvidia (NVDA) showed the highest volatility (52.8%) reflecting the AI boom cycle

**Earnings Season Impact:**
- Volatility spikes consistently appeared during earnings season windows across all technology stocks
- Financial stocks (JPM, V) showed more moderate earnings-related fluctuations

### Risk Distribution (14 analyzed companies)
- Low Risk: 7 companies (50%)
- Medium Risk: 7 companies (50%)
- High Risk: 0 companies

---

## 5. How to Run

### Prerequisites

1. **WRDS Account:** Register at wrds-www.wharton.upenn.edu
2. **Python 3.8+** installed on your computer
3. **Required packages** (install via pip)

### Installation Steps
1. Clone the repository
git clone https://github.com/YOUR_USERNAME/acc102-stock-analysis.git
cd acc102-stock-analysis

2. Install dependencies
pip install -r requirements.txt

3. Run the analysis
python stock_analysis.py

text

### Usage Guide

1. Launch the script - The tool will prompt for WRDS credentials
2. Enter username and password - These are not stored; used only for the current session
3. View company list - 15 preset companies with tickers and names are displayed
4. Select mode:
   - Press 1 for single company deep analysis
   - Press 2 for two company comparison
5. Choose company/companies by entering their number from the list
6. Review outputs:
   - Text analysis in the console
   - 4-panel chart displayed in a pop-up window
   - Investment reference at the bottom

### Expected Outputs

- Console: Detailed performance metrics, financial data, risk assessment
- Charts: 4-panel visualization with earnings season annotations
- Runtime: ~10-20 seconds per analysis

---

## 6. Product Link / Demo

- **GitHub Repository:** [Link to this repository]
- **Demo Video:** [Mediasite link - to be added before submission]

---

## 7. Reflection & Limitations

### What Worked Well

- **Automated Data Pipeline:** The WRDS connection and multi-table SQL queries successfully fetch and merge data from three different sources.
- **Comprehensive Metrics:** The combination of market performance with financial health provides a holistic view of each company.
- **User-Friendly Design:** The numbered menu system and clear English prompts make the tool accessible to non-programmers.
- **Earnings Season Innovation:** The quarterly background shading adds context that many free tools lack.
- **Cross-Market Comparison:** Including both US and Chinese companies enables unique insights.

### Limitations

**Data Limitations:**

- **Google (GOOGL/GOOG):** CRSP ticker mapping unreliable. Substituted with Visa (V).
- **Tencent (TCEHY):** OTC-traded ADR has sparse CRSP coverage.
- **Financial Data Coverage:** Some companies may show "N/A" for certain ratios.
- **Currency Effects:** Chinese ADR prices in USD reflect both stock and currency movements.
- **Survivorship Bias:** Only currently-listed companies included.

**Methodological Limitations:**

- **Earnings Season Estimation:** Uses fixed quarterly windows, not actual dates.
- **Risk-Free Rate Assumption:** Sharpe ratio assumes constant 2% risk-free rate.
- **S&P 500 Benchmark:** Not ideal for Chinese ADRs.

---

## 8. AI Disclosure

| Item | Details |
|------|---------|
| **AI Tool** | DeepSeek (DeepSeek-R1 model) |
| **Access Date** | April 2026 |
| **Used For** | SQL query syntax debugging, matplotlib layout optimization, code structure refinement |
| **Not Used For** | Problem definition, analysis logic, metric selection, data validation, interpretation of results |

**Student Declaration:** I confirm that I have run, checked, and understood every line of code in this project. I can explain all components of the workflow, including data retrieval, metric calculation, risk scoring logic, and chart generation. The problem definition, analytical framework, and interpretation of findings are entirely my own work.

---

## Repository Structure
acc102-stock-analysis/
├── README.md
├── stock_analysis.py
├── stock_analysis.ipynb
├── requirements.txt
├── .gitignore
└── figures/
├── single_analysis_example.png
└── comparison_example.png

text

---

## License

This project is created for the ACC102 Mini Assignment at Xi'an Jiaotong-Liverpool University (XJTLU). All financial data is sourced from WRDS and used exclusively for educational purposes. This tool is not financial advice.

---

**Author:** Yanzhu Xie
**Course:** ACC102 - Data Analysis for Business
**Institution:** XJTLU
**Academic Year:** 2025-2026
