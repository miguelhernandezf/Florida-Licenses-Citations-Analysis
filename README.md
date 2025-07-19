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

### 📊 Key Trends Identified

- **License Composition**: The most recent report shows that only **30.27%** of licenses belong to **Senior Drivers (24.03%)** and **Teenage Drivers**, meaning the majority of licenses are held by individuals aged **21–64**. The data also shows that **females consistently hold more licenses than males** over the years.

- **Year of Decline**: The only year where the `Year-over-Year License Growth` was negative was **2023 vs 2022**. While this doesn’t confirm a long-term trend, it prompts further investigation into economic or demographic changes during that period.

- **Licensing Concentration**: Driver license issuance is **heavily concentrated in South Florida**, particularly in **Miami-Dade, Broward, and Palm Beach** counties, reflecting population density and urban demand.

- **Top Violation — SPD POST ZONE**: This continues to be the most cited violation across all years, suggesting either poor speed limit visibility, signage issues, or frequent enforcement focus in these zones.

- **Enforcement Distribution**: The **City Police Departments** issue the majority of citations, followed by the **Florida Highway Patrol (FHP)** and **Sheriff’s Departments**. This distribution highlights possible variations in local enforcement practices and resource availability.

- **Citation Volume Over Time**: Citations have shown a general upward trend, with the largest increase occurring between **2020 and 2021**, with a **20.64% jump** (~400,000 more citations).

- **Most Common Legal Outcome**: The most frequent resolution for citations is **Dismissal**, indicating patterns in adjudication or possible inefficiencies in case processing.

---

### ✅ Actionable Recommendations

- **Target Educational Campaigns at Common Violations**: Focus awareness initiatives around high-frequency infractions like **SPD POST ZONE** to reduce recurrence through community education and improved signage.

- **Leverage South Florida Licensing Density for Resource Allocation**: Since South Florida is the dominating location in terms of volumen of licenses. It would be efficent to increase the FLHSMV 

- **Review Court Outcomes for Policy Insight**: The trend of rising **Dismissals** could indicate areas for judicial or law enforcement training or reveal flaws in citation issuance.

- **Expand Young Driver Safety Programs**: Given the growing number of **Teenage Drivers**, it’s vital to support pre-licensing education and defensive driving courses aimed at this age group.

- **Analyze Declining Violations for Replicable Strategies**: Investigate why certain violation types are consistently declining and identify enforcement or policy strategies that could be replicated elsewhere.

- **Enhance Agency Coordination**: Since citations are distributed across various enforcement agencies, establishing **standardized reporting and shared data protocols** could enhance both transparency and strategic planning.

