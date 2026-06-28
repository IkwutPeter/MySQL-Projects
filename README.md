# 🦠 COVID-19 Global Layoffs Analysis
### Data Cleaning & Exploratory Data Analysis with MySQL

![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Data Cleaning](https://img.shields.io/badge/Data%20Cleaning-0D6EFD?style=for-the-badge)
![EDA](https://img.shields.io/badge/Exploratory%20Data%20Analysis-6C757D?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-2E8B57?style=for-the-badge)

---

## 📌 Project Overview

This project performs a comprehensive **data cleaning** and **exploratory data analysis (EDA)** on a global dataset tracking workforce layoffs during and after the COVID-19 pandemic. The goal was to uncover patterns in how the pandemic disrupted employment across industries, geographies, and time.

The analysis highlights the scale of global economic disruption and surfaces actionable insights about which sectors and regions were hit hardest — and when.

---

## 🎯 Objectives

- Clean and standardise a raw, messy layoffs dataset for reliable analysis
- Identify companies with the highest total and percentage layoffs
- Uncover geographic patterns — which countries bore the greatest burden
- Track layoff trends across years to map the pandemic's economic timeline
- Rank industries and funding stages most vulnerable to workforce reduction

---

## 🔍 Key Insights

| Insight | Finding |
|---|---|
| 📊 Total percentage laid off | Identified companies with 100% workforce reductions |
| 🏢 Top companies | Ranked by absolute and relative layoff volume |
| 🌍 Country analysis | Mapped layoff concentration by nation |
| 📅 Yearly trends | Tracked pandemic economic impact over time |
| 🏭 Industry breakdown | Revealed sectors most affected |

---

## 🛠️ Methods & Techniques

**Data Cleaning**
- Removed duplicates using `ROW_NUMBER()` window functions
- Standardised inconsistent company names, industry labels, and country entries
- Handled NULL values and blank fields systematically
- Converted and validated date formats

**Exploratory Data Analysis**
- Aggregation queries using `GROUP BY`, `ORDER BY`, `SUM()`, `MAX()`
- Rolling totals with `SUM() OVER (PARTITION BY ... ORDER BY ...)`
- Year and month extraction for time-series analysis
- Ranking queries using `DENSE_RANK()` to identify top companies per year

---

## 💻 SQL Techniques Used

```sql
-- Example: Rolling total of layoffs by month
SELECT 
    SUBSTRING(date, 1, 7) AS month,
    SUM(total_laid_off) AS total_off,
    SUM(SUM(total_laid_off)) OVER (ORDER BY SUBSTRING(date, 1, 7)) AS rolling_total
FROM layoffs_staging2
WHERE SUBSTRING(date, 1, 7) IS NOT NULL
GROUP BY month
ORDER BY month;
```

```sql
-- Example: Top 5 companies by layoffs per year
WITH Company_Year AS (
    SELECT company, YEAR(date) AS years, SUM(total_laid_off) AS total_laid_off
    FROM layoffs_staging2
    GROUP BY company, YEAR(date)
),
Company_Year_Rank AS (
    SELECT *, DENSE_RANK() OVER (PARTITION BY years ORDER BY total_laid_off DESC) AS ranking
    FROM Company_Year
    WHERE years IS NOT NULL
)
SELECT * FROM Company_Year_Rank
WHERE ranking <= 5;
```

---

## 📁 Repository Structure

```
MySQL-Projects/
│
├── data_cleaning.sql        # Full data cleaning pipeline
├── exploratory_analysis.sql # EDA queries and insights
└── README.md                # Project documentation
```

---

## 👤 Author

**Peter Rowland Ukotije-Ikwut, PhD (CEnv, MIEnvSc)**
Researcher | Environmental Scientist | AI & Data Practitioner

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/peter-rowland-ukotije-ikwut-ph-d-cenv-mienvsc-84241368)
[![GitHub](https://img.shields.io/badge/GitHub-Portfolio-181717?style=flat-square&logo=github)](https://github.com/IkwutPeter)
[![Google Scholar](https://img.shields.io/badge/Google%20Scholar-Publications-4285F4?style=flat-square&logo=google-scholar&logoColor=white)](https://scholar.google.com/citations?user=d9soCH0AAAAJ&hl=en)

---

*Part of a broader data analytics portfolio bridging environmental science and AI-driven insight.*
