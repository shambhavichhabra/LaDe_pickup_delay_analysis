# 📦 How Much Is Delay Costing You? A City-Level View of Courier Inefficiency

This project analyzes **pickup responsiveness** in last-mile logistics using the real-world LaDe dataset published by Alibaba’s Cainiao division.

It focuses on **real-time tasks** — courier pickups with a delay of 60 minutes or less — and measures how much time is lost after couriers accept a task but before they pick it up. This time gap, or **pickup delay**, is often overlooked but operationally critical.

Through threshold modeling, time-based performance breakdowns, and cost estimation, this project reveals where and when responsiveness breaks down — and how that inefficiency translates into thousands of minutes (and yuan) lost every day.

---

## 📂 Dataset Overview

- Source: [LaDe dataset](https://huggingface.co/datasets/Cainiao-AI/LaDe)
- Published by: Cainiao Network (Alibaba Group)
- Focus: Real-time pickup tasks across **five Chinese cities**
- Time window: Accept time, pickup time, location, and courier ID
- Filters: Only **pickup-related data** was used — not delivery timing or route optimization

Renamed columns for clarity:
- `pickup_time` ← original `got_time`
- `courier_id`, `city`, `accept_time`, and others standardized
  
---

## ❓ Business Questions

1. **How long do couriers wait between accepting and picking up tasks?**
2. **At what point does pickup delay become a problem?**
3. **How many tasks exceed that delay threshold — and in which cities?**
4. **How does performance vary by hour of day and day of week?**
5. **How much avoidable time is lost daily due to pickup delays?**
6. **Can we track or forecast responsiveness trends over time?**

---

## 🔍 Key Insights

- When converted to operational cost, excess pickup delays result in **thousands of RMB lost daily**.
  Using a conservative 0.1 RMB/min estimate, peak delay days cost **over 8,000 RMB**, with Shanghai,    Jilin, and Yantai leading in total loss.

  This reframes delay not just as inefficiency — but as measurable waste.

- **Average pickup delay** across all tasks: **~30.96 minutes**

- **Highest excess delay rate**:
  
   - **Jilin** → ~73% of tasks exceeded city threshold  
   - **Yantai** → ~66%

- **Thresholds were not fixed**, *which define what is considered excess and what is reasonable delay* — they were customized per city using a **tasks-per-courier ratio**.  
   - Cities with more courier workload were given higher delay tolerance.  
   - Example: Shanghai & Hangzhou → threshold = 30 mins; Jilin → threshold = 15 mins

- **Delays are time-sensitive**:  
   - Delay is **worst in early morning hours** (4–6 AM), peaking above **42 minutes**  
   - Delay drops significantly during mid-day hours

- **Delay is high every day**:  
   - 50–55% of tasks exceed their city-specific threshold **on every single weekday**

- **Lost time adds up fast**:  
   - On peak days, excess delay reaches **over 80,000 minutes lost in a single day**


- The 7-day rolling average of pickup delay shows a **slow upward trend** — While this isn't a forecast, it suggests a potential deterioration in responsiveness — although more data is needed to confirm whether this is part of a longer-term cycle or a short-term anomaly.
 
---

## ✅ Recommendations

- **Use city-specific pickup delay benchmarks instead of one-size-fits-all thresholds**

In most operations, performance is judged using blanket SLAs (e.g., “pickup must happen within 15 minutes”). But this approach ignores the structural differences across cities.

In this project, I designed **custom thresholds** based on the actual task-per-courier load in each city. For example:

- Cities with fewer couriers but higher workload (e.g., Shanghai) naturally experience longer pickup delays — not because of inefficiency, but because couriers have more to do.
- Judging such cities against a flat 15-minute benchmark would unfairly penalize them.

By assigning higher thresholds to high-load cities, and stricter ones to low-load cities, we get a fairer and more realistic measure of performance. This makes the analysis more tailored, and the insights more trustworthy for decision-making.

- **Address early-morning pickup delays more strategically**

The biggest pickup delays occur between 4:00–6:00 AM, with average delays exceeding 40+ minutes.
These early hours likely represent urgent or time-sensitive requests — people don't schedule pickups at 4 AM unless it matters. However, courier availability appears limited at that time.
This can be improved by:
1. Offering incentives or surge pay to couriers during off-hours
2. Testing higher delivery pricing tiers to offset cost of faster night delivery
3. Implementing a "priority mode" for critical pickups, where users can optionally pay more for faster dispatch from farther couriers.

Solving early-morning lag isn't just about speed — it improves trust in critical moments.

- **Create dynamic alert thresholds for daily monitoring**

Excess delay is a signal.
When over 50% of tasks exceed thresholds on any given day, that day should be flagged as an operational risk.
A dynamic alerting system that triggers when excess delay exceeds a moving baseline (e.g., 50% of tasks) can help ops teams preemptively adjust staffing or incentives.
This is especially useful for sudden demand surges (weekends, weather spikes) where manual oversight may lag.

- **Monitor delay trends using rolling averages — even without forecasting**

Our dataset is too short for reliable ARIMA/Holt-Winters forecasting.
But monitoring the 7-day rolling average of pickup delays still offers huge value.
For instance, the rolling average shows a rising trend in delays — this should be a warning flag.
With this rolling view, companies can:
1. Adding staff during pressure periods
2. Temporarily adjusting SLAs
3. Offering incentives or discounts to shift demand
4. If the trend levels off or reverses, it also helps evaluate the impact of interventions.

- **Investigate why Jilin and Yantai underperform despite low workloads**
  
Jilin and Yantai have the lowest task-per-courier ratios — 5.0 and 13.0 respectively — but also the highest rates of delay (Jilin: 73% over threshold).
That’s counterintuitive. In high-load cities like Shanghai (19 tasks/courier), delays are lower.
This anomaly suggests that the problem in Jilin and Yantai is not volume, but potentially courier inefficiency, routing issues, uneven courier distribution (i.e., supply exists, but not where demand is)
These cities need targeted diagnostics, not just more couriers. They’re outliers — and that’s a clue worth exploring.

---

*This project combines data cleaning, timestamp parsing, business metric engineering, and time-based performance analysis using Python, pandas, seaborn and statsmodels.*

---
