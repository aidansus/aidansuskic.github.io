# 📊 Recording Academy Digital Ecosystem Analysis & Domain Split Strategy

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458.svg)](https://pandas.pydata.org/)
[![Plotly](https://img.shields.io/badge/Plotly-Interactive%20Viz-3F4F75.svg)](https://plotly.com/)

An end-to-end web analytics pipeline and comparative study evaluating user traffic behavior, engagement metrics, and session performance across `grammy.com` and `recordingacademy.com` following a major domain migration.

---

## 📌 Business Overview & Objective

When **The Recording Academy** separated its single web property into two distinct domains—`grammy.com` (fan-facing event content) and `recordingacademy.com` (institutional and membership operations)—executives required empirical validation of the shift.

This project analyzes daily web analytics time-series data from both properties to evaluate:
1. **Audience Segmentation:** Did splitting domains successfully segregate high-intent industry users from casual event consumers?
2. **Event Traffic Volatility:** How severe are traffic surges during peak event cycles (*Awards Week* and *Awards Night*)?
3. **Engagement & Retention Performance:** How do bounce rates and average session lengths differ between consumer and B2B/institutional platforms?

---

## 🛠️ Tech Stack & Methodology

* **Data Wrangling & Pipeline:** Python, Pandas, NumPy
* **Data Visualization:** Plotly Express (Interactive time-series, distribution plots, comparative visual overlays)
* **Analytical Techniques:** Time-Series Segmentation, Event-Based Metric Normalization, Behavioral Funnel & Bounce Analysis

---

## 📁 Repository Structure

```text
├── data/
│   ├── grammy_live_web_analytics.csv     # Event-focused web traffic dataset
│   └── ra_live_web_analytics.csv         # Institutional web traffic dataset
├── notebooks/
│   └── domain_split_analysis.ipynb       # Main EDA, ETL, and visualization pipeline
├── .gitignore
├── README.md
└── requirements.txt
