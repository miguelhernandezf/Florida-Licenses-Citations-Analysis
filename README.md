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

#### 🧮 **DAX (Data Analysis Expressions)**

There are in total 19 DAX calculations in this dashboard, I would not like to extend that much the documentation, I just going to dive in the most used calculations and explained them briefly.

#### **Driver Licenses Dataset**

<details>
<img width="349" height="242" alt="image" src="https://github.com/user-attachments/assets/b69137fe-2bc7-4c13-a558-ea219b0832b3" />
</details>

`Selected Year`: The first two slides were configured to always be inizialized in 2025 because it displays metrics such as `Total Licenses`, or `YoY License Growth`. In both cases is necesary to started with a Filter applied because
the SUM(`Licenses`) is the sum of all six years, and YoY metric needs to be inizialized in determined year to be able to compare year to year, if not it will display a (blank).

<details>
<img width="1240" height="109" alt="image" src="https://github.com/user-attachments/assets/bf5176f8-e38f-44df-9561-617f6bb4fd14" />
</details>


#### **Traffic Citations Dataset**

`Total Licenses`: Since grand totals from the original dataset were ruled out, to calculate the sum of total licenses we have to make a new measure of the sum of every enforcement department.

<details>
<img width="407" height="109" alt="image" src="https://github.com/user-attachments/assets/4b22b6d5-3b34-4663-8faf-54d3fdd3eeb3" />
</details>

<details>
<img width="356" height="265" alt="image" src="https://github.com/user-attachments/assets/300f97a9-ad6a-44fb-9923-258546178f31" />
</details>


### 📊 Visualization
---

- Produced a 4-Tabs dashboard using Power BI
  

---

---

---

---




---

### 💡 Recommendations
---
 
- This report enables stakeholders to identify which day of the week has the highest frequency of motor vehicle collisions, supporting more informed decision-making around resource allocation, public safety campaigns, or policy adjustments.
- Identifying Peak Collision Days: This report helps identify the day of the week with the highest frequency of collisions. Focus awareness campaigns and enforcement on peak days to reduce accidents.

- Yearly Crash Trends: Data can be filtered by year to analyze trends. Use this data to allocate resources effectively, especially during high-crash periods.

- Vehicle Type Insights: The dashboard allows filtering by vehicle type. Introduce targeted safety measures for vehicle types with higher collision rates.

- Geographic Analysis by Borough: Collisions can be analyzed by borough, supporting localized decisions. Direct resources to high-collision boroughs and evaluate infrastructure improvements in those areas.

- Interactive Map for Risk Zones: The map visualizes crash locations and risk zones. Review high-risk areas and collaborate with city planners to improve safety measures at critical intersections.
