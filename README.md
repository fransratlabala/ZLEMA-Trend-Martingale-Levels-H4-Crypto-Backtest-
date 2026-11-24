# ZLEMA-Trend-Martingale-Levels-H4-Crypto-Backtest-
Quantitative research framework testing ZLEMA trend signals and capped Martingale re-entries on H4 crypto OHLCV data, validated via bootstrap CIs, serial-dependence tests, leave-one-out checks, and Deflated Sharpe Ratio.# Statistical Audit: Dual-ZLEMA 5-Level Martingale Strategy (H4 Crypto Universe)

An empirical statistical audit and compounded risk simulation of a **dual-ZLEMA** entry strategy paired with a **5-level Martingale escalation ladder** across **10 H4 cryptocurrency pairs** from **`2018-01-21` to `2026-11-14`** (**`3,874` total trades**).

---

## 1. Executive Summary

* **Aggregate Expectancy (`R`-Units)**: **`+645.59R`** cumulative return (`Mean R = +0.166647R`, `PF = 1.17`, `Win Rate = 40.45%`, `Payoff Ratio = 1.72x`), with a **`3.99x`** return-to-max-drawdown ratio against a peak drawdown of **`-161.85R`**.
* **Signal vs. Martingale Sizing**: Both the Level 1 base signal (`n = 2,608`, `+157.21R`, `Mean R = +0.060278R`, `p = 0.0240`) and the de-Martingaled `1x` unit-normalized series (`n = 3,874`, `+343.70R`, `Mean R = +0.088719R`, `p = 0.000185`) carry genuine positive edge. The 5-level Martingale overlay amplifies cumulative R primarily through **position sizing** (`+301.89R` / `46.76%` of raw R).
* **Structural Breadth**: All **`10 / 10` H4 assets** finish net profitable (`HHI = 0.1359` vs. `0.1000` equal-weight), and both **Longs** (`+440.62R`, `p = 0.000030`) and **Shorts** (`+204.97R`, `p = 0.028166`) are statistically profitable with no significant directional asymmetry (`p = 0.122473`).
* **Temporal Regime Shift**: **`7 / 9` calendar years** are net profitable—including **`2022–2026` (`5` consecutive profitable years)**—while **`2019` (`-107.93R`)** and **`2021` (`-52.42R`)** dragged on the first half (`H1: +77.07R` vs. `H2: +568.52R`, Welch `p = 0.001352`).
* **Compounded Equity at `0.5%` Base Risk (`$10,000` Start, Strict Ruin Stop)**: Survived all **`3,874` trades** to finish at **`$188,133.24`** (`+1,781.33%` total return, **`CAGR = +39.51%/yr`**, peak **`$196,429.97`**), with an early account low of **`$4,504.82`** (`2020-02-12`), **`-56.96%` max percentage drawdown** (`Calmar = 0.69`), longest win/loss streaks of **`10` / `21` trades**, and up to **`10` concurrent open trades**.

---

## 2. Audit Structure & Research Questions

| Section | Focus | Key Findings |
| :--- | :--- | :--- |
| **RQ1 & RQ2** | Expectancy & Resampling | Positive drift validated across IID, Block, and Stationary bootstrap resampling accounting for serial correlation. |
| **RQ3 (3A–3D)** | Base Signal vs. Martingale Ladder | Base signal is profitable at `1x`; Martingale lifts total R via size scaling (`L2+` raw `+488.39R`, `p = 0.002580`; unit-normalized `L2+` vs. `L1` diff `+0.087032R`, `p = 0.099401`). |
| **RQ4A** | Outlier & Tail Sensitivity | Removing top `50` winners (`+349.29R` / `54.10%` of R) leaves `+296.31R` (`p = 0.02092`); `1%` two-sided trim leaves `+530.56R` (`p = 0.000132`). |
| **RQ4B** | Cross-Asset Decomposition | `10 / 10` H4 crypto pairs net positive (`LTCUSDT_H4` top at `+150.93R`; ex-top asset `+494.66R`, `p = 0.000199`; `HHI = 0.1359`). |
| **RQ4C** | Long vs. Short Symmetry | `1,933` Longs (`+440.62R`, `p = 0.000030`) vs. `1,941` Shorts (`+204.97R`, `p = 0.028166`); observed diff `+0.122344R` (`p = 0.122473`). |
| **RQ4D & 4E** | Temporal Stability & `R` Drawdowns | `H2` (`2022-12-16` to `2026-11-14`) strongly outperforms `H1` (`p = 0.001352`); peak `R` drawdown `-161.85R` (`2018-11-14` to `2020-02-12`), recovered `2022-07-18` (`1,531` trades / `1,341.5` days); max loss streak `21` trades (`-39.67R`); Level 5 losses `3 / 11`. |
| **RQ5 (5A+)** | Compounded Equity & Risk Tiers | Path-dependent simulation from `$10,000` with strict ruin stop (`equity <= 0` halts run), concurrent position tracking (`entry_time -> exit_time`), and fractional risk-tier sweep (`0.25%–5.00%`). |

---

## 3. Repository & Notebook Workflow

* **Environment**: Python 3.11 (`pandas`, `numpy`, `scipy`, `matplotlib`) run in Jupyter / JupyterLab.
* **Expected Trade Log Columns**:
  * `R`: Realized trade return in `R`-multiples.
  * `level`: Martingale escalation rung (`1` through `5`).
  * `exit_time` (or `close_time` / `timestamp`): Trade exit timestamp for chronological sorting and drawdown/equity tracking.
  * `entry_time` (or `open_time`, optional): Trade entry timestamp used by the sweep-line algorithm to compute true overlapping concurrent open positions.
  * `side` / `direction` (optional): Trade direction (`LONG` / `SHORT`) for Test 4C.

---

## 4. Live-Deployment Caveats & Risk Guardrails

1. **Martingale Escalation & Margin Stress**: Losing streaks up to **`21` consecutive trades** and **`3 / 11` (`27.27%`) Level 5 losses** create sharp exposure spikes. Enforce hard exchange margin buffers, per-ladder notional caps, and fee/slippage modeling before live execution.
2. **Deep Percentage & Underwater Drawdowns**: Even at **`0.5%` base risk**, peak drawdown reached **`-56.96%`** (account low **`$4,504.82`** at trade `#690` in Feb 2020) with up to **`10` concurrent open trades**. Avoid high base-risk percentages where geometric volatility drag and losing clusters can trigger severe drawdowns or ruin.
3. **Regime Non-Stationarity**: Recent strength in **`2022–2026`** (`Mean R = +0.293507R` in `H2`) followed a weak/negative **`2018–2021`** stretch (`2019: -107.93R`, `2021: -52.42R`). Size positions for adverse regimes rather than extrapolating peak-year (`2025: +213.12R`) returns.
4. **Right-Skewed Payoff Concentration**: Because the top `50` winners account for `54.10%` of raw `R`, missing or clipping large trend/escalation winners due to execution lag or premature stops can materially degrade realized performance.


<span style="color:red">**DISCLAIMER: THIS REPOSITORY, CODE, BACKTESTS, AND STATISTICAL AUDIT RESULTS ARE PROVIDED STRICTLY FOR EDUCATIONAL AND RESEARCH PURPOSES ONLY AND DO NOT CONSTITUTE FINANCIAL, INVESTMENT, OR TRADING ADVICE. PAST PERFORMANCE AND SIMULATED RESULTS ARE NOT INDICATIVE OF FUTURE RETURNS. ALGORITHMIC AND LEVERAGED/MARTINGALE TRADING CARRY SUBSTANTIAL RISK OF LOSS, INCLUDING COMPLETE LOSS OF CAPITAL. YOU ARE SOLELY RESPONSIBLE FOR YOUR OWN RESEARCH, RISK MANAGEMENT, AND TRADING DECISIONS.**</span>
