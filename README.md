# IPL-Data-Analysis-Report
An interactive Power BI dashboard for IPL Data Analysis, featuring season filters, player and team performance insights, top run scorers, wicket takers, and trend visualizations. Built using Power BI and Power Query to showcase actionable insights from sports datasets.
---

## Table of Contents

1. [Project Overview] (#project-overview)
2. [What's included / Key features] (#whats-included--key-features)
3. [Data sources & structure] (#data-sources--structure)
4. [Data preparation & modeling (Power Query + data model)] (#data-preparation--modeling-power-query--data-model)
5. [Key metrics & sample DAX measures] (#key-metrics--sample-dax-measures)
6. [Report pages & how to use the dashboard] (#report-pages--how-to-use-the-dashboard)
7. [How to open / requirements] (#how-to-open--requirements)
8. [Folder structure (recommended)] (#folder-structure-recommended)
9. [Known issues & troubleshooting] (#known-issues--troubleshooting)
10. [Customizing & updating the dataset] (#customizing--updating-the-dataset)
11. [Contributing] (#contributing)
12. [License & contact] (#license--contact)

---

## Project Overview

This repository contains the **IPL Data Analysis Report** created in Power BI (.pbix). The interactive report visualizes match-level and player-level IPL statistics across seasons to discover trends, compare teams, identify top performers, and examine match-level details.

The dashboard is designed for analysts, fans, and anyone who wants to explore IPL performance metrics and season-by-season trends through an interactive and story-driven visualization.

---

## What's included / Key features

* Interactive, filterable Power BI report (.pbix) with multiple report pages.
* Season and team slicers to quickly filter visuals across the report.
* Visualizations for top run scorers, top wicket takers, strike rates, economy rates, averages, and form/trend charts.
* Team performance comparison (wins, losses, net run rate, head-to-head comparisons).
* Drill-through pages for match and player details (match summary, player performance in a match).
* Tooltips and hover cards for quick insights.
* Data transformation performed using Power Query; calculations implemented as DAX measures.

---

## Data sources & structure

> **Note:** Replace or update these files with the actual datasets you used.

Typical dataset files used with this report (CSV / Excel):

* `matches.csv` — match-level info (match id, date, teams, venue, result, season, winner, toss, etc.)
* `deliveries.csv` — ball-by-ball delivery data (match id, inning, over, ball, batsman, bowler, runs, extras, wicket info)
* `players.csv` / `teams.csv` — optional metadata about players and teams

The data model links `matches` to `deliveries` (one-to-many) via `match_id`, with lookup tables for players/teams where applicable.

---

## Data preparation & modeling (Power Query + data model)

Key transformation steps typically performed in Power Query:

1. **Import**: Load CSV/Excel tables into Power BI.
2. **Cleansing**: Remove duplicates, trim whitespace, standardize team and player names, handle missing values.
3. **Date**: Parse and create proper `Date` and `Season` columns for time-intelligence visuals.
4. **Merge / Expand**: Join match-level and delivery-level tables where needed (e.g., to attach match metadata to delivery rows).
5. **Calculated columns**: Create helper columns such as `RunsScored`, `IsWicket`, `IsExtra`, or `BallNumber`.
6. **Star schema**: Design a clean model — fact table (`deliveries`/`events`) and dimension tables (`matches`, `players`, `teams`, `venues`).

Modeling tips:

* Avoid bi-directional relationships unless necessary.
* Use integers/IDs for joins and keep text columns for display only.
* Add season and year columns for easy slicers.

---

## Key metrics & sample DAX measures

Below are common metrics used in the report. Column names are illustrative — adjust to your dataset's actual column names.

```dax
-- Total runs scored in the dataset
Total Runs = SUM(Deliveries[Runs])

-- Total wickets (counting only rows where a wicket occurred)
Total Wickets = COUNTROWS(FILTER(Deliveries, NOT(ISBLANK(Deliveries[WicketKind]))))

-- Balls faced by a batsman (example aggregation on deliveries table)
Balls Faced = COUNTROWS(FILTER(Deliveries, Deliveries[IsDeliveryToBatsman] = TRUE))

-- Strike Rate (batting)
Strike Rate = DIVIDE([Total Runs], [Balls Faced]) * 100

-- Batting Average
Batting Average = DIVIDE([Total Runs], [Dismissals])

-- Economy Rate (bowling)
Economy Rate = DIVIDE([Runs Conceded], [Overs Bowled])
```

**Notes:**

* Use `DIVIDE()` to safely handle division-by-zero.
* `Dismissals`, `Runs Conceded`, and `Overs Bowled` may be calculated measures that depend on your data model.

---

## Report pages & how to use the dashboard

**Typical pages included:**

1. **Overview / Executive Summary** — Season filter, KPIs (Total Matches, Total Runs, Average Runs per Match, Top Player of Season), and sparkline trends.
2. **Team Comparison** — Win/Loss summary, Net Run Rate trend, head-to-head.
3. **Top Players** — Leaderboards for runs, wickets, strike rate, averages (with dynamic season filter).
4. **Match Details** — Select a match to view innings summary, scorecards, and ball-by-ball highlights.
5. **Player Profile** — Select a player to see career stats, season-wise performance, and form charts.
6. **Venue / Stadium Analysis** — Average scores by ground, boundary factors, and venue-specific stats.
7. **Appendix / Data Model** — Visual of the relationships and data source notes.

**Using the report:**

* Use the **Season** and **Team** slicers at the top to filter visuals across pages.
* Click a player or team in a visual to cross-filter related charts.
* Right-click visuals for **drill-through** to detailed pages (e.g., match-level drill-through).
* Hover over bars/points for tooltips with extra context.

---

## How to open / requirements

* **Power BI Desktop** (recommended: latest stable release). Open the `.pbix` file in Power BI Desktop to view and interact with the report.
* If publishing to Power BI Service, use an account with publishing permissions (Power BI Pro recommended for sharing).

---

* **Slicer color / visibility issues**: If slicer option background and font are the same color (e.g., white on white), open the Format pane for the slicer and adjust `Items` and `Selection controls`, or apply a report theme. You can also edit the report theme JSON for consistent text/background colors.
* **Missing visuals after refresh**: Check queries in Power Query for changed column names or schema changes in source files.
* **Slow refresh / large dataset**: Consider aggregating historical data, limiting rows, or enabling query folding where possible.

---

## Customizing & updating the dataset

1. Replace CSV files in `/data` with updated versions (maintain same column names), then open the `.pbix` and click **Refresh**.
2. If column names changed, adjust Power Query transformation steps accordingly.
3. Re-publish to Power BI Service if you maintain a live dashboard for viewers.

---
*If you'd like, I can:*

* Convert this into a `README.md` file and place example screenshots into `/assets/screenshots`.
* Add explicit DAX measure implementations using your actual column names if you share the column names or the exported CSVs.

*Last updated:* 2025-09-24
