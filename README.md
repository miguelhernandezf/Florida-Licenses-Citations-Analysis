# 🚦🚗📄 Florida-Licenses-Citations-Analysis

---

### 🎯 Objectives

This dashboard aims to visualize important insights from a large, disconnected system of yearly data. Without meaningful analysis, this information remains difficult to interpret limiting its value for decision-makers, policymakers, and the general public 
who rely on it to understand critical driving trends across Florida.

This analytical tool supports informed decision-making for key stakeholders such as the State of Florida, public institutions, insurance companies, and other entities that benefit from understanding licensing behavior and citation overviews and patterns.

---

### 📚 About Data  

---

The Department of Highway Safety and Motor Vehicles (FLHSMV) provides annual reports on driver licenses and traffic citations across the state of Florida. Each file represents a different year and includes raw, tabular data. 

- **Driver Licenses Dataset** contains columns such as `Gender`, `County`, `Licenses`, `Age Brackets`, and `Grand Total`.  
- **Traffic Citations** dataset contains columns such as `Violation Group`, `FHP`, `CityPD`, `ShrfDept`, `Other`, `TotalViol`, `Guilty`, `PD CivilP`, `Adj w/h ClkCt`, `Adj w/h Judge`, `NotGuilty`, `Dism`, `NoliPros`, `TotaDisp` and `DispPend`. 

Thanks to **FLHSMV** for sharing this data.  
📍 [Dataset link: FLHSMV](https://www.flhsmv.gov/resources/driver-and-vehicle-reports/vehicle-and-vessel-reports-and-statistics/)

---

### ✏️ Data Wrangling

---

#### ETL Process Overview:

Both datasets — **Driver Licenses** and **Traffic Citations** — required tailored cleaning and transformation before analysis. The process included:  
- Removing null values  
- Filtering out redundant and pre-calculated columns  
- Renaming columns for consistency  
- Resolving inconsistencies like naming differences across files (e.g., "St Johns" vs. "St. Johns")  
- Extracting temporal information (Year) and unifying the schema for multi-year analysis  

---

#### **Driver Licenses Dataset**

All six PDF files (2020–2025) used for this project had different internal structures, which created inconsistencies during data integration. To solve this, I:

- Converted each file from PDF to Excel manually.
- Standardized the structure by promoting headers and aligning columns across years.
- Removed rows with errors, state-wide totals, and previously calculated grand totals.
- Extracted the year from the file name and duplicated it as a column to enable filtering and cross-referencing.
- Unpivoted the age bracket columns into a single column to facilitate age-based filtering and aggregation.
- Grouped the age values from 15–20 into a combined bracket to keep consistency with other age ranges.

**Original imported dataset:**  
<details>
<img width="1759" height="934" alt="image" src="https://github.com/user-attachments/assets/97d2ee3b-8748-4aab-b762-dc2ed5135efb" />
<img width="1762" height="971" alt="image" src="https://github.com/user-attachments/assets/cbd49be4-f05c-4b38-b6e2-60125887dfcb" />
<img width="1765" height="977" alt="image" src="https://github.com/user-attachments/assets/6058ce08-a7c6-4186-b281-9e76a13651a9" />
</details>

---

**Final transformed dataset:**  
<details>
<img width="1759" height="973" alt="image" src="https://github.com/user-attachments/assets/540962de-9ad7-4142-8fc7-0940ea3eff65" />
</details>

---

#### **Traffic Citations Dataset**

The Traffic Citations dataset required custom handling due to being spread across five PDF reports (2020–2024), each with formatting discrepancies.

To ensure uniformity, I:

- Manually manipulated and converted all PDF files into Excel with a common structure.
- Removed pre-aggregated summary rows and totals that could distort visualizations.
- Standardized column headers across years (e.g., `FHP`, `City PD`, `ShrfDept`, `Other`, `Guilty`, `Dismissed`, `Noli Pros`, etc.).
- Created DAX-based measures such as `Total Citations`, calculated by summing FHP, City PD, Sheriff, and Other columns.
- Cleaned and formatted categorical outcome fields (`Guilty`, `Not Guilty`, `Dismissed`, `Adj w/h Judge`, etc.) to enable accurate distribution tracking.
- Extracted and unified the `Year` column to enable consistent time-based slicing.
- Ensured proper data types for all numeric columns and validated values across all years.

---

**Original imported dataset:**  
<details>
<img width="1761" height="971" alt="image" src="https://github.com/user-attachments/assets/6b3ff4e3-3cd2-4bd8-83bd-8900c10028fb" />
<img width="1771" height="971" alt="image" src="https://github.com/user-attachments/assets/b03bfb00-706b-402a-9a9e-5cfa4f8b0b37" />
<img width="1762" height="971" alt="image" src="https://github.com/user-attachments/assets/86b67fb1-b9bd-4bb9-82d4-e23a6279081c" />
<img width="1764" height="968" alt="image" src="https://github.com/user-attachments/assets/548d1c7d-acd5-4a34-9dc4-3ffbecaaea5c" />
</details>


**Final transformed dataset:**  
<details>
<img width="1766" height="966" alt="image" src="https://github.com/user-attachments/assets/6bf08d4e-9a5c-4b02-98fe-78772a5556a3" />
<img width="1763" height="970" alt="image" src="https://github.com/user-attachments/assets/9518c023-2794-48d5-8332-c6de41a71ecf" />
</details>

---


### 🧩 Data Model

This project uses a star schema data model in Power BI, with the following dimensions:

- **Year**: Unique dimension present in both dataset.
- **County, Gender, and Age Bracket**: For slicing driver license trends.
- **Violation Group and Descripcion Dimension**: Helps to slice Citations trends visuals.

The fact tables are:
- `Driver Licenses` (cleaned & transformed Excel from 6 PDFs)
- `Uniform Traffic Citation Report` (merged 5 years of structured PDF data)

These tables were connected using lookup relationships to enable dynamic filtering and drill-downs.

STAR scheme:

<details>
<img width="562" height="509" alt="image" src="https://github.com/user-attachments/assets/bad50922-4d7e-4550-8e7c-175139317eed" />
<img width="824" height="396" alt="image" src="https://github.com/user-attachments/assets/0bd5e67d-a433-4c1d-babf-7e0cf6250c8c" />
</details>

---

#### 🧮 **DAX (Data Analysis Expressions)**

There are in total 19 DAX calculations in this dashboard. To keep this documentation concise, I will focus on the most impactful and frequently used ones.

#### **Driver Licenses Dataset**

<details>
<img width="349" height="242" alt="image" src="https://github.com/user-attachments/assets/b69137fe-2bc7-4c13-a558-ea219b0832b3" />
</details>

`Selected Year`: The first two slides were configured to always initialize in 2025 because they display metrics such as `Total Licenses` and `YoY License Growth`. Both require a base year to compare year-over-year values; otherwise, they would return (blank) or misleading aggregates.

<details>
<img width="1240" height="109" alt="image" src="https://github.com/user-attachments/assets/bf5176f8-e38f-44df-9561-617f6bb4fd14" />
</details>

#### **Traffic Citations Dataset**

<details>
<img width="356" height="265" alt="image" src="https://github.com/user-attachments/assets/300f97a9-ad6a-44fb-9923-258546178f31" />
</details>

`Total Citations`: Since grand totals were removed during data cleaning, this measure sums citations reported by each enforcement department.

<details>
<img width="407" height="109" alt="image" src="https://github.com/user-attachments/assets/4b22b6d5-3b34-4663-8faf-54d3fdd3eeb3" />
</details>

---

### 📊 Visualization

---

- Produced a 4-slide interactive dashboard using Power BI

---

#### Slide 1 (Licenses Overview)

This slide gives a general view of the Driver Licenses dataset. All slides follow a consistent layout: slicers at the top, followed by KPIs. These KPIs provide insights such as `Total Licenses`, `Senior Drivers (%)`, `Teenage Drivers (%)`, and `Year-over-Year License Growth`. Below each KPI card, supporting context is displayed for better interpretability.

It includes:
- A line chart showing license trends by gender over the years
- A bar chart showing license volume by age bracket
- A toggle button to switch between visual and tabular views

<img width="1444" height="810" alt="image" src="https://github.com/user-attachments/assets/c8f5e059-52f6-40f8-bf36-807f7917de34" />
<img width="1443" height="809" alt="image" src="https://github.com/user-attachments/assets/eec248fb-476b-483e-a038-c74c59b06a5b" />

---

#### Slide 2 (County View)

This view provides a location-based analysis. Users can explore how different counties compare in terms of total licenses, gender distribution, and growth. It enables regional comparisons and decision-making support for local policy or service optimization.

<img width="1440" height="809" alt="image" src="https://github.com/user-attachments/assets/813a0b13-affe-4d2a-8e46-9c29c3726ede" />

---

#### Slide 3 (Citations Overview)

This is a general exploration of the Traffic Citations dataset. It includes four KPIs:
- `Total Citations`
- `Guilty Outcome Rate`
- `Total Paid Civil Penalty`
- `Most Common Violation` and its citation count

Charts included:
- Bar chart: `Most Cited Violations` (top violation descriptions)
- Donut chart: Enforcement agency breakdown (FHP, Sheriff, PD, etc.)
- Bar chart: Total Citations by `Violation Group`
- Table: Top 1 Violation per Group with totals

<img width="1442" height="809" alt="image" src="https://github.com/user-attachments/assets/a6aca214-d3a0-4fee-9ff5-c63c2665e4a4" />

---

#### Slide 4 (Citations Time-Based)

This slide focuses on time-based insights within the citations dataset. KPIs include:
- `% Growth in Total Citations YoY`
- `Average Citations YoY`

Charts:
- Line: Behavior of the Most Common Violation over time
- Line: Citation totals over time (by year)
- Bar: Outcomes over time (`Guilty`, `Dismissed`, etc.)
- Line: `Guilty` vs `Dismissed` trend comparison

<img width="1441" height="809" alt="image" src="https://github.com/user-attachments/assets/0d7b6a89-08e8-4dd6-9ba3-9c77f1836797" />

---

### ⚠️ Data Limitations and Suggestions

- The **Driver Licenses dataset** required significant manual transformation due to inconsistent formatting across years. For better automation and time consistency, maintaining a standardized base format is essential.

- The **Traffic Citations dataset** lacks geographic detail such as county, latitude, or longitude of infractions. Including this information would greatly enhance the ability to cross-analyze both datasets geographically.

- Several **violation outcome labels** like `"Nolle Prosequi"` and `"Adj w/h Judge"` were not self-explanatory and required manual research to interpret, which could affect automation or broader user understanding.

---

#### 📊 Key Trends Identified

- **Licenses Composition**: The most recent report indicates that only 30.27% of licenses belongs to `Senior Drivers` (24.03%) and `Teenagers Drivers` (%), so the gross of licenses are condensed between 21-64 years and charts shows a constant superiority of quantity
of licenses of females over males over years.

- **Decreasing year**: There is just one year where the `Year-over-Year License Growth` is negative, and it is 2023 vs 2022. With this data we can not make conclutions, but we can investigate or try to understand the context happening in those years.

- **Licensing Concentration**: The highest concentration of issued driver licenses in centered in South Florida, including Miami-Dade, Broward, and Palm Beach.

- **Top Violation: SPD POST ZONE**: This remains the most cited violation across years, highlighting potential areas for improved signage or enforcement calibration.

- **Enforcement Distribution**: The majority of citations are issued by **City Police Departments**, followed by **FHP** and **Sheriff’s Departments**. This reflects resource distribution and possible enforcement gaps.

- **Citations Over Time**: The quantity of citations tends to increase over the years. Being the 2021 vs 2020 the most variant interval, the delta between these years are 20.64%, around 400.000 citations

- **Most Common Court Situation**: Citations tends to finishing up in a `Dismissed` case.

---

#### ✅ Actionable Recommendations

- **Focus Education Campaigns on Most Common Violations**: Direct resources toward educating drivers about top infractions (e.g., SPD POST ZONE) to reduce recurrence.

- **Use Citation Outcome Trends for Judicial Review**: Agencies can assess changes in outcomes (e.g., increasing *Dismissals*) to identify potential inefficiencies or training needs.

- **Support Youth Driver Safety Programs**: Given the rising number of young drivers, it’s advisable to enhance defensive driving initiatives aimed at new licensees.

- **Explore Root Causes Behind Declining Violation Trends**: Understanding why some infractions are dropping could guide effective replication of enforcement strategies.

- **Improve Interdepartmental Coordination**: As enforcement is distributed among multiple agencies, coordinated data-sharing and unified reporting standards could improve efficiency and analysis.

