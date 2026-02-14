
![SkinCancer](https://github.com/user-attachments/assets/68b4e44f-5f54-4a66-a1f9-2852fcfda379)

# 🩺 DermAI Diagnostic | Skin Cancer Analysis

**SQL-Based Analysis of 1,088 Clinical Cases to Identify Cancer Risk Factors**
Analyzing DermAI Diagnostic Database to uncover links between demographics, environmental exposure, and lesion traits in skin cancer. Prepares data for SQL queries, analysis, and ML models to improve early diagnosis.


[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---


## 📚 Table of Contents

- [🩺 DermAI Diagnostic | Skin Cancer Analysis](#-dermai-diagnostic--skin-cancer-analysis)
- [📊 Project Overview](#-project-overview)
- [📂 Project Workflow](#-project-workflow)
- [🎯 Key Findings](#-key-findings)
  - [Environmental Risk Patterns](#environmental-risk-patterns)
  - [Demographic Risk Stratification](#demographic-risk-stratification)
  - [Lesion Size as Diagnostic Indicator](#lesion-size-as-diagnostic-indicator)
  - [Critical Diagnostic Gap](#critical-diagnostic-gap)
- [🛠️ Technology Stack](#️-technology-stack)
- [🔍 Methodology](#-methodology)
  - [1. Database Setup & Data Integration](#1-database-setup--data-integration)
  - [2. SQL Query Development](#2-sql-query-development)
  - [3. Visualization & Reporting](#3-visualization--reporting)
- [💡 Business Recommendations](#-business-recommendations)
  - [Immediate Actions](#immediate-actions)
  - [Public Health Interventions](#public-health-interventions)
  - [Healthcare System Optimization](#healthcare-system-optimization)
- [📊 Dashboard Preview](#-dashboard-preview)
  - [Page 1: SQL Analysis Results](#page-1-sql-analysis-results)
  - [Page 2: Interactive Data Explorer](#page-2-interactive-data-explorer-in-development)
- [🎓 Skills Demonstrated](#-skills-demonstrated)
  - [Technical Skills](#technical-skills)
  - [Business Skills](#business-skills)
- [📈 Future Enhancements](#-future-enhancements)
- [📄 Data Sources & Ethics](#-data-sources--ethics)
- [🔗 Full Access](#full-access)
- [📫 Contact](#-contact)
- [📝 License](#-license)
- [🙏 Acknowledgments](#-acknowledgments)

## 📊 Project Overview
Comprehensive SQL analysis of 1,088 skin lesion cases to identify environmental, demographic, and clinical risk factors for early cancer detection. This project demonstrates end-to-end data analysis—from PostgreSQL query development to interactive Power BI visualization.

 ## Project Workflow
- Data Source: Anonymized dataset from 10Alytics learning platform
- Data Prep: Cleaned, structured, and loaded into PostgreSQL
- SQL Analysis: Explored lesion metrics, demographics, and environmental risks
- Visual Insights: Created Excel dashboards showing insights
- High-Risk Flags: Identified patients with cancer history and lesion clusters
- Presentation: Built PowerPoint slides summarizing findings and recommendations
- Documentation: Proper documentation with MS Word
- ML-Ready Output: Packaged structured data for future machine learning models

**Business Impact:**
- 🎯 Identified **54 high-risk patients** requiring immediate clinical surveillance
- 📈 Discovered **45% pesticide exposure** correlation with malignant lesions
- ⚠️ Flagged **366 unbiopsied ACK cases** revealing diagnostic gaps in healthcare delivery
- 📐 Established **lesion size >100mm²** as critical early warning indicator

---

## 🎯 Key Findings

### Environmental Risk Patterns
- **45.09%** of malignant lesions linked to pesticide exposure
- **80.48%** of precancerous lesions occur in areas lacking piped water
- **82%** of precancerous cases lack sewage system access

### Demographic Risk Stratification
- **Males show 3× higher smoking rates** (23.66% vs 7.41%) in malignant cases
- **6× higher drinking rates** (59.26% vs 9.88%) compared to females
- **50%+** of malignant cases have personal cancer history

### Lesion Size as Diagnostic Indicator
| Diagnosis Type | Avg Lesion Area | Risk Level |
|----------------|-----------------|------------|
| **MEL** (Melanoma) | 197.58 mm² | Critical |
| **BCC** (Basal Cell) | 106.63 mm² | High |
| **SCC** (Squamous Cell) | 100.95 mm² | High |
| **ACK** (Actinic Keratosis) | 24.95 mm² | Medium |
| **NEV** (Nevus) | 3.67 mm² | Low |

**Key Insight:** Melanoma lesions are **54× larger** than benign nevus

### Critical Diagnostic Gap
- **366 high-symptom ACK cases** remain unbiopsied despite clinical indicators
- Malignant lesions show high biopsy confirmation even with minimal symptoms
- Pattern suggests need for symptom-independent biopsy protocols

---

## 🛠️ Technology Stack

| Category | Tools |
|----------|-------|
| **Database** | PostgreSQL 16 + pgAdmin 4 |
| **Analysis** | SQL (CTEs, Window Functions, Aggregations) |
| **Visualization** | Power BI Desktop, Microsoft Excel |
| **Documentation** | Microsoft Word, PowerPoint |
| **Version Control** | Git, GitHub |


## 🔍 Methodology

### 1. Database Setup & Data Integration
- Imported two source tables: **Patient_Info** and **Lesion_Info**
- Created unified view (`lesion_patient_view`) combining demographics with clinical data
- Established 1:Many relationship (patients → lesions)

### 2. SQL Query Development
Developed **5 analytical queries** addressing specific business questions:

#### **Query 1: Environmental Risk Analysis**
```sql
SELECT
    CASE
        WHEN diagnostic IN ('MEL', 'SCC', 'BCC') THEN 'Malignant'
        WHEN diagnostic IN ('ACK') THEN 'Precancerous'
        ELSE 'Benign'
    END AS diagnosis_category,
    SUM(CASE WHEN pesticide = TRUE THEN 1 ELSE 0 END) * 100.0 / COUNT(*) AS pesticide_pct,
    COUNT(*) AS total_lesions
FROM lesion_patient_view
GROUP BY diagnosis_category;
```
**Finding:** 45% pesticide correlation with malignant lesions

#### **Query 2: Lesion Size Analysis**
```sql
SELECT
    diagnostic,
    ROUND(AVG(3.142 * diameter_1/2 * diameter_2/2)::numeric, 2) AS avg_area_mm2
FROM lesion_patient_view
GROUP BY diagnostic
ORDER BY avg_area_mm2 DESC;
```
**Finding:** Lesion size strongly predicts malignancy

#### **Query 3: Demographic Risk Profiling**
Analyzed smoking, drinking, and cancer history by age, gender, and diagnosis type

#### **Query 4: Symptom Analysis**
Mapped clinical symptoms (itching, bleeding, growth) to diagnosis confirmation rates

#### **Query 5: High-Risk Patient Identification**
```sql
SELECT COUNT(DISTINCT patient_id) AS high_risk_count
FROM lesion_patient_view
WHERE skin_cancer_history = TRUE
  AND age > 50
  AND pesticide = TRUE
  AND diagnostic IN ('BCC', 'MEL', 'SCC');
```
**Result:** 54 high-risk patients identified

### 3. Visualization & Reporting
- **Static Report (Page 1):** Visualizes SQL query results with analytical commentary
- **Interactive Dashboard (Page 2):** Power BI dashboard with dynamic filtering *(coming soon)*
- **Executive Presentation:** PowerPoint deck for stakeholder communication

---

## 💡 Business Recommendations

### Immediate Actions
1. **Clinical Protocols**
   - Prioritize biopsy for lesions >100mm² regardless of symptoms
   - Establish fast-track pathway for 54 identified high-risk patients
   - Address 366 unbiopsied ACK cases through systematic review

2. **Public Health Interventions**
   - Implement stricter pesticide safety regulations in high-exposure regions
   - Target health campaigns in areas lacking basic water/sewage infrastructure
   - Focus screening on males 50+ with smoking/drinking history

3. **Healthcare System Optimization**
   - Develop risk stratification tools based on lesion size + patient history
   - Create symptom-independent biopsy criteria for malignant-appearing lesions
   - Establish high-risk patient registries for systematic follow-up



## 📊 Dashboard Preview

### Page 1: SQL Analysis Results
![Power BI Dashboard](https://github.com/Fadekemi29/SQL_Projects/blob/main/Skin_Cancer_Analysis/DermAi_Skin_Cancer_Analysis_Dashboard.PNG)


**Static presentation** of PostgreSQL analytical findings:
- Environmental risk factor distribution by diagnosis type
- Lesion size comparison across cancer categories
- High-risk patient summary and geographic distribution

**Purpose:** Showcase SQL query development and analytical methodology

### Page 2: Interactive Data Explorer *(In Development)*
Dynamic dashboard with full interactivity:
- Real-time filtering across demographics, symptoms, and risk factors
- Cross-filtering between all visualizations
- User-driven data exploration tools

**Purpose:** Demonstrate Power BI proficiency and dashboard design skills

---

## 🎓 Skills Demonstrated

### Technical Skills
✅ **SQL Proficiency:** Complex queries using CTEs, window functions, CASE statements, aggregations  
✅ **Database Design:** Normalized schema, relationships, view creation  
✅ **Data Analysis:** Statistical analysis, correlation identification, cohort stratification  
✅ **Data Visualization:** Power BI, Excel charting, visual storytelling  
✅ **Healthcare Analytics:** HIPAA-aware data handling, clinical indicator analysis  

### Business Skills
✅ **Problem Definition:** Translated clinical questions into analytical frameworks  
✅ **Insight Generation:** Identified actionable patterns from raw data  
✅ **Stakeholder Communication:** Technical findings → business recommendations  
✅ **Documentation:** Comprehensive technical and executive-level reporting  


## 📈 Future Enhancements

### Phase 1: Current Project ✅
- SQL analysis complete
- Static visualizations created
- Documentation finalized
- [] Interactive dashboard (Page 2) - *In Progress*

### Phase 2: Machine Learning Integration
- Feature engineering for predictive models
- Classification model (malignant vs benign)
- Risk scoring algorithm
- Model deployment via API

### Phase 3: Advanced Analytics
- Time-series analysis (lesion growth patterns)
- Geographic clustering analysis
- Survival analysis integration
- Real-time alerting system


## 📄 Data Sources & Ethics

**Dataset:** Anonymized clinical data from 10Alytics learning platform  
**Sample Size:** 1,088 lesion cases from 1,088 unique patients  
**Privacy:** All personally identifiable information removed  
**Usage:** Educational and portfolio purposes only  

**Ethical Considerations:**
- Data fully anonymized before analysis
- No real patient identifiers included
- Findings presented at aggregate level only
- HIPAA-compliant data handling practices followed


## Full Access 

- [Executive Presentation]( https://view.officeapps.live.com/op/view.aspx?src=https%3A%2F%2Fraw.githubusercontent.com%2FFadekemi29%2FSQL_Projects%2Frefs%2Fheads%2Fmain%2FSkin_Cancer_Analysis%2FSkin%2520Cancer%2520Analysis%2520PPTX.pptx&wdOrigin=BROWSELINK)
- [Process Documentation](https://view.officeapps.live.com/op/view.aspx?src=https%3A%2F%2Fraw.githubusercontent.com%2FFadekemi29%2FSQL_Projects%2Frefs%2Fheads%2Fmain%2FSkin_Cancer_Analysis%2FDermAi%2520Diagnostic-%2520Skin%2520Cancer%2520Analysis.docx&wdOrigin=BROWSELINK)
- [Excel Visualisations](https://view.officeapps.live.com/op/view.aspx?src=https%3A%2F%2Fraw.githubusercontent.com%2FFadekemi29%2FSQL_Projects%2Frefs%2Fheads%2Fmain%2FSkin_Cancer_Analysis%2FSkin%2520Cancer%2520Analysis.xlsx&wdOrigin=BROWSELINK)
- [Raw Dataset]( https://github.com/Fadekemi29/SQL_Projects/blob/main/Skin_Cancer_Analysis/Skin_Cancer_Dataset) 
- [SQL Code]( https://github.com/Fadekemi29/SQL_Projects/blob/main/Skin_Cancer_Analysis/DermAI%20SQL.sql)




## 📫 Contact

**Fadekemi Adefemi**  
Data Analyst | Healthcare Analytics Specialist

📧 **Email:** Fadekemiadefemi22@gmail.com  
💼 **LinkedIn:** [linkedin.com/in/fadekemi-adefemi0129](https://www.linkedin.com/in/fadekemi-adefemi0129/)  
💻 **GitHub:** [github.com/Fadekemi29](https://github.com/Fadekemi29/)  
📍 **Location:** Wigan, UK

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.

---

## 🙏 Acknowledgments

- **10Alytics** - For providing the anonymized dataset and learning platform

---

⭐ **If you found this project helpful, please consider giving it a star!**

---

**Last Updated:** January 2025  
**Project Status:** 🟢 Active Development  
**Version:** 1.0 - SQL Analysis Complete | 2.0 - Interactive Dashboard (Coming Soon)
