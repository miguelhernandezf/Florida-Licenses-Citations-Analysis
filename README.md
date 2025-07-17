# 🚦🚗📄 Florida-Licenses-Citations-Analysis

---

### 🎯 Objectives

This dashboard aims to visualize important insights from a large, disconnected system of yearly data. Without meaningful analysis, this information remains difficult to interpret limiting its value for decision-makers, policymakers, and the general public 
who rely on it to understand critical driving trends across Florida.
---

### 📚 About Data  
--- 
The Department of Highway Safety and Motor Vehicles (FLHSMV) provides annual reports on driver licenses and traffic citations across the state of Florida. Each file represents a different year and includes raw, tabular data. 

- **Driver Licenses Dataset** contains columns such as Gender, County, Licenses, Age Brackets, and Grand Total. 

- **Traffic Citations** dataset contains column such as Violation Group, FHP, CityPD, ShrfDept, Other, TotalViol, Guilty, PD CivilP, Adj w/h ClkCt, Adj w/h Judge, NotGuilty, Dism, NoliPros, TotaDisp and DispPend. 

Thanks to **FLHSMV** for sharing this data.
📍 [Dataset link: FLHSMV](https://www.flhsmv.gov/resources/driver-and-vehicle-reports/vehicle-and-vessel-reports-and-statistics/)

---

### ✏️ Data Wrangling 

---

ETL:

In this process and on both datasets were done basics steps of cleaning and prepation such as, removing nulls, filtered useless or previous total calculated columns, renamed columnms, and identify and solving inconsistencys like having two counties "St Johns" & "St. Johns"

- **Driver Licenses Dataset**

All six PDF's(2020-2025) used for this project had different structure ending in a issue when had to unified them. To solve this I had to clean the format and structure of every file, I converted them from PDF to Excel and then I cleaned and unified the structure.
Una vez dentro, I got rid of the errors, Grand Total previous calculated columns, and StateWide Totals. After promoting header to name my columns I extracted the year from the File Name column, and duplicate it to store at the very end of the file, so that way I can utilize the year and the FileName. The most critical step did it here was unpivot the age brackets, so this way we can store all the ages in just one column and we can work easily with the ages. After this step, I combined ages from 15-20 into one column so we are following the pattern of having age brackets. 

Original imported dataset:
<details>
<img width="1759" height="934" alt="image" src="https://github.com/user-attachments/assets/97d2ee3b-8748-4aab-b762-dc2ed5135efb" />
<img width="1762" height="971" alt="image" src="https://github.com/user-attachments/assets/cbd49be4-f05c-4b38-b6e2-60125887dfcb" />
<img width="1765" height="977" alt="image" src="https://github.com/user-attachments/assets/6058ce08-a7c6-4186-b281-9e76a13651a9" />
</details>
---

Final transformed dataset:

<details>
<img width="1759" height="973" alt="image" src="https://github.com/user-attachments/assets/540962de-9ad7-4142-8fc7-0940ea3eff65" />
</details>


### 📊 Visualization
---

- Produced a 4-Tabs dashboard using Power BI
  
![Visual1](https://github.com/user-attachments/assets/098cb444-0e96-413d-9d76-7bb4f5b11ae4)
---
![Visual2](https://github.com/user-attachments/assets/23ffee15-09e0-4ad7-83f6-b983a0783986)
---
![Visual3](https://github.com/user-attachments/assets/6a8ccdd0-0621-4562-9775-83e8d42da898)
---
![Visual4](https://github.com/user-attachments/assets/ca7972b2-dceb-426a-ba06-cae7d0ef5161)
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
