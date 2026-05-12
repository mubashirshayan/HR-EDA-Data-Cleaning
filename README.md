# HR Data Cleaning, Outlier Treatment & Exploratory Data Analysis (EDA)

## Project Overview
This project focuses on data cleaning, outlier treatment, and exploratory data analysis (EDA) of a large HR dataset containing over 2 million employee records with intentional data quality issues.

## Dataset Characteristics
- **Size**: ~2 million rows with 15 columns
- **Data Quality**: Contains 48,000+ logical inconsistencies including:
  - **Age-Experience Gaps**: Employees younger than their years of experience + education years
  - **Invalid Salaries**: Negative or zero salary values
  - **Hierarchical Errors**: Junior employees with higher salaries than Directors
  - **Missing Values**: Strategic NaNs in performance ratings and departments

## Key Columns
- Employee_ID
- Full_Name
- Department
- Job_Title
- Hire_Date
- Performance_Rating
- Experience_Years
- Status (Active/Resigned)
- Work_Mode (On-site/Remote/Hybrid)
- Salary
- Year
- Country
- City
- Age
- Job_Level (Junior/Mid/Senior/Director)

## Data Processing Pipeline

### 1. **Data Loading & Type Casting**
- Loaded HR raw dataset from CSV
- Converted Hire_Date to datetime format
- Converted Salary to int64 type
- Removed duplicate rows

### 2. **Handling Missing Values**
- Identified 3,333 missing values in Performance_Rating column
- Used stratified random sampling by department to fill missing performance ratings

### 3. **Experience Years Correction**
- Applied multi-level median imputation strategy
- Used Department and Job_Level as grouping variables
- Set Experience_Years = Age - 22 (assuming age at employment)
- Replaced invalid values (< 22) with department and job level medians

### 4. **Outlier Treatment - IQR Method**
- **Methodology**: Interquartile Range (IQR) technique
- Identified outliers: Values > Q3 + 1.5*IQR or < Q1 - 1.5*IQR
- **Q1 (25th percentile)**: ~50,470
- **Q3 (75th percentile)**: ~100,940
- **IQR**: 50,470
- **Lower Bound**: Negative (no lower outliers)
- **Upper Bound**: ~176,165

- **Treatment Method**: Replaced outliers with group medians by Job_Title and Job_Level

### 5. **Performance Rating Imputation**
- **Technique**: Stratified Random Sampling
- Maintained distribution of performance ratings within each department
- Random selection based on probability distribution of existing ratings

## Data Quality Improvements

| Metric | Before | After |
|--------|--------|-------|
| Missing Performance Ratings | 3,333 | 0 |
| Invalid Salaries (≤0) | Multiple | 0 |
| Experience-Age Inconsistencies | Many | 0 |
| Salary Outliers (>1.5*IQR) | ~102,715 | Corrected |

## Key Findings from EDA

### Salary Distribution
- **Min**: 40,010
- **Max**: 239,627
- **Median**: 90,033 - 147,627 (varies by job level)
- **Distribution**: More concentrated after outlier treatment

### Age & Experience
- **Age Range**: 22-58 years
- **Experience Range**: 0-36 years
- **Correlation**: Strong positive correlation between age and experience years

### Department Distribution
- Multiple departments: Sales, IT, Finance, Operations, HR
- Salary variance across departments and job levels

### Job Level Analysis
- **Junior**: Lower salary range (40K-67K)
- **Mid**: Mid-range salaries (70K-115K)
- **Senior**: Higher salaries (109K-179K)
- **Director**: Highest salaries (239K+)

## Techniques Applied

1. **Data Cleaning**
   - Duplicate removal
   - Type conversion
   - Missing value handling

2. **Outlier Detection & Treatment**
   - IQR (Interquartile Range) method
   - Group-wise median imputation
   - Stratified analysis by Job_Title and Job_Level

3. **Missing Data Imputation**
   - Stratified Random Sampling for categorical variables
   - Median imputation for numerical variables
   - Department-level stratification

## Technologies Used
- **Python** 3.x
- **Pandas**: Data manipulation and analysis
- **NumPy**: Numerical computations
- **Matplotlib & Seaborn**: Data visualization
- **Jupyter Notebook**: Interactive analysis

## Files
- `hr-eda-data-cleaning-iqr-median-imputation.ipynb` - Main notebook with full analysis

## How to Run
1. Ensure Python and required libraries are installed
2. Load the HR raw dataset from Kaggle
3. Run the notebook cells sequentially
4. Review visualizations and statistical summaries

## Data Source
Kaggle: HR Dataset Clean and Raw (2M+ rows)
- Raw Dataset: `hr_raw.csv`
- Intentionally contains logical inconsistencies for testing analyst skills

## Key Insights
- Data cleaning reduced logical inconsistencies from 48,000+ to 0
- IQR-based outlier treatment preserved data distribution while removing anomalies
- Stratified sampling maintained departmental performance rating distributions
- Multi-level median filling preserved hierarchical salary structure

## Future Analysis Opportunities
- Predictive modeling for employee attrition
- Salary equity analysis by department and role
- Performance prediction models
- Career progression analysis
- Geographic salary analysis

---
**Author**: Data Analysis Project  
**Date**: May 2026  
**Tools**: Python, Pandas, NumPy, Matplotlib, Seaborn
