# Analyzing Electric Vehicle Charging Habits 🚗⚡

## Overview

As Electric Vehicles (EVs) continue to gain popularity, the demand for reliable charging infrastructure is increasing rapidly. In residential apartment buildings, shared EV charging stations are becoming more common, but limited access often creates competition among tenants.

This project analyzes EV charging session data from apartment buildings to help property managers understand tenant charging behavior, optimize station availability, and improve resident satisfaction.

Using SQL and PostgreSQL, I explored user activity patterns, peak charging periods, garage utilization, and long-duration charging sessions.

---

## Objectives

The key goals of this analysis were to:

* Identify garages with the highest number of unique shared charging users
* Discover the most popular days and times for shared charging sessions
* Detect users with unusually long charging durations
* Provide insights that can help apartment managers improve charging access and planning

---

## Dataset Information

The dataset was stored in a PostgreSQL table named `charging_sessions`.

### Table: `charging_sessions`

| Column            | Description                     |
| ----------------- | ------------------------------- |
| garage_id         | Building / garage identifier    |
| user_id           | Individual user identifier      |
| user_type         | Shared or Private charger       |
| start_plugin      | Charging session start datetime |
| start_plugin_hour | Session start hour              |
| end_plugout       | Charging session end datetime   |
| end_plugout_hour  | Session end hour                |
| duration_hours    | Charging duration in hours      |
| el_kwh            | Electricity consumed (kWh)      |
| month_plugin      | Month session started           |
| weekdays_plugin   | Day of week session started     |

---

## Tools Used

* PostgreSQL
* SQL
* Data Analysis
* GitHub

---

# Business Questions & SQL Analysis

---

## 1️⃣ Which garages have the highest number of shared charging users?

```sql
SELECT DISTINCT garage_id,
       COUNT(DISTINCT user_id) AS num_unique_users
FROM charging_sessions
WHERE user_type = 'Shared'
GROUP BY garage_id
ORDER BY num_unique_users DESC;
```

### Results

| Garage ID | Unique Shared Users |
| --------- | ------------------- |
| Bl2       | 18                  |
| AsO2      | 17                  |
| UT9       | 16                  |
| AdO3      | 3                   |
| MS1       | 2                   |
| SR2       | 2                   |
| AdA1      | 1                   |
| Ris       | 1                   |

### Insight

* **Bl2** has the highest number of shared charger users.
* **AsO2** and **UT9** also experience strong demand.
* These garages may require additional charging ports to reduce congestion.

---

## 2️⃣ What are the most popular shared charging start times?

```sql
SELECT DISTINCT weekdays_plugin,
       start_plugin_hour,
       COUNT(weekdays_plugin) AS num_charging_sessions
FROM charging_sessions
WHERE user_type = 'Shared'
GROUP BY weekdays_plugin, start_plugin_hour
ORDER BY num_charging_sessions DESC
LIMIT 10;
```

### Top Charging Times

| Day       | Hour  | Sessions |
| --------- | ----- | -------- |
| Sunday    | 17:00 | 30       |
| Friday    | 15:00 | 28       |
| Thursday  | 16:00 | 26       |
| Thursday  | 19:00 | 26       |
| Sunday    | 15:00 | 25       |
| Sunday    | 18:00 | 25       |
| Wednesday | 19:00 | 25       |
| Friday    | 16:00 | 24       |
| Monday    | 15:00 | 24       |
| Saturday  | 17:00 | 23       |

### Insight

* **Late afternoons and evenings (3 PM – 7 PM)** are the busiest charging hours.
* **Sundays** show the highest charging demand.
* Property managers may consider reservation systems during these hours.

---

## 3️⃣ Which users occupy shared chargers for the longest average time?

```sql
SELECT user_id,
       AVG(duration_hours) AS avg_charging_duration
FROM charging_sessions
WHERE user_type = 'Shared'
GROUP BY user_id
HAVING AVG(duration_hours) > 10
ORDER BY avg_charging_duration DESC;
```

### Results

| User ID  | Avg Duration (hrs) |
| -------- | ------------------ |
| Share-9  | 16.85              |
| Share-17 | 12.89              |
| Share-25 | 12.21              |
| Share-18 | 12.09              |
| Share-8  | 11.55              |
| AdO3-1   | 10.37              |

### Insight

* Some users occupy chargers for **10+ hours on average**.
* Long-duration sessions reduce charger availability for others.
* Introducing idle fees or charging time limits may improve turnover.

---

# Key Findings

✅ Shared charging demand is concentrated in a few garages
✅ Peak demand occurs mainly during afternoons and evenings
✅ Sundays experience the highest usage volume
✅ A small number of users occupy chargers for extended periods

---

# Recommendations

### For Apartment Managers:

* Install more chargers in high-demand garages (**Bl2, AsO2, UT9**)
* Introduce booking or reservation systems during peak hours
* Apply time limits or idle fees after full charge
* Monitor user behavior regularly for optimization

---

# Project Structure

```bash
Analyzing-EV-Charging-Habits/
│── README.md
│── queries.sql
│── insights.md
```

---

# What I Learned

Through this project, I strengthened my skills in:

* SQL Aggregations
* Filtering with WHERE and HAVING
* Grouping and Sorting Data
* Turning raw data into business insights

---

# Author

**Jeremiah Kehinde**
Data Analyst | SQL Enthusiast | Turning Data into Decisions

---

# If you found this project interesting:

⭐ Star the repository
🍴 Fork the project
📩 Connect with me on GitHub
