# 🦠 COVID-19 Data Analytics Project

> A comprehensive SQL-based analysis of global COVID-19 data, highlighting case and death trends, infection rates, and vaccination rollouts. Data was visualized using Tableau for global insights across countries and continents.

---

## 1. Overview

This project explores global COVID-19 data using SQL and Tableau to uncover trends in infections, fatalities, and vaccinations. The goal is to identify global and regional patterns that can support policy response, public awareness, and health resource allocation.

The analysis combines two datasets — `coviddeaths` and `covidvaccinations` — and utilizes CTEs, window functions, temp tables, and joins to derive insights, later visualized through an interactive Tableau dashboard.

---

## 2. Business Objectives

- Track the spread of COVID-19 over time by country and continent
- Compare death rates and infection percentages across regions
- Understand the global vaccination rollout and its population coverage
- Build a dashboard that supports both descriptive and comparative analysis

---

## 3. Tech Stack

The dashboard was built using the following tools and technologies:

- MySQL Workbench – Main platform for writing and executing complex queries  
- Tableau Public – Used for building interactive dashboards  
- Data Transformation – SQL-based NULL handling, formatting, joins, and CTEs  
- Window Functions – Applied to track rolling vaccination counts per country  
- View & Temp Table Creation – For storing intermediate results  
- Export Format – Tableau Public for full dashboard + `.png` for preview

---

## 4. Dataset Description

- Source: [Our World in Data – COVID-19](https://ourworldindata.org/coronavirus)  
- Tables:
  - `coviddeaths`: contains case, death, population, and hospitalization data
  - `covidvaccinations`: contains vaccination and testing data
- Time Range: Jan 2020 – April 2021  
- Coverage: Over 100 countries

---

## 5. Features / Highlights

### • Business Problem

COVID-19 presented a global crisis where governments, researchers, and citizens needed up-to-date data to make informed decisions. However, the raw datasets were complex and required significant preprocessing.

This project aims to transform raw global COVID-19 data into a clean, insightful dashboard that compares infection and death rates across time and geography while tracking vaccination progress.

### • Goal of the Dashboard

To build a global-level dashboard that:

- Highlights countries with the highest infections and death rates  
- Visualizes infection rate per population over time  
- Compares continent-level deaths  
- Displays vaccination rollout globally

### • Walkthrough of Key Visuals

- Global KPIs: Total Cases, Total Deaths, and Global Death Rate  
- Map of Infection Rates per Country: Percent of infected population visualized geographically  
- Deaths per Continent (Bar Chart): Comparison of fatality levels across continents  
- Infection Growth Over Time (Line Chart): Avg. % of population infected by country from March 2020 to March 2021

### • Business Impact & Insights

- Europe and North America saw the highest cumulative death counts  
- Belgium and the UK had among the highest infection rates per capita  
- Countries like Cambodia and Afghanistan had notably low infection percentages  
- Vaccination rollout was uneven, highlighting disparities across regions  
- The dashboard can inform international comparisons and help monitor future health crises

---

## 6. SQL Highlights

Key SQL techniques used:

- Data cleaning using `NULLIF`, `CAST`, and `STR_TO_DATE`  
- CTEs and `PARTITION BY` window functions for rolling vaccination tracking  
- Temporary tables and views for modular analysis  
- Aggregations and conditionals to track infection and mortality rates

Full SQL workflow included in project repository.

---

## 7. Tableau Dashboard

[View Full Dashboard on Tableau Public](https://public.tableau.com/views/CovidProject_17516757694200/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

### Snapshot

Below is a snapshot of the Tableau dashboard:

![COVID-19 Tableau Dashboard](https://raw.githubusercontent.com/annievu22/Covid_Project/main/Covid%20Project%20-%20Tableau%20Snapshot.png)

---

## 8. Final Conclusion

This project demonstrates how structured SQL queries and well-designed dashboards can transform raw pandemic data into meaningful, accessible insights. From formatting dates and filling NULLs to performing joins and rolling aggregates, each SQL step helped uncover key global trends.

The resulting Tableau dashboard provides a quick yet comprehensive view of global COVID-19 impacts, from infections to deaths to vaccinations. It reveals regional disparities, health system challenges, and the scale of the pandemic.

In future iterations, this project could be enhanced by:

- Integrating mobility or policy data to contextualize spikes  
- Including vaccination manufacturer breakdowns  
- Applying machine learning for trend forecasting  

Overall, this project reflects a strong foundation in SQL data transformation, global health analytics, and effective data storytelling.

---

## 9. Repository Structure

