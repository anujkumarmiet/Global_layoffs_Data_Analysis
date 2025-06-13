# 🧹 Global Layoffs Analysis with MySQL

This project explores global layoff trends using real-world data by performing **end-to-end data cleaning** and **exploratory data analysis** (EDA) in **MySQL**. The goal was to transform raw, messy data into structured insights that can inform decision-making and visualization.

## 📊 Project Workflow

### 🔧 1. Data Cleaning Using MySQL

Performed thorough cleaning in a staging table `layoffs_staging2`:

- **Removed Duplicates**  
  ```sql
  DELETE FROM layoffs_staging2
  WHERE row_num > 1;
  ```
- **Standardized Columns** (trimming spaces, fixing typos, and formatting dates)  
  ```sql
  UPDATE layoffs_staging2
  SET company = TRIM(company),
      industry = 'Crypto'
  WHERE industry = 'Crypto Currency';
  ```

- **Handled NULLs & Outliers**  
  ```sql
  UPDATE layoffs_staging2
  SET total_laid_off = 0
  WHERE total_laid_off IS NULL;
  ```

- **Converted Date Formats**  
  ```sql
  UPDATE layoffs_staging2
  SET date = STR_TO_DATE(date, '%m/%d/%Y');
  ```

- **Dropped Irrelevant Columns** and finalized dataset for analysis

---

### 📈 2. Exploratory Data Analysis (EDA)

Uncovered trends and patterns across geography, time, and industries.

#### Top Companies with Highest Layoffs (All Time)
```sql
SELECT company, SUM(total_laid_off) AS total
FROM layoffs_staging2
GROUP BY company
ORDER BY total DESC
LIMIT 10;
```

#### Layoffs by Year
```sql
SELECT YEAR(date) AS year, SUM(total_laid_off) AS total
FROM layoffs_staging2
GROUP BY year
ORDER BY year;
```

#### Rolling Monthly Layoffs
```sql
WITH Date_CTE AS (
  SELECT SUBSTRING(date, 1, 7) AS month, SUM(total_laid_off) AS total
  FROM layoffs_staging2
  GROUP BY month
)
SELECT month, 
       SUM(total) OVER (ORDER BY month) AS rolling_total
FROM Date_CTE;
```

---

## 📌 Key Insights

- **2022** saw the highest total layoffs globally.  
- **Tech & Retail** were the most affected industries.  
- **SF Bay Area** had the most layoffs by location.  
- Post-IPO companies accounted for the highest layoffs in terms of funding stage.

---

## 🛠 Tools Used

- **MySQL Workbench**  
- **SQL Queries** (Joins, CTEs, Window Functions)  
- **Data Wrangling & Cleaning**  
- **EDA & Reporting with SQL**


**#MySQL #DataCleaning #EDA #SQLProject #LayoffsAnalysis #AlexTheAnalystInspired**
