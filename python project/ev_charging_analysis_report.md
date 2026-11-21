# ⚡ Electric Vehicle Charging Station Analysis Report
**Industry:** Transportation / Clean Energy  
**Dataset:** `26_ev_charging_sessions.csv`  
**Pipeline Run Date:** November 2026  
**Status:** Completed & Validated  

---

## Executive Summary

This project evaluates operational charging session logs across a public/semi-public Electric Vehicle (EV) network consisting of 49 stations across five primary location environments: **Highway**, **Mall**, **Transit Hub**, **Residential**, and **Office District**.

The primary objective is to analyze energy delivery, dwell times, station utilization patterns, and equipment charging speed dynamics to resolve availability bottlenecks and formulate five empirical, high-impact recommendations to improve charger availability.

```
Total Charging Sessions Analyzed:  850 unique sessions
Total Electrical Energy Delivered: 40,914.18 kWh (40.91 MWh)
Average Charging Duration:         186.31 minutes (3.11 hours)
Average Energy Delivered/Session:  48.13 kWh
Total Active Charging Stations:    49 stations
```

---

## 1. Dataset Inspection & Quality Audit

The raw dataset was ingested and checked for schema integrity, column types, missingness, and record deduplication.

| Inspection Metric | Empirical Finding | Action Taken |
| :--- | :--- | :--- |
| **Initial Dimensions** | 862 rows, 8 columns | Audited all columns |
| **Duplicate Records** | 12 duplicate `Session_ID` instances (24 total rows) | Deduplicated: retained 1st occurrence, removed 12 exact duplicates |
| **Post-Dedup Count** | 850 unique sessions | Clean baseline created |
| **Missing `Energy_kWh`** | 21 values (2.47%) | Median Imputation (49.10 kWh) |
| **Missing `Charging_Duration_Mins`**| 21 values (2.47%) | Median Imputation (184.00 mins) |
| **Required Schema Check** | All 8 required columns verified present | Confirmed matching standard naming conventions |

### Missing Data Handling Justification
Rather than discarding records (listwise deletion), which would discard 42 informative sessions (4.94% of the total dataset) and eliminate valid spatial/temporal metadata, **Median Imputation** was applied:
- **Resistant to Skewness:** The median is non-parametric and immune to extreme distribution tails.
- **Negligible Distribution Distortion:** The post-imputation mean energy shifted by only **0.0245 kWh** (48.11 vs 48.13), and post-imputation duration shifted by only **0.0586 mins** (186.37 vs 186.31).
- **Preserved Sample Size:** Exactly 850 complete session observations were maintained for all downstream multidimensional analyses.

---

## 2. Core Operational Metrics & Equipment Dynamics

### Overall Network Key Performance Indicators (KPIs)
- **Total Valid Sessions:** 850
- **Total Energy Delivered:** 40,914.18 kWh
- **Average Energy per Session:** 48.13 kWh
- **Average Duration per Session:** 186.31 mins (3.11 hours)

### Charger Type Comparison
Analysis reveals a critical behavioral pattern regarding charger equipment:

| Charger Type | Session Count | Session Share (%) | Total Energy (kWh) | Avg Energy/Session (kWh) | Avg Duration (Mins) | Avg Duration (Hours) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **DC Fast** | 290 | 34.12% | 14,042.77 | 48.42 | 188.79 | 3.15 hrs |
| **Level 2** | 287 | 33.76% | 14,208.72 | 49.51 | 183.00 | 3.05 hrs |
| **Level 1** | 273 | 32.12% | 12,662.69 | 46.38 | 187.16 | 3.12 hrs |

> [!WARNING]
> **Major Operational Bottleneck:** The average duration at **DC Fast chargers (188.79 mins / 3.15 hours)** is nearly identical to **Level 1 (187.16 mins)** and **Level 2 (183.00 mins)**. Because DC Fast chargers can deliver 50 kWh in 30–45 minutes, drivers are parking at fast stalls for **2 to 2.5 hours after charging has completed**. This severe "stall hogging" directly degrades network availability.

---

## 3. Spatial and Location Demand Breakdown

Stations span five distinct commercial and infrastructural settings:

| Station Location | Session Count | Energy Share (%) | Total Energy (kWh) | Avg Energy (kWh) | Avg Duration (Mins) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Highway** | 181 | 20.98% | 8,582.24 | 47.42 | 192.01 |
| **Transit Hub** | 172 | 20.66% | 8,454.50 | 49.15 | 168.45 |
| **Mall** | 170 | 21.15% | 8,655.25 | 50.91 | 201.69 |
| **Residential** | 167 | 19.70% | 8,061.90 | 48.27 | 182.53 |
| **Office District** | 160 | 17.50% | 7,160.29 | 44.75 | 186.69 |

- **Malls** deliver the highest energy per session (**50.91 kWh**) and the highest dwell times (**201.69 mins / 3.36 hrs**), matching retail and leisure behavior.
- **Transit Hubs** exhibit the shortest dwell times (**168.45 mins / 2.81 hrs**) and rapid vehicle turnover.
- **Highways** support the greatest session volume (**181 sessions**), indicating vital en-route demand.

---

## 4. Station Utilization Rankings

### Top 5 Stations by Session Count
1. **ST028 (Highway):** 30 sessions | 1,172.70 kWh delivered | Avg: 39.09 kWh | Avg Duration: 190.60 mins
2. **ST019 (Mall):** 26 sessions | 1,144.67 kWh delivered | Avg: 44.03 kWh | Avg Duration: 177.42 mins
3. **ST022 (Transit Hub):** 24 sessions | 1,179.97 kWh delivered | Avg: 49.17 kWh | Avg Duration: 215.79 mins
4. **ST004 (Transit Hub):** 24 sessions | 1,056.05 kWh delivered | Avg: 44.00 kWh | Avg Duration: 202.96 mins
5. **ST041 (Highway):** 24 sessions | 969.64 kWh delivered | Avg: 40.40 kWh | Avg Duration: 169.92 mins

### Top 5 Stations by Total Energy Delivered
1. **ST049 (Residential):** 1,217.68 kWh | 22 sessions | Avg: 55.35 kWh | Avg Duration: 157.68 mins
2. **ST035 (Transit Hub):** 1,208.01 kWh | 19 sessions | Avg: 63.58 kWh | Avg Duration: 205.00 mins
3. **ST022 (Transit Hub):** 1,179.97 kWh | 24 sessions | Avg: 49.17 kWh | Avg Duration: 215.79 mins
4. **ST028 (Highway):** 1,172.70 kWh | 30 sessions | Avg: 39.09 kWh | Avg Duration: 190.60 mins
5. **ST019 (Mall):** 1,144.67 kWh | 26 sessions | Avg: 44.03 kWh | Avg Duration: 177.42 mins

### Utilization Disparity
The top station (**ST028**, 30 sessions) processes almost **4x** the volume of the least utilized station (**ST009**, Residential, 8 sessions, 414.76 kWh).

---

## 5. Temporal Demand Patterns: Peak Hours & Weekdays

### Hourly Peak Dynamics
- **Peak Charging Hours:** **07:00 (42 sessions)** and **09:00 (42 sessions)** form an exact tie, driven by morning commute arrivals.
- **Secondary Shoulder Peaks:** **23:00 (41 sessions)** and **20:00 (40 sessions)** indicate evening home arrival and overnight charging initiation.
- **Off-Peak Trough:** **00:00 (27 sessions)** and **02:00 (28 sessions)**.

### Weekly Demand Dynamics
- **Busiest Day:** **Wednesday (130 sessions / 15.29%)** followed by **Thursday (126 sessions / 14.82%)** and **Sunday (125 sessions / 14.71%)**.
- **Identified Weekday Ties:** **Friday and Saturday** tied exactly at **119 sessions each**.
- **Quiet Day:** **Tuesday (113 sessions / 13.29%)**.

---

## 6. Statistical Duration Analysis & IQR Outlier Detection

### NumPy Statistical Summary of Charging Duration
- **Mean Duration:** 186.31 minutes (3.11 hours)
- **Median Duration:** 184.00 minutes (3.07 hours)
- **90th Percentile (P90):** 320.00 minutes (5.33 hours)

### IQR Outlier Detection Method
The Interquartile Range (IQR) method detects extreme values via the formula:
$$\text{IQR} = Q_3 - Q_1$$
$$\text{Upper Threshold} = Q_3 + 1.5 \times \text{IQR}$$
$$\text{Lower Threshold} = Q_1 - 1.5 \times \text{IQR}$$

**Empirical Computations:**
- $Q_1$ (25th Percentile): **105.25 minutes**
- $Q_3$ (75th Percentile): **268.00 minutes**
- $\text{IQR}$: **162.75 minutes**
- $\text{Upper Threshold}$: **512.12 minutes (8.54 hours)**
- $\text{Lower Threshold}$: **-138.88 minutes**

### Outlier Result
- **Statistical Outliers Flagged (> 512.12 mins):** **0 sessions**.
- **Explanation:** The maximum session duration in the dataset is **358.00 minutes (5.97 hours)**. Because all session durations fall beneath the mathematical threshold of 512.12 minutes, no points qualify as statistical outliers under the standard $1.5 \times \text{IQR}$ rule.
- **Operational Reality:** Although not statistical anomalies, **85 sessions (10%)** exceed the 90th percentile threshold of **320.00 minutes (5.33 hours)**, demonstrating persistent stall occupation.

---

## 7. Duration Group Breakdown

Sessions were grouped into three operational duration tiers:

| Duration Tier | Definition & Rationale | Session Count | Share (%) | Mean Energy (kWh) |
| :--- | :--- | :---: | :---: | :---: |
| **Short** | **< 120 mins (< 2 hours):** Fast top-ups, brief convenience stops | 255 | 30.00% | 48.09 kWh |
| **Medium** | **120 to < 240 mins (2–4 hours):** Standard dwell for shopping, dining, meetings | 313 | 36.82% | 48.29 kWh |
| **Long** | **>= 240 mins (>= 4 hours):** Work shifts, park-and-ride, overnight parking | 282 | 33.18% | 48.00 kWh |

Notably, average energy delivered remains virtually identical (~48 kWh) across all duration groups, confirming that long-duration parkers are **idle** rather than actively drawing high power.

---

## 8. Five Practical Recommendations to Improve Station Availability

Based strictly on empirical evidence from this dataset, the following five actions will directly optimize charging station throughput and availability:

### 1. Introduce Progressive Idle Fees on DC Fast Chargers
- **Data Finding:** Average dwell time at DC Fast chargers is **188.79 minutes (3.15 hours)**, identical to Level 1 (187.16m) and Level 2 (183.00m), despite delivering equivalent energy (~48.4 kWh) that requires only 30–45 minutes of fast-charge current.
- **Action:** Enforce a post-charge grace period (10 minutes) followed by an idle fee of **$0.75/minute**. This will discourage drivers from using DC Fast stalls as free parking and recover up to 2 hours of available charger time per session.

### 2. Implement Time-of-Use (TOU) Dynamic Tariffs to Flatten Morning Peaks
- **Data Finding:** Demand surges at **07:00 and 09:00 (tied at 42 sessions each)** and mid-week on **Wednesday (130 sessions)** and **Thursday (126 sessions)**, while overnight hours (00:00–03:00) drop to 27–36 sessions.
- **Action:** Establish a peak-hour premium tariff between 07:00–10:00 AM on weekdays and offer discounted off-peak charging overnight (11:00 PM – 06:00 AM). This price signal will shift discretionary charging to off-peak slots and relieve commute congestion.

### 3. Match Hardware Specification to Location Dwell Profiles
- **Data Finding:** Mall locations experience the longest dwell times (**201.69 minutes / 3.36 hours**) and highest energy per session (**50.91 kWh**), while Highway locations process the highest volume (**181 sessions**) but suffer from 192-minute dwell times.
- **Action:** Reallocate charger deployments: upgrade Highway stations (e.g., ST028) with high-power 150kW+ DC Fast dispensers to reduce highway stop times to under 30 minutes, while deploying high-density, lower-cost Level 2 dispensers at Malls and Office Districts where multi-hour parking is natural.

### 4. Deploy Dynamic Load-Balancing & Mobile Wayfinding for Congested Stations
- **Data Finding:** Extreme utilization inequality exists: top stations like **ST028** (Highway, 30 sessions, 1,172 kWh) and **ST022** (Transit Hub, 24 sessions, 1,180 kWh) experience high traffic, while bottom stations like **ST009** (Residential, 8 sessions) and **ST021** (Office, 10 sessions) sit largely idle.
- **Action:** Integrate real-time occupancy monitoring into mobile navigation apps to actively steer arriving drivers away from full hubs (ST028/ST022) to adjacent underutilized stalls (e.g., ST001, ST005), offering slight charging credits for shifting stations.

### 5. Establish Designated Extended-Dwell Zones for Long-Duration (>4h) Parkers
- **Data Finding:** One-third of all sessions (**33.18% / 282 sessions**) are **Long Duration (≥ 4 hours)**, extending up to 358 minutes, with the 90th percentile reaching **320.00 minutes (5.33 hours)**.
- **Action:** At Transit Hubs and Residential lots, physically divide parking areas into "Express Charging" (max 60 mins) and "Extended Dwell" (overnight/workday Level 2) zones with automated power throttling. This preserves rapid-turnover bays for drivers needing quick recharges without stranding long-dwell commuters.

---

## 9. Visualizations Generated

The analysis generated five high-resolution charts saved in the project folder:
1. `sessions_by_hour.png`: Hourly session distribution with 07:00 & 09:00 peaks highlighted.
2. `sessions_by_weekday.png`: Weekly session breakdown highlighting Wednesday and weekday ties.
3. `station_rankings.png`: Top 10 EV charging stations ranked by total energy delivered (kWh).
4. `energy_by_location.png`: Dual-axis comparison of total and average energy by location.
5. `duration_group_counts.png`: Bar chart of Short, Medium, and Long duration groups with percentage shares.

---
*Report automatically generated by `ev_charging_analysis.py`.*
