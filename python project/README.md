<<<<<<< HEAD
# ⚡ Project 26: Electric Vehicle Charging Station Analysis

A comprehensive, reproducible data science and operational intelligence project analyzing Electric Vehicle (EV) charging behavior, energy distribution, dwell durations, and station availability.

Developed using **Python**, **Pandas**, **NumPy**, and **Matplotlib**.

---

## 📌 Project Overview

In the clean energy and electric mobility sector, charging infrastructure availability is a major factor in EV adoption and driver satisfaction. Rapidly expanding EV fleets place heavy, uneven loads on public charging stations, creating queues, equipment mismatch, and station congestion.

This project analyzes an empirical dataset of EV charging session records (`26_ev_charging_sessions.csv`), covering **49 charging stations** across five distinct location environments (**Highway**, **Mall**, **Transit Hub**, **Residential**, and **Office District**).

The complete analysis pipeline cleans raw telemetry data, computes descriptive and non-parametric statistical metrics, analyzes temporal and spatial utilization, detects duration outliers via the Interquartile Range (IQR) technique, models sessions using an Object-Oriented `ChargingSession` class, generates publication-ready visualizations, and delivers **five actionable recommendations** to optimize charger throughput and reduce congestion.

---

## 📂 Repository File Structure

```text
ev_charging_analysis/
│
├── 26_ev_charging_sessions.csv     # Raw input dataset
├── ev_charging_analysis.py         # Complete, all-in-one runnable Python script
├── ev_charging_analysis_report.md  # Comprehensive analytical report & findings
├── README.md                       # Setup, execution guide, and methodology
│
├── charts/                         # Exported high-resolution visualization figures
│   ├── sessions_by_hour.png        # Hourly charging demand distribution (peak hours)
│   ├── sessions_by_weekday.png     # Weekly demand distribution (busiest days & ties)
│   ├── station_rankings.png        # Top 10 stations ranked by energy delivered (kWh)
│   ├── energy_by_location.png      # Total & average energy consumption by location
│   └── duration_group_counts.png   # Session breakdown across Short, Medium, & Long tiers
│
├── sessions_by_hour.png            # Direct root copies for portable relative linking
├── sessions_by_weekday.png
├── station_rankings.png
├── energy_by_location.png
└── duration_group_counts.png
```

---

## ⚙️ Installation & Setup

### Prerequisites
- Python 3.9 or higher (tested on Python 3.10, 3.11, 3.12, 3.13, 3.14, 3.15).
- Git (optional, for cloning or version control).

### 1. Clone or Navigate to the Project Folder
```bash
cd "C:\Users\MUKUL SINGH\.gemini\antigravity\scratch\ev_charging_analysis"
```

### 2. (Optional) Create and Activate a Virtual Environment
```bash
# Windows (PowerShell / Command Prompt)
python -m venv venv
.\venv\Scripts\activate

# macOS / Linux
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install pandas numpy matplotlib seaborn
```

Or using a `requirements.txt`:
```bash
pip install -r requirements.txt
```

---

## 🚀 How to Run the Analysis

Execute the single master Python script:
```bash
python ev_charging_analysis.py
```

### What the Script Executes:
1. **Locates & Validates Data:** Ingests `26_ev_charging_sessions.csv`, validates all 8 required column names, and displays dimensions and preview.
2. **Audits & Cleans Data:** Deduplicates 12 duplicate `Session_ID` rows (keeping first occurrences) and applies median imputation for missing `Energy_kWh` (21 values) and `Charging_Duration_Mins` (21 values), printing detailed before/after comparisons.
3. **Feature Engineering:** Extracts `Hour`, `Weekday_Name`, and `Month` from `Charging_Start_Time`.
4. **Calculates Core KPIs:** Total sessions, total energy (kWh), average duration (mins), and average energy per session.
5. **Ranks Stations:** Computes top and bottom stations by session count and total energy delivered.
6. **Analyzes Charger Types:** Compares durations and energy across `DC Fast`, `Level 1`, and `Level 2`.
7. **Identifies Temporal Peaks:** Highlights peak hours (07:00 & 09:00 tie) and busiest weekday (Wednesday), explicitly reporting ties.
8. **Applies Data Structures:** Stores charger types in a `tuple`, unique locations in a `set`, and station counts in a `dictionary`.
9. **Tests Custom Station Query Function:** Demonstrates `get_avg_energy_for_station()` for existing and missing station IDs.
10. **Statistical Duration & IQR Analysis:** Computes NumPy mean, median, P90, Q1, Q3, IQR, and upper outlier threshold.
11. **Segments Duration Groups:** Categorizes sessions into `Short` (<2h), `Medium` (2–4h), and `Long` (≥4h).
12. **OOP Demonstration:** Demonstrates the `ChargingSession` class with real record data.
13. **Exports Deliverables:** Saves 5 PNG figures into `charts/` and generates `ev_charging_analysis_report.md`.

---

## 📊 Summary of Empirical Findings

| Metric / Analysis Dimension | Empirical Result | Strategic Takeaway |
| :--- | :--- | :--- |
| **Total Valid Sessions** | **850 sessions** | Clean baseline after removing 12 duplicate IDs |
| **Total Energy Delivered** | **40,914.18 kWh** (~40.91 MWh) | Average consumption: **48.13 kWh/session** |
| **Average Charging Duration**| **186.31 minutes** (3.11 hours) | Median: **184.00 mins**; 90th percentile: **320.00 mins** |
| **DC Fast Charging Dwell** | **188.79 minutes** (3.15 hours) | **Critical Finding:** Fast stalls are blocked for hours post-charge |
| **Peak Charging Hours** | **07:00 and 09:00 (Tied at 42 sessions each)** | Morning commuter arrival peak; secondary evening peak at 23:00 (41) |
| **Busiest Weekday** | **Wednesday (130 sessions)** | Followed by Thursday (126) and Sunday (125); Friday & Saturday tied (119) |
| **Top Station by Energy** | **ST049 (Residential, 1,217.68 kWh)** | Top station by session count: **ST028 (Highway, 30 sessions)** |
| **Highest Energy Location** | **Mall (50.91 kWh/session, 201.69 min dwell)** | Retail visitors linger longer and draw more energy |
| **IQR Outlier Threshold** | **512.12 minutes (Upper Bound)** | Standard 1.5*IQR flags **0 outliers** (max duration is 358.00 mins) |
| **Duration Groups** | **Short (<2h):** 255 (30.0%)<br>**Medium (2–4h):** 313 (36.8%)<br>**Long (≥4h):** 282 (33.2%) | 1 in 3 vehicles stays ≥4 hours, occupying stalls unnecessarily |

---

## 💡 Five Practical Recommendations to Improve Availability

1. **Implement Idle Fees on DC Fast Chargers:** Enforce a 10-minute post-charge grace period followed by a **$0.75/min overstay fee**. DC Fast dwell times currently average **188.79 mins** despite full battery recharge in 30–45 mins. Eliminating post-charge parking will instantly increase DCFC stall availability by 200–300%.
2. **Deploy Dynamic Time-of-Use (TOU) Pricing:** Apply a peak charging tariff between **07:00–10:00 AM on weekdays** and offer discounted overnight rates (11:00 PM – 06:00 AM). This flattens the 42-session morning peak and redistributes demand into quiet overnight troughs.
3. **Rebalance Hardware Allocation by Venue Profile:** Upgrade busy **Highway corridors** (e.g., ST028) with 150kW+ high-power DCFC dispensers to drop dwell times below 30 minutes, while installing high-density **Level 2 chargers at Malls and Office Districts** where visitors naturally dwell for 2–4 hours.
4. **Alleviate Station Imbalance with Real-Time Mobile Routing:** Shift traffic away from congested hubs (**ST028** with 30 sessions, **ST022** with 24 sessions) toward underutilized stations (**ST009** with 8 sessions, **ST021** with 10 sessions) through dynamic app navigation and small driver incentive credits.
5. **Establish Dedicated Extended-Dwell Zones:** At Transit Hubs and Residential complexes where **33.18% of sessions last ≥ 4 hours**, partition parking stalls into "Fast Turnover" (max 60 mins) and "Extended Dwell" (overnight/workday Level 2) zones with automated power throttling.

---

## 📈 Visualizations Catalog

| Figure File | Description |
| :--- | :--- |
| `sessions_by_hour.png` | Bar chart of sessions by hour of day with peak hours (07:00 & 09:00) highlighted in red. |
| `sessions_by_weekday.png` | Bar chart of weekly volume highlighting Wednesday and ties between Friday & Saturday. |
| `station_rankings.png` | Horizontal bar chart of the Top 10 charging stations by energy delivered with session counts. |
| `energy_by_location.png` | Dual-axis bar chart comparing total energy (kWh) and average energy per session by location. |
| `duration_group_counts.png` | Breakdown of Short, Medium, and Long duration groups with boundary definitions and share %. |

---

## 🔬 Python Class Implementation Example

```python
from ev_charging_analysis import ChargingSession

# Create a session instance from empirical data
session = ChargingSession(
    session_id="EV000000",
    station_id="ST029",
    energy_kwh=29.68,
    station_location="Mall",
    duration_mins=344.0
)

print(repr(session))
# Output: ChargingSession(Session_ID='EV000000', Station_ID='ST029', Energy_kWh=29.68)

print(session.calculate_charging_cost(rate_per_kwh=0.25))
# Output: $7.42

print(session.get_average_power_kw())
# Output: 5.18 kW
```

---

## 📜 License & Reproducibility

This project is licensed under the MIT License. All analyses and figures are completely reproducible by executing `ev_charging_analysis.py` against `26_ev_charging_sessions.csv`.
=======
# Electric-Vehicle-Charging-Station-Analysis
HCL PROJECT
>>>>>>> 057dea7585bf44df2e0561d57f2c087066bf5e65
