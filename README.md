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

## 3. Dataset Description

- Source: [Our World in Data – COVID-19](https://ourworldindata.org/coronavirus)  
- Tables:
  - `coviddeaths`: contains case, death, population, and hospitalization data
  - `covidvaccinations`: contains vaccination and testing data
- Time Range: Jan 2020 – April 2021  
- Coverage: Over 100 countries

---

## 4. Features / Highlights

### 4.1. Business Problem

COVID-19 presented a global crisis where governments, researchers, and citizens needed up-to-date data to make informed decisions. However, the raw datasets were complex and required significant preprocessing.

This project aims to transform raw global COVID-19 data into a clean, insightful dashboard that compares infection and death rates across time and geography while tracking vaccination progress.

### 4.2. Goal of the Dashboard

To build a global-level dashboard that:

- Highlights countries with the highest infections and death rates  
- Visualizes infection rate per population over time  
- Compares continent-level deaths  
- Displays vaccination rollout globally

### 4.3. Walkthrough of Key Visuals

- Global KPIs: Total Cases, Total Deaths, and Global Death Rate  
- Map of Infection Rates per Country: Percent of infected population visualized geographically  
- Deaths per Continent (Bar Chart): Comparison of fatality levels across continents  
- Infection Growth Over Time (Line Chart): Avg. % of population infected by country from March 2020 to March 2021

### 4.4. Business Impact & Insights

- Europe and North America saw the highest cumulative death counts  
- Belgium and the UK had among the highest infection rates per capita  
- Countries like Cambodia and Afghanistan had notably low infection percentages  
- Vaccination rollout was uneven, highlighting disparities across regions  
- The dashboard can inform international comparisons and help monitor future health crises

---

## 5. Data Sources & Schema

This project uses two key datasets sourced from [Our World in Data](https://ourworldindata.org/covid-vaccinations) and stored locally for SQL analysis. These datasets were loaded into MySQL Workbench and cleaned for structured querying.

### 🔗 Dataset Links

- **COVID-19 Deaths Dataset**  
  Includes daily case counts, deaths, testing, hospitalizations, and ICU admissions by country and date.  
  [📁 View Dataset 1 (Google Drive)](https://drive.google.com/file/d/1zJ9vGEhgsUVph1_665KEP0vhawt37Y_v/view?usp=sharing)

- **COVID-19 Vaccinations Dataset**  
  Contains records of total and new vaccinations, vaccination per hundred, and demographic variables.  
  [📁 View Dataset 2 (Google Drive)](https://drive.google.com/file/d/1Tk17MTTUC6_rhhjfvrPEROqO7i-x0waX/view?usp=sharing)

- The two tables are linked using both the `location` and `date` fields, enabling country-level trend analysis and vaccination comparisons over time.

### 5.1. Table 1: `coviddeaths`

| Column Name              | Description                                   |
|--------------------------|-----------------------------------------------|
| location                 | Country or region name                        |
| continent                | Continent of the country                      |
| date                     | Date of record (formatted as YYYY-MM-DD)     |
| population               | Population of the country                     |
| total_cases              | Cumulative number of confirmed cases          |
| new_cases                | New cases reported on that day                |
| total_deaths             | Cumulative number of confirmed deaths         |
| new_deaths               | New deaths reported on that day               |
| new_cases_smoothed       | 7-day rolling average of new cases            |
| icu_patients             | Number of ICU patients                        |
| hosp_patients            | Number of hospital patients                   |
| total_deaths_per_million | Deaths per 1 million population               |

> Additional columns exist for advanced metrics such as case rates, hospitalization, testing, and smoothed indicators.

### 5.2. Table 2: `covidvaccinations`

| Column Name                     | Description                                      |
|----------------------------------|--------------------------------------------------|
| date                            | Date of record (formatted as YYYY-MM-DD)        |
| location                        | Country or region name                          |
| total_vaccinations              | Total vaccine doses administered                |
| people_vaccinated               | People with at least one dose                   |
| people_fully_vaccinated         | People who are fully vaccinated                 |
| new_vaccinations                | Doses administered on a specific day            |
| new_tests                       | Number of new COVID-19 tests conducted          |
| positive_rate                   | Share of positive tests                         |
| tests_per_case                  | Number of tests conducted per confirmed case    |
| extreme_poverty, smokers, gdp   | Socioeconomic and demographic indicators        |

> Like the deaths table, this dataset includes smoothed and per-million versions of many metrics.

---

## 6. Tools, Technologies & Methodology

This project integrates both SQL and Tableau to deliver a full-stack data analysis pipeline—from raw data cleaning to business-focused insights.

### 6.1. Technologies Used

- **SQL (MySQL Workbench):** Used for data cleaning, formatting, table joins, aggregations, and advanced analytics using CTEs, temp tables, and window functions.
- **Python (pandas, seaborn, matplotlib):** Utilized in early stages for exploratory data analysis and basic visual validation.
- **Tableau Public:** Primary tool for building interactive dashboards to communicate findings and trends across countries and time periods.

### 6.2. Methodology

a. **Data Ingestion & Cleaning:**
   - Standardized date format using `STR_TO_DATE()`.
   - Replaced blank values with `NULL` using `NULLIF()` to enable accurate filtering and aggregation.
   - Converted string-based numeric fields to usable format using `CAST()`.

b. **Exploratory & Descriptive Analysis in SQL:**
   - Calculated death rates and infection rates by country and date.
   - Identified countries and continents with the highest case and death counts.
   - Assessed vaccine rollout and testing metrics.
   - Aggregated global statistics to track COVID trends over time.

c. **Advanced SQL Logic:**
   - Used **JOINs** to merge datasets on `location` and `date`.
   - Applied **window functions** (e.g. `SUM() OVER (...)`) to calculate rolling vaccination counts.
   - Built **CTEs** and **temporary tables** to support complex aggregations.
   - Created **views** to streamline integration with Tableau dashboards.

d. **Visualization & Insight Communication:**
   - Tableau dashboard presents KPIs such as total deaths, infection rates, and vaccination coverage.
   - Users can interactively explore COVID trends by country and continent over time.
   - The final visualization is accessible on [Tableau Public](https://public.tableau.com/views/CovidProject_17516757694200/Dashboard1).

> This end-to-end process—from raw data to interactive dashboard—demonstrates the use of SQL for deep analytical querying and Tableau for clear, business-relevant storytelling.

---

## 7. SQL Data Preparation & Analytical Workflow

This section outlines how SQL was used not only for data cleaning but also for conducting exploratory and comparative analysis before visualization.

### 7.1. Data Cleaning

- Used `STR_TO_DATE()` to standardize all date columns from `MM/DD/YYYY` to `YYYY-MM-DD` format.
- Applied `NULLIF()` to convert blank strings into `NULL`, enabling proper filtering and aggregation.
- Used `CAST()` to convert textual numeric fields (like deaths or vaccinations) into integers for accurate calculations.

### 7.2. Exploratory & Descriptive Analysis

- **Case Fatality Rate by Country:**

  ```sql
  SELECT location, date, total_cases, total_deaths,
         (total_deaths / total_cases) * 100 AS death_percentage
  FROM coviddeaths
  WHERE location LIKE '%states';
  ```

- **Infection Rate vs. Population:**

  ```sql
  SELECT location, population, total_cases,
         (total_cases / population) * 100 AS infection_rate
  FROM coviddeaths
  WHERE continent IS NOT NULL;
  ```

- **Countries with Highest Deaths:**

  ```sql
  SELECT location, MAX(CAST(total_deaths AS SIGNED)) AS total_death_count
  FROM coviddeaths
  WHERE continent IS NOT NULL
  GROUP BY location
  ORDER BY total_death_count DESC;
  ```

- **Global Statistics by Date:**

  ```sql
  SELECT date,
         SUM(new_cases) AS total_cases,
         SUM(CAST(new_deaths AS SIGNED)) AS total_deaths,
         SUM(CAST(new_deaths AS SIGNED)) / SUM(new_cases) * 100 AS death_percentage
  FROM coviddeaths
  WHERE continent IS NOT NULL
  GROUP BY date;
  ```

### 7.3. Advanced Aggregation Techniques

- **JOINs** were used to combine deaths and vaccinations by country and date.
- **Window Functions** (`OVER (PARTITION BY ...)`) calculated running totals of new vaccinations for each country.
- **CTEs** were used to simplify complex queries and allow modular percentage calculations.
- **Temporary Tables** were created to store vaccination statistics and avoid recalculations.
- **Views** were created to persist logic for visualization and simplify Tableau integration.

> These SQL workflows allowed for deep exploration of the data and formed the analytical backbone of the Tableau dashboard.

---

## 8. Tableau Dashboard

🔗[View Full Dashboard on Tableau Public](https://public.tableau.com/views/CovidProject_17516757694200/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

### Dashboard Snapshot

Below is a snapshot of the Tableau dashboard:

![COVID-19 Tableau Dashboard](https://raw.githubusercontent.com/annievu22/Covid_Project/main/Covid%20Project%20-%20Tableau%20Snapshot.png)

---

## 9. Final Conclusion

This project demonstrates how structured SQL queries and well-designed dashboards can transform raw pandemic data into meaningful, accessible insights. From formatting dates and filling NULLs to performing joins and rolling aggregates, each SQL step helped uncover key global trends.

The resulting Tableau dashboard provides a quick yet comprehensive view of global COVID-19 impacts, from infections to deaths to vaccinations. It reveals regional disparities, health system challenges, and the scale of the pandemic.

In future iterations, this project could be enhanced by:

- Integrating mobility or policy data to contextualize spikes  
- Including vaccination manufacturer breakdowns  
- Applying machine learning for trend forecasting  

Overall, this project reflects a strong foundation in SQL data transformation, global health analytics, and effective data storytelling.


