# AeroOps — Flight Operations Intelligence Dashboard

An interactive single-page HTML dashboard that presents the findings from the Flight Delay & Cancellation Prediction project as a fully operational analytics platform. It is designed to simulate a real-time airline operations control centre, complete with live weather overlays, ML model metrics, and multi-airline performance tracking.

---

##  File

`Flight_dashboard.html`

---

##  How to Open

No installation or server required. Simply open `Flight_dashboard.html` in any modern web browser (Chrome, Firefox, Edge, Safari).

---

##  Dashboard Overview

### Live Status Bar
Displayed at the top of every panel, the status bar shows a real-time ticker of airline on-time performance rates with directional trend indicators:

| Airline | On-Time % | Trend |
|---------|-----------|-------|
| Alaska (AS) | 85.9% | ▲ +2.2% |
| Delta (DL) | 84.1% | ▲ +0.8% |
| Southwest (WN) | 81.7% | ▲ +0.3% |
| United (UA) | 79.3% | ▼ −1.5% |
| American (AA) | 76.2% | ▼ −2.1% |
| JetBlue (B6) | 73.4% | ▼ −3.1% |

Hub-level alerts are also shown for average delay (ORD: 18.4 min, ATL: 11.2 min), active weather events (SFO: Low Visibility), and taxi-out times (DFW: 24.7 min).

---

##  Navigation Panels

###  Overview
Network-level operations summary for the full 2015 BTS domestic dataset.

**Key Metrics:**
| Metric | Value | Trend |
|--------|-------|-------|
| On-Time Performance | 80.4% | ▲ +1.2% vs last week |
| Delay Rate | 19.6% | ▼ −1.2% vs last week |
| Avg Arrival Delay | 11.5 min | ▲ +0.8 min vs last week |
| Cancellation Rate | 1.68% | ▲ +0.12% vs last week |
| Flights Operated | 5.82M | YTD · 2015 BTS Data |

**Charts included:**
- Monthly delay rate trend (% flights with arrival delay > 15 min)
- Delay cause breakdown (distribution of delay minutes by cause)
- Day × departure-period heatmap of delay rates
- Airline performance scatter: mean vs 90th-percentile arrival delay (bubble size = flight volume)

**Weather Impact Panel** shows live conditions at major hubs:
- ATL ☀️ 72°F · Clear → Low Impact
- ORD 🌧️ 51°F · Rain → High Impact
- DFW ⛅ 68°F · Partly Cloudy → Low Impact
- DEN 🌨️ 28°F · Snow → High Impact
- LAX 🌫️ 65°F · Fog → Medium Impact
- JFK ☀️ 58°F · Clear → Low Impact

---

###  Network Map
A US domestic airport operations map with colour-coded delay tiers.

**Delay Tier Legend:**
- 🔴 High Delay — > 20% of flights delayed
- 🟡 Med Delay — 15–20% of flights delayed
- 🟢 Low Delay — < 15% of flights delayed

**Also includes:**
- **Top 10 Busiest Routes** by flight volume with average delay displayed per route
- **Airport Delay Ranking** — average arrival delay in minutes for the 15 worst-performing airports
- **Route Lookup** — search a specific origin–destination pair for performance stats

---

###  Reliability
Airline-level performance, cancellation analysis, and SLA tracking.

**Summary KPIs:**
| Metric | Value |
|--------|-------|
| Best On-Time Performance | Alaska (AS) — 85.9% ▲ |
| Worst On-Time Performance | JetBlue (B6) — 73.4% ▼ |
| Avg Cancellation Rate | 1.68% ▲ +0.12% |
| Diverted Flights | 0.21% (Stable) |
| SLA Compliance | 94.7% ▲ +0.5% |

**Charts included:**
- **Airline On-Time Performance Ranking** — % of flights with arrival delay ≤ 15 min
- **Cancellation Reason Distribution** — breakdown by cause code: A (Carrier), B (Weather), C (NAS), D (Security)
- **Tail Delay (90th Percentile) by Airline** — worst-case delay experience for passengers
- **Full Airline Reliability Matrix** — sortable table with columns: Flights, On-Time %, Avg Delay, 90th Pct, Cancel %, Status

---

###  ML Model Intelligence
Displays the trained model metrics from the Flight Delays notebook, providing a real-time model performance view.

**Regression Models (Arrival Delay in Minutes):**

| Model | MAE | RMSE | R² | Stability | Latency |
|-------|-----|------|----|-----------|---------|
| XGBoost Regressor | 8.42 min | 18.71 min | 0.631 | 0.847 | 42 ms |
| TFT (Temporal Fusion) | 9.83 min | 21.14 min | 0.594 | 0.921 | 217 ms |
| GRU Network | 10.21 min | 22.47 min | 0.571 | 0.889 | 184 ms |

**Classification Models (Flight Cancellation — Binary):**

| Model | PR-AUC | F1 Score | Brier Score | Stability | Latency |
|-------|--------|----------|-------------|-----------|---------|
| XGBoost Classifier | 0.7821 | 0.6904 | 0.0432 | 0.812 | 38 ms |

**Radar Charts:** Normalised multi-metric comparison charts (0–1 scale, outward = better) for both regression and classification model families.

---

### $ Revenue Impact
Estimates the financial cost of delays and cancellations, translating ML predictions into operational dollar impact.

---

###  Alerts
Active operational alerts panel — 3 alerts flagged at time of snapshot — including weather events, high-delay airport warnings, and system data lag status (1.2 s ✓).

---

###  Architecture
System architecture overview showing the data pipeline from source to dashboard:

| Data Source | Description |
|-------------|-------------|
| **FAA SWIM** | Real-time airspace data feed (simulated overlay) |
| **METAR/TAF** | Meteorological Aerodrome Reports and Terminal Area Forecasts |
| **AIP Feed** | Aeronautical Information Publication |
| **Kaggle BTS** | 2015 Bureau of Transportation Statistics domestic flight data |

Also includes a **Runbook** tab and **RBAC Settings** for role-based access control configuration.

---

##  Technical Details

| Property | Value |
|----------|-------|
| Type | Single-page HTML (self-contained) |
| Dependencies | None — no external server required |
| Data | 2015 BTS Domestic Flights · 5.8M flights |
| Refresh | Simulated live overlays with static BTS dataset base |
| Compatibility | Chrome, Firefox, Edge, Safari |
