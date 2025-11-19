# Geospatial Election Integrity Analysis – Kogi State Nigeria

A geospatial and statistical investigation into polling-unit–level voting anomalies across Kogi State during Nigeria’s 2023 General Elections.  
This report combines **GIS neighbourhood analysis** and **Z-score statistical outlier detection** to highlight polling units whose voting results deviate significantly from their immediate surroundings.

---

## 📌 Table of Contents
1. [Introduction](#1-introduction)  
2. [Data Preparation](#2-data-preparation)  
   - [2.1 Dataset Collection](#21-dataset-collection)  
   - [2.2 Data Cleaning and Formatting](#22-data-cleaning-and-formatting)  
3. [Geospatial Neighbourhood Analysis](#3-geospatial-neighbourhood-analysis)  
4. [Outlier Detection Methodology](#4-outlier-detection-methodology)  
   - [4.1 Statistical Approach (Z-Score Method)](#41-statistical-approach-z-score-method)  
   - [4.2 Why Z-Score Instead of DBSCAN/HDBSCAN](#42-why-z-score-instead-of-dbscanhdbscan)  
5. [Outlier Computation and Sorting](#5-outlier-computation-and-sorting)  
6. [Findings](#6-findings)  
7. [Top 3 Outliers per Party](#7-top-3-outliers-per-party)  
   - [7.1 APC](#71-apc)  
   - [7.2 LP](#72-lp)  
   - [7.3 PDP](#73-pdp)  
   - [7.4 NNPP](#74-nnpp)  
8. [Visualization and Mapping](#8-visualization-and-mapping)  
9. [Discussion and Insights](#9-discussion-and-insights)  
10. [Conclusion](#10-conclusion)  
11. [Appendix](#11-appendix)  

---

## 1. Introduction

Election integrity underpins democratic trust. In the context of the controversies linked to Nigeria’s 2023 General Elections, this project conducts a **geospatial anomaly analysis** at the polling unit level in **Kogi State**.

**Goal:**  
To identify polling units whose results show unusual deviations from neighbouring polling units—potential indicators of:  
- Data tampering  
- Recording inconsistencies  
- Localised irregularities  

This is achieved using:  
- **Z-score–based statistical deviation analysis**  
- **Spatial neighbourhood computation** within a 1 km radius  
- **Outlier scoring** for APC, LP, PDP, and NNPP  

---

## 2. Data Preparation

### 2.1 Dataset Collection
Dataset: **KOGI_crosschecked.csv**

Contains:  
- State, LGA, Ward, PU Code, PU Name  
- Votes for APC, LP, PDP, NNPP  

**Geocoding Challenge:**  
The dataset initially lacked latitude and longitude.

Two geocoding attempts were made:

1. **PU names-based geocoding**  
   - Duration: ~50 minutes  
   - Result: Returned *0 valid coordinates*

2. **LGA + Ward–based geocoding**  
   - Effective and accurate  
   - Successfully generated coordinates for all polling units  

### 2.2 Data Cleaning and Formatting
Performed steps:
- Removed invalid/missing coordinates  
- Converted coordinates to numeric values  
- Standardized vote counts (no blanks or negatives)  
- Cleaned and normalized column names  

---

## 3. Geospatial Neighbourhood Analysis

Distances between polling units were computed using the **Haversine formula**.

- A **1 km radius** was used to identify “neighbouring” polling units.  
- Each polling unit was assigned a list of neighbours stored in:  
  **`neighbors_within_1km`**
<img width="881" height="633" alt="Screenshot (724)" src="https://github.com/user-attachments/assets/5536256a-54e9-4802-9678-828ff337602d" />

This spatial structure forms the basis for statistical comparison.

---

## 4. Outlier Detection Methodology

### 4.1 Statistical Approach (Z-Score Method)

For each political party (APC, LP, PDP, NNPP):

1. Compute mean vote counts among neighbouring polling units  
2. Compute standard deviation  
3. Calculate Z-score:
Outlier Score =
Votes𝑷𝑼 − Mean𝑵𝒆𝒊𝒈𝒉𝒃𝒐𝒖𝒓𝒔/Std𝑵𝒆𝒊𝒈𝒉𝒃𝒐𝒖𝒓

A high **absolute** Z-score = strong deviation → possible anomaly.

---

### 4.2 Why Z-Score Instead of DBSCAN / HDBSCAN?

Although DBSCAN/HDBSCAN detect spatial clusters, they:

- Lack interpretability  
- Do not quantify “how abnormal” a vote count is  
- Are less suitable for election integrity reporting  

**Z-score was chosen because it is:**  
✔ Transparent  
✔ Numerically interpretable  
✔ Consistent across all political parties  
✔ Easy to audit and explain to stakeholders  

---

## 5. Outlier Computation and Sorting

Generated outputs include:

- `KOGI_with_outlier_scores.csv`  
- `KOGI_outlier_scores_with_neighbours.xlsx`  
- `KOGI_all_outliers_sorted.csv`  
- `top_outliers_APC.csv`  
- `top_outliers_LP.csv`  
- `top_outliers_PDP.csv`  
- `top_outliers_NNPP.csv`  
<img width="851" height="223" alt="Screenshot (725)" src="https://github.com/user-attachments/assets/102f060f-738f-49c6-8de4-7e0eb03265b0" />

Process summary:
- Z-scores calculated per party  
- Combined into a consolidated dataset  
- Sorted in descending order  
- Top outliers flagged for inspection  

---

## 6. Findings

Key findings include:

- Significant deviations found across multiple LGAs  
- Outliers varied in distribution and intensity across political parties  
- Some wards exhibited clustering of anomalies  
- Several polling units showed extreme deviations from their neighbours  

These deviations warrant further scrutiny.

---

## 7. Top 3 Outliers per Party

### 7.1 APC
| PU Name | LGA | Outlier Score |
|--------|------|---------------|
| AJAGWUMU LGEA PRIMARY SCHOOL | DEKINA | **451.89** |
| AGINI/BUZHI LGEA SCHOOL | LOKOJA | **443.00** |
| R.C.C. SCHOOL – UPOGORO IV | OKENE | **286.54** |

**Conclusion:**  
APC results show unusually high deviations, suggesting possible localized irregularities.

---

### 7.2 LP
| PU Name | LGA | Outlier Score |
|--------|------|---------------|
| OPEN SPACE ETUTU-EKPE ATE 1 | DEKINA | **559.59** |
| LGEA SCH. GANAJA VILLAGE 55 | AJAOKUTA | **465.51** |
| UNITY LAYOUT BEHIND FIRST UNITY | AJAOKUTA | **156.82** |

**Conclusion:**  
LP outliers show strong localised spikes inconsistent with neighbouring PUs.

---

### 7.3 PDP
| PU Name | LGA | Outlier Score |
|--------|------|---------------|
| LGEA SCH. GANAJA VILLAGE | AJAOKUTA | **308.78** |
| OJEDE CENTRE OPEN SPACE | ANKPA | **144.94** |
| OPEN SPACE/OKULA 2 & 3 – OKO OKULA | OFU | **124.82** |

**Conclusion:**  
PDP outliers are notably high in politically competitive or mixed-party regions.

---

### 7.4 NNPP
| PU Name | LGA | Outlier Score |
|--------|------|---------------|
| LGEA PRI. SCH. OLADEBU | OFU | **107.57** |
| HOLY TRINITY SCHOOL OBOROKE I | OKEHI | **87.62** |
| OKOWOWOLO OPEN SPACE | DEKINA | **60.46** |

**Conclusion:**  
Though fewer, NNPP outliers remain statistically significant.

---

## 8. Visualization and Mapping

Maps were created using **GeoPandas** and **Matplotlib** to display identified anomalies:

- APC Outlier Map
  <img width="713" height="549" alt="Screenshot (726)" src="https://github.com/user-attachments/assets/3b0b5eb9-8705-4e68-8bd6-681541f72c0d" />

- PDP Outlier Map
  <img width="694" height="547" alt="Screenshot (727)" src="https://github.com/user-attachments/assets/4b26fcc2-952d-474c-b3ce-d19c9a858b24" />

- LP Outlier Map
  <img width="719" height="575" alt="Screenshot (728)" src="https://github.com/user-attachments/assets/eb712d23-396f-4e1c-9ea6-b437d4fdb23a" />

- NNPP Outlier Map
  <img width="717" height="530" alt="Screenshot (729)" src="https://github.com/user-attachments/assets/deefd577-29b1-4623-8d35-99435da71eac" />


These maps visually highlight spatial concentrations and help identify geopolitical zones requiring closer review.

---

## 9. Discussion and Insights

Key observations:

- Many anomalies occur near **LGA borders**  
- APC shows widespread high outliers across numerous LGAs  
- LP anomalies concentrated in semi-urban environments  
- PDP deviations often in politically competitive regions  
- NNPP anomalies small in number but strongly significant  

These patterns may reflect inconsistencies in reporting, demographic shifts, or administrative variations.

---

## 10. Conclusion

This project successfully applied **Z-score analysis** and **spatial neighbourhood modelling** to detect potential voting irregularities across Kogi State.

### ✔ Key Takeaways
- Identifies polling units warranting deeper investigation  
- Provides transparent, statistic-driven evidence  
- More interpretable than clustering-based alternatives  
- Geographic + statistical integration yields richer insights  

**Future Work:**  
Combining Z-score with HDBSCAN could enhance anomaly detection and reveal regional clusters of irregularities.

---

## 11. Appendix

### Dataset
- `KOGI_crosschecked.csv`

### Output Files
- `KOGI_with_outlier_scores.csv`  
- `KOGI_all_outliers_sorted.csv`  
- `KOGI_outlier_scores_with_neighbours.xlsx`  

### Libraries Used
- pandas  
- geopandas  
- numpy  
- matplotlib  
- scipy  

