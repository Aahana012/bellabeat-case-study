# Bellabeat Data Analysis

Analysis of Bellabeat smart device data to uncover user activity and sleep patterns.

## Table of Contents
1. Introduction
2. Data Description
3. Data Cleaning and Preparation
4. Exploratory Data Analysis
5. Correlation, Insights, and Visualizations
6. Conclusions and Recommendations
7. Appendix


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

- Histogram of daily steps ![distribution_of_daily_steps](https://github.com/user-attachments/assets/988eb118-8429-496b-ba7f-0194d36d6ce9)
- Scatter plot of steps vs. calories burned with a linear trendline ![steps_vs_cals_burned](https://github.com/user-attachments/assets/91509be7-e416-454b-9d59-7fd6b2ff4b6a)
- Bar chart of average steps by day of the week ![avg_daily_steps_by_day_of_wk](https://github.com/user-attachments/assets/a320473c-05cb-42f5-a3f5-eb658283e6e6)
- Boxplot of sleep duration distribution ![sleep_duration_dist](https://github.com/user-attachments/assets/bc622955-29dc-4119-967e-b8409a9fa632)
- Correlation heatmap among numeric variables ![corr_matrix_of_variables](https://github.com/user-attachments/assets/05c7c6b6-99a8-4ea2-81df-c4ea3d9ee2f5)

These analyses provided insights into typical activity and sleep behaviors among users, highlighting relationships between key metrics.


## 5. Correlation, Insights, and Visualizations

This section summarizes the key patterns and relationships observed in the data, illustrated with supporting visualizations.

---

### Histogram of Daily Steps

**CODE SNIPPET:**
ggplot(analyzed_data, aes(x = total_steps)) +
  geom_histogram(binwidth = 2000, fill = "skyblue", color = "black") +
  labs(
    title = "Distribution of Daily Steps",
    x = "Total Steps per Day",
    y = "Count of Days"
  ) +
  theme_minimal()


**PLOT:**
![distribution_of_daily_steps](https://github.com/user-attachments/assets/988eb118-8429-496b-ba7f-0194d36d6ce9)

**FINDING:** The distribution of daily steps is right-skewed, with most users recording between **4,000 and 10,000 steps per day**. A small subset consistently achieved over 15,000 steps, suggesting varying levels of activity among users.

---

### Scatter Plot of Steps vs. Calories Burned

**CODE SNIPPET:**
cor(analyzed_data$total_steps, analyzed_data$calories, use = "complete.obs")
	[1] 0.5619815
ggplot(analyzed_data, aes(x = total_steps, y = calories)) +
+     geom_point(alpha = 0.6) +
+     geom_smooth(method = "lm", se = FALSE, color = "blue") +
+     labs(title = "Steps vs Calories Burned", x = "Total Steps", y = "Calories")


**PLOT:**
![steps_vs_cals_burned](https://github.com/user-attachments/assets/91509be7-e416-454b-9d59-7fd6b2ff4b6a)

**FINDING:** There is a **moderate positive correlation (~0.56)** between daily steps and calories burned. As expected, higher step counts are associated with greater caloric expenditure.

---

### Bar Chart of Average Steps by Day of the Week

**CODE SNIPPET:**
analyzed_data %>%
  mutate(day_of_week = weekdays(as.Date(activity_date))) %>%
  group_by(day_of_week) %>%
  summarise(mean_steps = mean(total_steps, na.rm = TRUE)) %>%
  ggplot(aes(x = reorder(day_of_week, -mean_steps), y = mean_steps)) +
  geom_bar(stat = "identity", fill = "lightgreen") +
  labs(
    title = "Average Daily Steps by Day of Week",
    x = "Day of Week",
    y = "Average Steps"
  ) +
  theme_minimal()


**PLOT:**
![avg_daily_steps_by_day_of_wk](https://github.com/user-attachments/assets/a320473c-05cb-42f5-a3f5-eb658283e6e6)

**FINDING:** Average daily steps peaked on **Tuesdays and Saturdays (~8,950 steps)**, with the lowest activity on **Sundays (~7,600 steps)**. This suggests users tend to be more active during the early week and weekends.

---

### Boxplot of Sleep Duration Distribution

**CODE SNIPPET:**
summary(merged_data$total_minutes_asleep)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   58.0   361.0   432.5   419.2   490.0   796.0     453 
> ggplot(merged_data %>% filter(!is.na(total_minutes_asleep)),
+        aes(x = "Sleep", y = total_minutes_asleep / 60)) +
+     geom_boxplot(fill = "lightpink") +
+     labs(
+         title = "Sleep Duration Distribution",
+         x = "",
+         y = "Sleep Duration (Hours)"
+     ) +
+     theme_minimal()


**PLOT:**
![sleep_duration_dist](https://github.com/user-attachments/assets/bc622955-29dc-4119-967e-b8409a9fa632)

**FINDING:** Sleep duration varied substantially, ranging from approximately **1 to 13 hours per night**. The median sleep duration was around **7.2 hours**, but over half of the records lacked sleep data, highlighting inconsistent sleep tracking.

---

### Correlation Heatmap Among Numeric Variables

**CODE SNIPPET:**
corrplot(cor_matrix, method = "color",
+          type = "upper",
+          tl.cex = 0.7,
+          tl.col = "black",
+          tl.srt = 45,
+          addCoef.col = "black",
+          number.cex = 0.6,
+          diag = FALSE) 


**PLOT:**

![corr_matrix_of_variables](https://github.com/user-attachments/assets/5a1d0963-9592-43ed-8d46-a0cc6415e8b2)

**FINDING:** The correlation matrix shows:
- A **strong correlation between total distance and total steps (r ≈ 0.98)**, as steps primarily determine distance.
- Positive relationships between **very active minutes, total steps, and calories burned**.
- No strong correlation between sleep duration and activity metrics.

---

### Engagement Trends

Many users consistently logged steps and calories but were **less consistent in tracking sleep**. This presents an opportunity for targeted communication or app features to encourage more complete health monitoring.

---

### Recommendation

These insights inform targeted recommendations to improve user engagement and promote healthier behaviors, as detailed in the next section.


## 6. Conclusions and Recommendations

Based on the analysis of activity and sleep data, several important patterns and opportunities were identified:

---

### Key Conclusions

- **Activity Tracking Engagement:**  
  Most users regularly tracked steps and calories, with daily step counts clustering between 4,000–10,000 steps. This indicates a generally moderate level of engagement with activity monitoring.

- **Positive Relationship Between Steps and Calories Burned:**  
  A moderate positive correlation (~0.56) was observed, confirming that increased step counts are associated with higher energy expenditure.

- **Variation by Day of the Week:**  
  Users were most active on **Tuesdays and Saturdays**, and least active on **Sundays**. This suggests activity interventions or challenges could be more effective when scheduled early in the week or over the weekend.

- **Low Sleep Tracking Participation:**  
  Over half of records lacked sleep duration data, highlighting inconsistent use of sleep monitoring features.

- **Strong Correlations Among Activity Metrics:**  
  Total steps, distance, and active minutes were highly interrelated, reflecting expected relationships between intensity and caloric burn.

---

### Recommendations

To improve user engagement and promote healthier behaviors, the following actions are recommended:

1. **Promote Consistent Sleep Tracking:**  
   Develop targeted in-app reminders, educational content, or incentives to encourage users to log their sleep consistently.

2. **Leverage Weekly Activity Trends:**  
   Introduce personalized challenges or notifications to boost activity on lower-engagement days like Sundays.

3. **Highlight Step and Calorie Goals:**  
   Use visual feedback to reinforce the positive connection between steps and caloric expenditure, motivating users to meet or exceed daily goals.

4. **Enable Goal Customization:**  
   Allow users to set personalized step and sleep goals that align with their lifestyle and fitness levels.

5. **Provide Insights and Rewards:**  
   Offer periodic reports summarizing progress and recognizing milestones to reinforce positive habits and long-term engagement.

---

By implementing these recommendations, the platform can support users in maintaining consistent activity and sleep tracking, ultimately fostering healthier behaviors and improving overall user satisfaction.


## 7. Appendix

**Data Files**
- `dailyActivity_merged.csv`
- `sleepDay_merged.csv`
- `cleaned_merged_data.csv`

**R Packages**
- tidyverse
- janitor
- lubridate
- ggplot2
- corrplot

**Key Steps**
- Cleaned and merged data with `lubridate` for date parsing.
- Filtered rows with positive activity.
- Generated visualizations (histogram, scatter plot, boxplot, correlation heatmap).

**Example Code**

```r
# Load and merge
daily_activity <- read_csv("dailyActivity_merged.csv") %>%
  clean_names() %>%
  mutate(activity_date = mdy(activity_date))

sleep_day <- read_csv("sleepDay_merged.csv") %>%
  clean_names() %>%
  mutate(sleep_day = as.Date(mdy_hms(sleep_day)))

merged_data <- daily_activity %>%
  left_join(sleep_day, by = c("id", "activity_date" = "sleep_day")) %>%
  filter(total_steps > 0)
```

**End of report.**
