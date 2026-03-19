# Indian_Stock_Analysis Tool in Python

### 📈 Overview

The **Indian Stock Analysis Tool (SAT)** is an interactive, browser-based dashboard for analyzing **NSE-listed Indian equities**.  
It helps you quickly:

- **Search** for Indian stocks with smart symbol suggestions (e.g. type `RELI`, get `RELIANCE.NS`).
- **Visualize** historical price action with candlestick charts, volume bars, and moving averages.
- **Compare** a stock against **sector** and **category** peers using preloaded and auto-updated datasets.
- **Assess** basic technical indicators such as **SMA (50/100/200)** and **RSI**.
- **Review** recently added stocks from your own local log/database.

The app is built using **Python, Flask, and Dash** on top of **Yahoo Finance** data, with additional local enrichment via JSON/Excel files.

---

### ✨ Key Features

- **Smart Symbol Search (India Focused)**  
  - Type at least 2 characters and get suggestions pulled from **Yahoo Finance** (filtered to `.NS` symbols).  
  - One-click buttons let you auto-fill the stock symbol input.

- **Dynamic Analysis Controls**  
  - Select **number of years** (1–10) for historical analysis.  
  - Toggle **Volume** and **Candlestick** view independently.  
  - Re-analysis is triggered by button click, Enter key, or relevant input changes.

- **Technical Indicators & Summary**  
  - Computes **50/100/200-day SMAs** and **RSI (14)**.  
  - Detects **Bullish/Bearish/Neutral** trend based on SMA-50 vs SMA-200.  
  - Provides a simple **BUY/SELL/HOLD-style** recommendation (for educational use only).  
  - Shows 1-year and selected-period **high/low** prices.

- **Pros & Cons Insight Panel**  
  - Dynamically generated **Pros** and **Cons** list based on:
    - SMA crossover (Bullish/Bearish).  
    - RSI zones (Overbought/Oversold/Neutral).  
    - Dividend presence in the selected period.  
    - Recent price momentum.  
  - Always includes general risk warnings (market volatility, external factors, etc.).

- **Sector & Category Intelligence (Local DB)**  
  - Uses `StockDataManager.py` to:
    - Maintain **sector** and **category** mappings in JSON (`data/sectors.json`, `data/categories.json`).  
    - Optionally enrich with **ISIN** information from a local `equities.xlsx`.  
    - Auto-update your local DB when you analyze a **new** stock.
  - Shows **Top Stocks by Sector** and **Top Stocks by Category** with basic BUY/SELL trend tags.

- **Dividends & Moving Averages Charts**  
  - Main chart:
    - Price line or candlestick.  
    - Volume bars (with green/red color).  
    - Dividend bars plotted on a secondary axis where available.  
  - Secondary chart:
    - 50/100/200-day **SMA curves** for visual trend analysis.

- **Recent Stocks Log**  
  - Maintains a `stock_log.txt` under `data/`.  
  - Shows last 5 added entries (stock name, timestamp, sector, category, ISIN, source).

- **Light/Dark Theme Toggle**  
  - Global **Light/Dark mode** switch.  
  - Automatically toggles **Plotly** chart template (`plotly_white` / `plotly_dark`).  
  - Responsive layout tuned for desktop and smaller screens.

---

### 🛠️ Tech Stack

| Component     | Technology / Library              |
| :----------- | :--------------------------------- |
| **Language** | Python 3.x                         |
| **Backend**  | Flask                              |
| **UI Layer** | Dash (Plotly Dash)                |
| **Charts**   | Plotly (`graph_objects`, subplots) |
| **Data API** | `yfinance` (Yahoo Finance)         |
| **Data Store** | Local JSON & Excel (`pandas`)   |
| **HTTP**     | `requests` (ticker search API)     |

---

### 📂 Project Structure (High Level)

```text
SAT_StockAnalysisTool/
├── SAT.py                 # Main Flask + Dash application
├── StockDataManager.py    # Local sector/category/metadata manager
├── ISIN.py                # (Optional) Utilities for ISIN handling
├── DATA/ or data/         # Local data directory (JSON, logs, Excel)
│   ├── sectors.json
│   ├── categories.json
│   ├── stock_log.txt
│   └── equities.xlsx      # (Optional) NSE/BSE master equities data
└── README.md              # This file
└── requirements.txt       # Program dependencies    
```

> Note: On Windows the data folder may appear as `DATA` in the repo, while `StockDataManager.py` uses `data`.  
> Ensure the folder name matches the casing expected in `StockDataManager.py` (`data` by default), or adjust the code accordingly.

---

### ⚙️ Installation & Setup

#### 1️⃣ Prerequisites

- Python **3.10+** (recommended).  
- A working internet connection (for live Yahoo Finance data).  
- Optional: An **NSE/BSE equities Excel file** (`equities.xlsx`) if you want accurate, locally sourced ISINs.

#### 2️⃣ Clone the Repository

```bash
git clone https://github.com/FaizReza/Project_Indian_Stock_Analysis_Tool.git
cd SAT_StockAnaysisTool
```

> Adjust the repository URL and folder name above to match your actual GitHub repo structure if different.

#### 3️⃣ (Recommended) Set Up Virtual Environment

```bash
python -m venv venv
venv\Scripts\activate   # On Windows
# source venv/bin/activate  # On macOS/Linux
```

#### 4️⃣ Install Dependencies

```bash
pip install -r requirements.txt
```

Otherwise, install the core libraries manually:

```bash
pip install flask dash yfinance plotly pandas requests openpyxl
```

#### 5️⃣ Prepare Local Data Files

- Ensure a `data/` (or `DATA/`) folder exists under the project root.  
- On first run, `StockDataManager.py` will:
  - Create `data/sectors.json` and `data/categories.json` populated with seed mappings.  
  - Create `data/stock_log.txt` when new stocks are logged.  
- If available, place your **NSE/BSE equities master file** as `data/equities.xlsx` with at least:
  - `SYMBOL` column for stock symbols.  
  - An `ISIN` column (name can vary, the tool searches for any column containing `"ISIN"`).

---

### ▶️ Running the Application

From the project directory:

```bash
python SAT.py
```

By default, the Flask app runs in **debug** mode and will start a local server, e.g.:

```text
http://<FQDN>:<Custom_Port>/
```

Open this URL in your browser to access the **Stock Analysis Dashboard**.

---

### 🧭 Using the Dashboard

- **Enter Stock Symbol**  
  - Type the stock name or ticker (e.g. `RELIANCE`, `TCS`, `INFY`).  
  - Choose from the suggestion buttons or press Enter.

- **Choose Analysis Duration**  
  - Set **Years** between `1` and `10` to widen/narrow the historical window.

- **Toggle Chart Options**  
  - `📊 Show Volume`: show/hide volume subplot.  
  - `🕯️ Candlestick`: switch between line chart and candlestick.

- **Interpret the Output Sections**
  - **Stock Overview**: Name, sector, category, symbol, exchange, ISIN, and related top stocks.  
  - **Price Summary & Recommendation**: High/low stats, trend label, and simple textual recommendation (`BUY` / `SELL` / `HOLD-style`).  
  - **Pros & Cons**: Bullet list driven by SMA, RSI, dividends, and price momentum.  
  - **Price Graph**: Price + volume + dividends.  
  - **Moving Averages Graph**: SMA 50/100/200 overlays.  
  - **Recently Added Stocks**: Last few stocks automatically logged to your local DB.

> ⚠️ **Important:** All recommendations and indicators are purely for **educational and experimental** purposes.  
> This tool is **not** a substitute for professional financial advice.

---

### 📓 Notes & Limitations

- Reliant on **Yahoo Finance** data availability and API behavior.  
- Some symbols may not return complete metadata (sector, category, ISIN) — in such cases, defaults like `"Unknown Sector"` are used.  
- SMA-200 and some indicators require sufficient data length; for recent IPOs or limited histories you may see neutral/placeholder messages.  
- Local JSON and Excel files are **mutable** and will grow as you analyze more stocks.

---

### 🤝 Collaboration & Customization

If you are interested in:

- Extending the dashboard with **fundamental ratios**,  
- Adding **BSE** or global market coverage,  
- Integrating more advanced analytics (e.g. backtesting, portfolio views), or  
- Deploying the app on a server (e.g. Gunicorn + Nginx, Render, etc.),

feel free to reach out!

---

### 📬 Contact

**Name:** Faiz Sultan  
**Email:** [faiz.sultan@gmail.com](mailto:faiz.sultan@gmail.com)

---
*Automate the boring stuff, focus on the architecture.*

