# 🦠 COVID-19 Data Analytics Project
![Language](https://img.shields.io/badge/Language-SQL-blue)
![Tableau](https://img.shields.io/badge/Visualization-Tableau-pink)
![Status](https://img.shields.io/badge/Project-Completed-brightgreen)
![Data](https://img.shields.io/badge/Data-COVID‒19-red)

> A SQL and Tableau analysis of global COVID-19 data, tracking global infection spikes, death rates, and vaccination rollouts across countries and continents.

---

## 1. Overview

This project examines global COVID-19 data using SQL and Tableau to uncover trends in infection rates, death tolls, and vaccination progress across countries and continents. It provides an interactive tool to support health awareness and policy planning.

SQL was used for structured data transformation including date formatting, joins, rolling metrics, and population-based comparisons. Tableau visuals deliver a clear narrative of pandemic evolution, highlighting regional disparities and vaccination rollouts.

---

## 2. Business Objectives

### 2.1. Business Problem

Since 2020, COVID-19 has disrupted global health, mobility, and economies. This project transforms raw data into actionable insights to help track cases, compare vaccination rates, identify high-risk regions, and support data-driven public health decisions.

> **Key questions** addressed in this analysis:
> - Which countries have the highest infection rates relative to their population?
> - What is the case fatality rate (death rate among confirmed cases) by country?
> - How do vaccination rollouts compare across countries and over time?
> - What percentage of each country’s population has been fully vaccinated?
> - What regions are underperforming in vaccination efforts despite high infection rates?

### 2.2. Business Impact & Insights

- Europe and North America saw the highest cumulative death counts. 
- Belgium and the UK had among the highest infection rates per capita. 
- Countries like Cambodia and Afghanistan had notably low infection percentages.  
- Vaccination rollout was uneven, highlighting disparities across regions.  
- The dashboard can inform international comparisons and help monitor future health crises.

---

## 3. Data Sources & Schema

This project uses two key datasets from [Our World in Data](https://ourworldindata.org/covid-vaccinations), which were downloaded and loaded into MySQL for structured querying. The datasets include daily case, death, and vaccination data by country and date.

### 🔗 Dataset Links

- **COVID-19 Deaths Dataset**  
  Includes daily case counts, deaths, testing, hospitalizations, and ICU admissions by country and date.  
  [📁 View Dataset 1 (Google Drive)](https://drive.google.com/file/d/1zJ9vGEhgsUVph1_665KEP0vhawt37Y_v/view?usp=sharing)

- **COVID-19 Vaccinations Dataset**  
  Contains records of total and new vaccinations, vaccination per hundred, and demographic variables.  
  [📁 View Dataset 2 (Google Drive)](https://drive.google.com/file/d/1Tk17MTTUC6_rhhjfvrPEROqO7i-x0waX/view?usp=sharing)

- The two tables are linked using both the `location` and `date` fields, enabling country-level trend analysis and vaccination comparisons over time.

```sql
SELECT *
FROM coviddeaths dea
JOIN covidvaccinations vac
  ON dea.location = vac.location AND dea.date = vac.date;
```

### 3.1. Table 1 Schema: `coviddeaths`

```sql
CREATE TABLE coviddeaths (
    location                 VARCHAR(100),
    continent                VARCHAR(50),
    date                     DATE,
    population               BIGINT,
    total_cases              FLOAT,
    new_cases                FLOAT,
    total_deaths             FLOAT,
    new_deaths               FLOAT,
    new_cases_smoothed       FLOAT,
    icu_patients             FLOAT,
    hosp_patients            FLOAT,
    total_deaths_per_million FLOAT
    -- Additional columns not listed here for brevity
);
````

### 3.2. Table 2 Schema: `covidvaccinations`

```sql
CREATE TABLE covidvaccinations (
    date                      DATE,
    location                  VARCHAR(100),
    total_vaccinations        FLOAT,
    people_vaccinated         FLOAT,
    people_fully_vaccinated   FLOAT,
    new_vaccinations          FLOAT,
    new_tests                 FLOAT,
    positive_rate             FLOAT,
    tests_per_case            FLOAT,
    extreme_poverty           FLOAT,
    smokers                   FLOAT,
    gdp                       FLOAT
    -- Additional columns not listed here for brevity
);
````
---

## 4. Methodology & SQL Analysis

This section outlines the complete process of transforming raw COVID-19 datasets into actionable insights using MySQL for cleaning, analysis, and Tableau dashboard preparation.

### 4.1. Data Cleaning

* **Standardized all date columns** → Converted to `YYYY-MM-DD` format using `STR_TO_DATE()`.

```sql
SELECT STR_TO_DATE(date, '%m/%d/%Y') AS formatted_date FROM coviddeaths;
```

* **Replaced blank entries with `NULL`** → Used `NULLIF()` to improve filtering and aggregation.

```sql
SELECT NULLIF(total_deaths, '') AS total_deaths_cleaned FROM coviddeaths;
```

* **Converted text-based numeric fields** → Used `CAST()` for calculations.

```sql
SELECT CAST(total_deaths AS SIGNED) AS deaths_numeric FROM coviddeaths;
```

> These steps ensured consistent data types and analytical readiness.

### 4.2. Exploratory Data Analysis (EDA)

* **Case Fatality Rate by Country** → Calculated death-to-case ratios.

```sql
SELECT location, date, total_cases, total_deaths,
       (total_deaths / total_cases) * 100 AS death_percentage
FROM coviddeaths
WHERE location LIKE '%states';
```

* **Infection Rate vs. Population** → Evaluated infection penetration.

```sql
SELECT location, population, total_cases,
       (total_cases / population) * 100 AS infection_rate
FROM coviddeaths
WHERE continent IS NOT NULL;
```

* **Top Countries by Death Count** → Identified hotspots globally.

```sql
SELECT location, MAX(CAST(total_deaths AS SIGNED)) AS total_death_count
FROM coviddeaths
WHERE continent IS NOT NULL
GROUP BY location
ORDER BY total_death_count DESC;
```

* **Global Daily Statistics** → Aggregated worldwide cases and deaths.

```sql
SELECT date,
       SUM(new_cases) AS total_cases,
       SUM(CAST(new_deaths AS SIGNED)) AS total_deaths,
       SUM(CAST(new_deaths AS SIGNED)) / SUM(new_cases) * 100 AS death_percentage
FROM coviddeaths
WHERE continent IS NOT NULL
GROUP BY date;
```

* **Calculate Rolling Vaccination Totals** → Used `SUM() OVER (...)` to show daily progress.

```sql
SELECT location, date,
       SUM(new_vaccinations) OVER (PARTITION BY location ORDER BY date) AS rolling_vaccinations
FROM covidvaccinations;
```

* **Vaccination Percentage by Population (CTE)** → Compared doses to country population.

```sql
WITH PopvsVac AS (
  SELECT dea.continent, dea.location, dea.date, dea.population,
         vac.new_vaccinations,
         SUM(vac.new_vaccinations) OVER (PARTITION BY dea.location ORDER BY dea.date) AS rolling_vaccinations
  FROM coviddeaths dea
  JOIN covidvaccinations vac
    ON dea.location = vac.location AND dea.date = vac.date
)
SELECT *, (rolling_vaccinations / population) * 100 AS pct_vaccinated
FROM PopvsVac;
```

> These queries enabled effective tracking of vaccine rollout and global health responses and supported trend analysis, comparisons, and hotspot detection.


### 4.3. SQL Preparation for Tableau

* **Created Views for Dashboard Integration** → Simplified data sources for visualization.

```sql
CREATE VIEW covid_vacc_summary AS
SELECT location, date,
       SUM(new_vaccinations) AS daily_vaccinations
FROM covidvaccinations
GROUP BY location, date;
```

> Views and cleaned tables streamlined the Tableau workflow and reduced query complexity.

### 4.4. Tableau Dashboard Design

- This Tableau dashboard provides a comprehensive visualization of global COVID-19 trends in cases, deaths, and vaccinations, supporting public health decisions through interactive and data-driven insights.

🔗[View Full Dashboard on Tableau Public](https://public.tableau.com/views/CovidProject_17516757694200/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

### Dashboard Snapshot

Below is a snapshot of the Tableau dashboard:

![COVID-19 Tableau Dashboard](https://raw.githubusercontent.com/annievu22/Covid_Project/main/Covid%20Project%20-%20Tableau%20Snapshot.png)

### Walkthrough of Key Visuals:

- **Country Selector (Dropdown Filter):**  
  Allows users to select and compare COVID-19 trends by country. All visuals on the dashboard update dynamically based on the selected country.

- **Top KPIs (Big Number Cards):**  
  Displays the latest numbers for:
  - Total confirmed cases
  - Total deaths
  - Total vaccinations
  - Fully vaccinated (% of population)  
  → These metrics provide quick context on the overall pandemic impact in a selected country.

- **Trend of Daily New Cases & Deaths (Dual Line Chart):**  
  Compares daily new confirmed cases vs. deaths. Helps highlight peak waves and the lag between infection and mortality.

- **Rolling Vaccinations Over Time (Line Chart):**  
  Shows the cumulative count of people vaccinated by date using a rolling total. Offers insight into vaccine rollout speed and consistency.

- **Vaccination Coverage by Country (Bar Chart):**  
  Visualizes the percentage of each country’s population that has been fully vaccinated. Enables quick comparisons of vaccine access and adoption.

- **Infection Rate vs. Death Rate by Country (Scatter Plot):**  
  Plots infection rates against case fatality rates to reveal patterns, outliers, and public health effectiveness.

- **Global Map: Total Deaths by Country:**  
  A filled map that colors each country by its total number of COVID-related deaths. Useful for regional comparisons and identifying severely impacted areas.

> Together, these visuals provide a comprehensive view of the pandemic’s evolution—supporting comparative analysis, policy decisions, and public communication.

---

## 7. Final Conclusion

This project demonstrates how global COVID-19 data can be cleaned and analyzed using SQL, then visualized through Tableau to uncover key public health and policy insights. By examining infection rates, death percentages, and vaccine rollout over time and geography, we enable deeper understanding of pandemic patterns.

**Key business insights:**

* Countries with early vaccine adoption saw reduced fatality rates in later waves
* High population countries didn’t always have the highest case or death counts
* Rolling vaccination totals and infection spikes revealed regional response gaps
* Global aggregates helped contextualize national strategies and inform WHO-level reporting

**Future enhancement:**

* Including real-time APIs for live case and vaccine data
* Layering economic impact indicators like unemployment or lockdown policies
* Using forecasting models to predict future infection or vaccination trends

Overall, this project showcases practical SQL analysis and dashboarding skills while turning complex public health data into actionable insights for global stakeholders.
