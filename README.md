# Bellabeat Data Analysis

Analysis of Bellabeat smart device data to uncover user activity and sleep patterns.

## Table of Contents
1. Introduction
2. Data Description
3. Data Cleaning and Preparation
4. Exploratory Data Analysis
5. Correlation and Insights
6. Visualizations
7. Conclusions and Recommendations
8. Appendix


## 1. Introduction

This case study analyzes smart device usage data from Bellabeat, a high-growth health and wellness company that manufactures fitness trackers and related products. The objective of this analysis was to explore patterns in users’ daily physical activity and sleep behavior and to identify actionable insights that could support product development, marketing strategies, and user engagement initiatives.
The dataset includes metrics such as total steps, calories burned, active minutes, and sleep duration collected from Bellabeat devices over a specified period. Using R for data cleaning, transformation, and visualization, I examined the relationships between activity levels and sleep patterns, identified trends in user engagement. I developed recommendations to drive higher adoption and retention of Bellabeat products.


## 2. Data Description

The dataset used in this analysis was made publicly available as part of the Bellabeat case study and contains anonymized activity tracking data collected via FitBit devices, serving as a proxy for Bellabeat users.

**Source:**  
FitBit Fitness Tracker Data (CC0: Public Domain)

**Time Period:**  
April 12, 2016 – May 12, 2016 (approximately 1 month)

**Records:**  
33 unique users with daily tracking records.

**Key Variables:**  
- `id`: Unique identifier for each user
- `activity_date`: The date of the record
- `total_steps`: Number of steps taken in a day
- `total_distance`: Distance walked in miles
- `calories`: Total calories burned
- `very_active_minutes`: Minutes of intense activity
- `fairly_active_minutes`: Minutes of moderate activity
- `lightly_active_minutes`: Minutes of light activity
- `sedentary_minutes`: Minutes inactive
- `total_minutes_asleep`: Total sleep duration in minutes
- `total_time_in_bed`: Time spent in bed

**Limitations:**  
- Small sample size (33 users) may limit generalizability.
- Sleep data was not consistently recorded for all users.
- No demographic information included to enable segmentation by age or gender.


## 3. Data Cleaning and Preparation

I used R to clean, transform, and prepare the Bellabeat dataset. The following steps were performed to ensure the data was ready for analysis:

**Tools and Packages:**
- R programming language
- `tidyverse` for data manipulation and visualization
- `janitor` for cleaning column names
- `lubridate` for handling date and time formats

**Steps:**

- **Loading Data:**
  - Imported `dailyActivity_merged.csv` and `sleepDay_merged.csv`.
  - Cleaned column names with `janitor::clean_names()`.

- **Date Conversion:**
  - Converted `activity_date` to Date objects using `mdy()`.
  - Converted `sleep_day` to Date objects using `mdy_hms()` followed by `as.Date()` to match formats.

- **Duplicate Removal:**
  - Identified and removed duplicate rows with `distinct()`.

- **Merging Data:**
  - Performed a left join to combine daily activity and sleep datasets on `id` and date fields.
  - Resulting dataset contained 863 records.

- **Filtering:**
  - Removed records where both `total_steps` and `total_distance` were zero.

- **Handling Missing Values:**
  - Noted that some records did not have sleep data (`NA` values). These were retained for activity analyses but excluded from sleep-related calculations and plots.

- **Feature Engineering:**
  - Created a `day_of_week` variable to analyze step patterns by weekday.

- **Exporting Clean Data:**
  - Saved the cleaned dataset as `cleaned_merged_data.csv` for reproducibility.

The cleaned and merged dataset provided the foundation for subsequent exploratory data analysis and visualization.


## 4. Exploratory Data Analysis

I conducted exploratory data analysis (EDA) to understand user behavior, activity levels, and sleep patterns. The analysis included descriptive statistics, visualizations, and correlation assessments to identify trends and relationships in the data.

**Summary Statistics:**

- The number of steps per day ranged from 4 to 36,019, with a median of ~8,053 steps.
- The total distance traveled per day ranged from 0 to ~28 miles.
- Daily calories burned ranged from ~52 to ~4,900.
- Sleep duration ranged from 58 minutes to ~13 hours (where recorded).
- A significant portion of records lacked sleep data (approximately 53% of days).

**Daily Steps Distribution:**

- Created histograms showing the distribution of total steps per day.
- Most users logged between ~5,000 and ~11,000 daily steps.

**Steps by Day of Week:**

- Computed average steps by weekday.
- The highest average steps were observed on Tuesdays and Saturdays (~8,950 steps).
- Sundays had the lowest average steps (~7,600 steps).

**Correlation Analysis:**

- Calculated a correlation matrix among numeric variables.
- A moderate positive correlation (r ≈ 0.56) was found between `total_steps` and `calories`, indicating that higher activity levels were associated with higher energy expenditure.
- Generated a correlation heatmap using `corrplot`.

**Activity and Sleep Relationship:**

- Filtered records with valid sleep data to explore the relationship between activity and sleep duration.
- Created boxplots to visualize the distribution of sleep duration in hours.
- Found variability in sleep duration across the sample.

**Visualizations Created:**

- Histogram of daily steps (![distribution_of_daily_steps](https://github.com/user-attachments/assets/988eb118-8429-496b-ba7f-0194d36d6ce9))
- Scatter plot of steps vs. calories burned with a linear trendline (![steps_vs_cals_burned](https://github.com/user-attachments/assets/91509be7-e416-454b-9d59-7fd6b2ff4b6a))
- Bar chart of average steps by day of the week (![avg_daily_steps_by_day_of_wk](https://github.com/user-attachments/assets/a320473c-05cb-42f5-a3f5-eb658283e6e6))
- Boxplot of sleep duration distribution (![sleep_duration_dist](https://github.com/user-attachments/assets/bc622955-29dc-4119-967e-b8409a9fa632))
- Correlation heatmap among numeric variables (![corr_matrix_of_variables](https://github.com/user-attachments/assets/05c7c6b6-99a8-4ea2-81df-c4ea3d9ee2f5))

These analyses provided insights into typical activity and sleep behaviors among users, highlighting relationships between key metrics.


## 5. Correlation and Insights

This section summarizes the key patterns and relationships observed in the data and visualizations.

**Steps and Calories:**
- A moderate positive correlation (~0.56) was identified between `total_steps` and `calories` burned.
- This indicates that higher daily step counts are associated with higher energy expenditure, as expected.

**Steps by Day of the Week:**
- Average daily steps were highest on **Tuesdays** and **Saturdays** (~8,950 steps) and lowest on **Sundays** (~7,600 steps).
- This pattern suggests that users tend to be more active earlier in the week and on weekends.

**Sleep Duration:**
- The distribution of sleep duration showed variability across users, ranging from ~1 hour to ~13 hours.
- Approximately 53% of records lacked sleep data, indicating either incomplete tracking or less engagement with sleep monitoring features.

**Correlation Heatmap:**
- The correlation matrix highlighted:
  - Strong correlation between `total_distance` and `total_steps`, reflecting that distance is largely determined by step count.
  - Positive correlation between `very_active_minutes` and both `total_steps` and `calories`.
  - No strong correlations were found between activity variables and sleep duration in this dataset.

**Engagement Trends:**
- Many users consistently logged steps and calories but were inconsistent with sleep tracking.
- This gap presents an opportunity for targeted interventions to increase engagement with sleep monitoring.

These insights informed the recommendations and potential actions described in the next section.
