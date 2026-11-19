# Dog-Sitting Operations & Scheduling Analysis

## Overview
An operational data analysis project examining scheduling bottlenecks, time-block delays, workload pacing, and effective hourly earnings across a 90-day dog-sitting dataset (335 total visits across 8 pets from August 1 to October 29, 2026).

## Data Model
Structured as a two-table relational schema queried in SQLite:
- **`pets`**: Pet attributes (`pet_id`, `dog_name`, `breed`, `size_category`, `energy_level`, `requires_meds`, `base_visit_rate`)
- **`care_logs`**: Visit records (`log_id`, `visit_date`, `time_block`, `pet_id`, `activity_type`, `duration_minutes`, `delay_minutes`)

## Key Findings

### 1. Morning Window Drives Schedule Delays
- **Morning (7–10 AM)** logged **133 visits** with a **46.6% delay rate** and **6.5 average delay minutes**.
- **Evening (5–8 PM)** had **123 visits** with an **18.7% delay rate** (**2.3 avg delay mins**).
- **Midday (11 AM–2 PM)** had **79 visits** with a **15.2% delay rate** (**2.2 avg delay mins**).

### 2. High-Energy Large Dogs Require More Time Per Dollar
- **Large / High-Energy** visits averaged **48.1 minutes** across **160 visits**, resulting in an effective rate of **$59.39/hr**.
- **Medium / Moderate-Energy** visits averaged **32.4 minutes** (**80 visits**) at **$73.86/hr**.
- **Small / Moderate-Energy** visits averaged **29.6 minutes** (**42 visits**) at **$80.70/hr**.
- **Small / Low-Energy** visits averaged **29.1 minutes** (**53 visits**) at **$87.48/hr**.

### 3. Workload Spikes & Rolling Capacity
- High-intensity days reached **237–245 active care minutes** (e.g., 6 visits / 237 mins on `2026-08-01` and 6 visits / 245 mins on `2026-09-11`).
- Tracking a 7-day rolling average via SQL window functions helps spot sustained multi-day workload pressure early (`237.0 mins` 7-day rolling average on `2026-08-01`).

## Recommendations
- **Buffer morning schedules:** Add transition buffers or cap back-to-back high-delay morning bookings to reduce the 46.6% morning delay rate.
- **Align pricing with duration & effort:** Review flat visit pricing for long, high-energy large-dog visits that lower effective hourly earnings compared to shorter low-energy visits.
- **Monitor rolling weekly minutes:** Use a 7-day rolling minute threshold to pace bookings during peak windows.

## Tools Used
- Python (`pandas`, `numpy`, `sqlite3`, `matplotlib`, `seaborn`)
- SQL (`JOIN`, CTEs, aggregate functions, window functions)
