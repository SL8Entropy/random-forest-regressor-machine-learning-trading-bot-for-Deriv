# Random Forest Regressor Machine-Learning Trading Bot for Deriv

**⚠ Disclaimer:**  
This project is for educational / research purposes only. Trading financial instruments carries substantial risk. Use this code at your own risk. The author is not responsible for any financial losses.

---

## 🚀 Overview

This repository provides a Python-based pipeline that:

1. **Gathers historical price data** from the broker/exchange (via the API) and saves it to a CSV (or Excel-compatible) file.  
2. **Calculates technical indicators** — specifically the Relative Strength Index (RSI) and Stochastic RSI (plus optionally other features).  
3. **Prepares a dataset** that includes raw price data + indicator columns, for machine-learning training.  
4. **Trains a machine-learning model** (Random Forest Regressor) on the dataset to predict future price movement/returns (or target variable of your choice).  
5. **Runs either:**
   - a **backtesting mode** (on historical data), or  
   - a **live-trading mode** (using the broker’s live API credentials).  

---

## 📁 Repository Structure *(as of now)*

```
/  ← root  
│  ml data gathering.py            ← script to fetch data and save price history  
│  rsi and stochastic calcer.py    ← script to compute RSI & Stoch RSI (and maybe other indicators)  
│  data.csv / data_with_indicators.csv  ← sample (or initial) data files  
│  backtesting_bot for random forest regressor.py     ← backtest script (regressor version)  
│  backtesting_bot_using_random_forest_classifier.py  ← optional classifier-based backtest variant  
│  trading_bot_using_random_forest_regressor.py       ← live-trading / execution script  
│  requirements.txt               ← list of Python dependencies  
│  [other files / plotting / log scripts, result graphs, etc.]  
```

---

## 🛠 Setup & Installation

Assuming you have Python (>= 3.x) installed, follow these steps:

1. (Recommended) Create and activate a virtual environment:  
   ```bash
   python3 -m venv venv  
   source venv/bin/activate        # on Linux/macOS  
   # On Windows: .\venv\Scripts\activate  
   ```

2. Install dependencies:  
   ```bash
   pip install -r requirements.txt
   ```

   This will install all required packages (pandas, scikit-learn, any API / HTTP libraries, etc.) used in the scripts.

---

## 📊 Usage — Data Gathering → Backtest or Live Trading

### Step 1: Data Gathering  
Use the script `ml data gathering.py` to fetch historical price data (OHLC, volume, etc.).  
- This will generate a CSV (or Excel-compatible data file).  
- You may need to specify output file name (e.g. `data.csv`, or a custom name).  
- Ensure that the file name (or path) matches what the subsequent scripts expect.

### Step 2: Indicator Calculation  
Run `rsi and stochastic calcer.py` to calculate indicators (RSI, Stochastic RSI, maybe others) based on the price data file.  
- This will produce a new CSV (e.g. `data_with_indicators.csv`) containing both price data and computed indicator columns.  
- Make sure that the input CSV file name matches the one generated in Step 1 (or update the script accordingly).

### Step 3A: Backtesting (on historical data)  
- Use `backtesting_bot for random forest regressor.py` (or the classifier variant) to train the model on the prepared data and simulate trades historically.  
- The script will read the data, train a Random Forest model, generate predictions/signals, simulate buying/selling per the logic implemented, and output performance metrics (profit/loss, win rate, cumulative returns, etc.).  

### Step 3B: Live Trading Mode  
- Use `trading_bot_using_random_forest_regressor.py` for live execution.  
- You must provide valid API credentials (`APP_ID` and `APP_TOKEN`) for the broker/exchange (in this case, for Deriv).  
- Make sure to set the correct credentials in the script (or via environment variables / config file as per your implementation).  
- Note: live trading involves real financial risk — make sure you thoroughly test on “demo” or small funds before using significant capital.  

---

## ✅ Requirements & Dependencies

All dependencies are listed in `requirements.txt`. To install:

```bash
pip install -r requirements.txt
```

(Assumes your virtual environment is active.)

---

## ⚙ Configuration Notes & Parameters

- **Excel / CSV naming:**  
  - When running data gathering or indicator-calculation scripts, make sure the output filename matches what later scripts expect.  
  - If you modify the filename in one script, update references in downstream scripts accordingly.

- **API Credentials (for live trading):**  
  - You need valid `APP_ID` and `APP_TOKEN` from Deriv.  
  - Store them securely — do not hardcode for public repos. Ideally, use environment variables or a config file (and add that file to `.gitignore`).  

---

## 📚 Background — Why RSI and Stochastic RSI?

- The Relative Strength Index (RSI) is a momentum oscillator measuring the speed and magnitude of recent price changes to identify overbought or oversold conditions. :contentReference[oaicite:3]{index=3}  
- The Stochastic RSI (Stoch RSI) applies the stochastic oscillator formula to RSI values (instead of price data) — giving a more sensitive oscillator with values typically between 0 and 1 (or 0–100). :contentReference[oaicite:5]{index=5}  

These indicators help the bot derive features about market momentum and likely price reversals — which in turn feed into the machine-learning model (Random Forest) for predictive trading signals.

---

## 💡 Tips & Recommendations Before Running / Using

- Ensure enough historical data is gathered — more data → better training.  
- Be cautious about overfitting: don’t train and test on the same period. Use **out-of-sample testing**.  
- Use realistic assumptions in backtesting (e.g. include slippage, fees) if possible.  
- Start with a demo or minimal capital when executing live trades.  
- Maintain secure handling of API credentials (do _not_ commit them).  

---

## 📛 License & Disclaimer

This project is provided **“as is”** for educational / research purposes only. Use at your own risk. The author does not provide financial advice, and is not responsible for any losses incurred using this code.

