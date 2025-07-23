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

> The two tables are linked using both the `location` and `date` fields, enabling country-level trend analysis and vaccination comparisons over time.

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

## 4. Tech Stack & Methodology

### 4.1. Tech Stack

- **SQL (MySQL Workbench):** Used for data cleaning, formatting, table joins, aggregations, and advanced analytics using CTEs, temp tables, and window functions.
- **Tableau Public:** Primary tool for building interactive dashboards to communicate findings and trends across countries and time periods.

### 4.2. Methodology

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

## 5. SQL Data Preparation & Analytical Workflow

This section outlines how SQL was used not only for data cleaning but also for conducting exploratory and comparative analysis before visualization.

### 5.1. Data Cleaning

- Used `STR_TO_DATE()` to standardize all date columns from `MM/DD/YYYY` to `YYYY-MM-DD` format.
- Applied `NULLIF()` to convert blank strings into `NULL`, enabling proper filtering and aggregation.
- Used `CAST()` to convert textual numeric fields (like deaths or vaccinations) into integers for accurate calculations.

### 5.2. Exploratory & Descriptive Analysis

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

### 5.3. Advanced Aggregation Techniques

- **JOINs** were used to combine deaths and vaccinations by country and date.
- **Window Functions** (`OVER (PARTITION BY ...)`) calculated running totals of new vaccinations for each country.
- **CTEs** were used to simplify complex queries and allow modular percentage calculations.
- **Temporary Tables** were created to store vaccination statistics and avoid recalculations.
- **Views** were created to persist logic for visualization and simplify Tableau integration.

> These SQL workflows allowed for deep exploration of the data and formed the analytical backbone of the Tableau dashboard.

---

## 6. Tableau Dashboard

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

This project showcases how structured SQL queries and interactive Tableau dashboards can convert raw pandemic data into valuable business and public health insights. By cleaning, joining, and aggregating complex datasets, we identified critical trends in COVID-19 transmission, mortality, and vaccine distribution across countries and continents.

From a business and policy perspective, the project offers actionable insights such as:
- Countries with high infection rates but low vaccination coverage, signaling urgent intervention needs.
- Shifting trends in new cases and deaths over time, useful for capacity planning in healthcare systems.
- Variations in vaccine rollout efficiency, helping identify logistical or equity challenges across regions.

The Tableau dashboard enables stakeholders—public health officials and NGOs—to monitor pandemic progression quickly, compare global responses, and guide data-driven decisions.

In future iterations, this project could be enhanced by:

- Integrating mobility or policy data to contextualize spikes  
- Including vaccination manufacturer breakdowns  
- Applying machine learning for trend forecasting  

Overall, this project reflects a strong foundation in SQL data transformation, global health analytics, and effective data storytelling.


