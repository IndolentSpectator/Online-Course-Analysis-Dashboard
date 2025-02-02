# Online-Course-Analysis-Dashboard

## **Project Overview**
This Power BI dashboard was developed to analyze online course data from various EdTech platforms. The goal was to uncover insights on course categories, viewer preferences, instructor performance, and engagement metrics to help an EdTech startup optimize its recorded lecture offerings.

---

## **Business Understanding**
The EdTech startup required a data-driven approach to address the following key questions:
1. Which course categories and subcategories are most popular?
2. How do factors like language, subtitles, and course duration impact engagement?
3. Who are the top instructors in terms of ratings, and which skills are in demand?
4. What insights can viewer engagement patterns reveal for future course development?

---

## **Data Understanding**
- **Dataset**: The dataset included information on course categories, subcategories, number of views, ratings, instructor details, languages, subtitles, skills, and course durations.
- **Timeframe**: The dataset covered historical data from multiple EdTech platforms.
- **Challenges**:
  - Inconsistent formats in columns like "Ratings" and "Duration."
  - Missing values in critical columns like "Skills" (21%) and "Categories."
  - Multi-valued fields (e.g., "Instructors" and "Subtitles") requiring transformation.

### **Data Preparation and Transformations**
1. **Basic Cleaning**:
   - Removed duplicates, errors, and blank rows.
   - Standardized the "Ratings" column by extracting numeric values and converting to decimals.
   - Consolidated "Duration" into hours using Power Query.

   ```M
   if Text.Contains(Text.Lower([Duration]), "months") then 
       Number.FromText(Text.Select([Duration], {"0".."9"})) * 60
   else if Text.Contains(Text.Lower([Duration]), "hours") then 
       Number.FromText(Text.Select([Duration], {"0".."9"}))
   else if Text.Contains(Text.Lower([Duration]), "minutes") then 
       Number.FromText(Text.Select([Duration], {"0".."9"})) / 60
   else 
       200
   ```

2. **Splitting Multi-Valued Fields**:
   - Used Power Query to split and unpivot columns like "Instructors" and "Subtitles."

   ```M
   List.Count(Text.Split([Subtitle Languages], ","))
   ```

3. **Word Cloud Preparation**:
   - Cleaned the "Skills" column to remove stop words and replace spaces with underscores for better visualization.

---

## **Dashboard Features**

### **1. Course Type Popularity**
- **Visualization**: Stacked bar chart showing the number of courses by type (e.g., Courses, Specializations, Professional Certificates).
- **Insight**: Courses dominate the offering with 2,307 entries, while other types like Specializations and Professional Certificates are niche categories.

### **2. Viewers vs. Duration**
- **Visualization**: Line chart showing the average number of views based on course duration (in hours).
- **DAX Measure**:
   ```DAX
   Avg_Views = AVERAGE(Online_Courses[Number of Views])
   ```
- **Insight**: Courses in the 200–400-hour range receive higher engagement, indicating a preference for medium-length courses.

### **3. Language Distribution**
- **Visualization**: Pie chart showing the proportion of courses by language.
- **Insight**: English dominates with 93.7% of courses, followed by Spanish and other languages. This highlights a potential opportunity to diversify content for non-English-speaking audiences.

### **4. Subtitle Impact on Views**
- **Visualization**: Line chart displaying the average number of views against the number of subtitle languages available.
- **DAX Measure**:
   ```DAX
   Subtitle_Count = COUNTROWS(VALUES(Online_Courses[Subtitle Languages]))
   ```
- **Insight**: Courses with subtitles in multiple languages show significantly higher engagement, emphasizing the importance of accessibility.

### **5. Top 5 Categories by Viewer Engagement**
- **Visualization**: Matrix visualization showing the top 5 categories ranked by average views.
- **DAX Measure**:
   ```DAX
   Rank_Category_by_avg_views = 
   IF(
       RANKX(
           ALL(Online_Courses[Category]), 
           CALCULATE(AVERAGE(Online_Courses[Number of Views]))
       ) <= 5, 
       CALCULATE(AVERAGE(Online_Courses[Number of Views])), 
       BLANK()
   )
   ```
- **Insight**: Data Science and Computer Science lead in engagement, with significantly higher average views.

### **6. Instructor Ratings**
- **Visualization**: Static table listing top instructors by category, subcategory, and average ratings.
- **DAX Measure**:
   ```DAX
   Avg_Rating = AVERAGE(Online_Courses[Rating])
   ```
- **Insight**: The top-rated instructors, like David Hua and Ethan Mollick, can be targeted for content collaboration.

### **7. Skills in Demand**
- **Visualization**: Word cloud representing the most frequently taught skills.
- **Insight**: Skills like "Data Analysis," "Machine Learning," and "Programming" are highly sought after, aligning with current job market trends.

### **8. Average Skills per Category**
- **Visualization**: Table showing the average number of skills and course duration by category.
- **Insight**: Categories like Computer Science and Data Science offer the highest skill density, indicating their focus on comprehensive content.

---

### **Key Insights from the Online Courses Analysis Dashboard**

#### **1. Course Popularity Trends**
- **Insight**: The majority of courses offered are standalone courses (2,307 courses), with specializations (382) and professional certificates (4) comprising much smaller shares.
- **Implication**: There is a significant opportunity to expand offerings in professional certificates and specializations, as these could cater to learners seeking structured, in-depth learning paths.

#### **2. Viewer Engagement by Course Duration**
- **Insight**: Courses with a duration between **200–400 hours** attract the highest average views, with a notable spike at 260 hours (~25K average views).
- **Implication**: This indicates learner preference for medium-length courses. Shorter or excessively long courses may not sustain engagement.

#### **3. Language Distribution and Preferences**
- **Insight**: English is the dominant language, accounting for **93.7% of courses**, followed by Spanish (5.1%) and minor contributions from other languages.
- **Implication**: The current focus on English-based content excludes significant global audiences. Diversifying content into other languages, especially Spanish, could attract more learners.

#### **4. Subtitle Availability and Engagement**
- **Insight**: Courses with more subtitles tend to have higher engagement, peaking at courses offering subtitles in **25+ languages** (~86K average views).
- **Implication**: Subtitle availability significantly enhances course accessibility and engagement. Investing in multi-language subtitles can broaden the audience base.

#### **5. Top 5 Categories by Viewer Engagement**
- **Insight**: Data Science leads in average views (6,406.96), followed by Computer Science (4,475.47) and English (766.63).
- **Implication**: The high demand for STEM-related content, particularly Data Science and Computer Science, highlights the importance of prioritizing these areas for content expansion.

#### **6. Instructor Ratings**
- **Insight**: The top-rated instructors, such as **David Hua**, **Ethan Mollick**, and **Karthik Hosangadi**, have consistent ratings of **4.8**. These individuals represent high-quality, engaging educators.
- **Implication**: Partnering with these instructors can boost course credibility and attract learners.

#### **7. Skills in Demand**
- **Insight**: Skills like **Data Analysis**, **Machine Learning**, and **Programming** are the most frequently mentioned, aligning with market demands for data-driven roles.
- **Implication**: Developing courses centered on these skills will ensure relevance in today’s job market and increase enrollment.

#### **8. Average Skills and Duration by Category**
- **Insight**: Data Science courses offer the highest skill density (4.06 skills on average) and are typically around 61.92 hours long.
- **Implication**: This balance between skill variety and moderate duration may contribute to the category’s high engagement levels.

---

### **Scope for Further Investigation**

#### **1. Expanding Language Coverage**
- Analyze regional preferences to identify specific languages with high potential for engagement.
- Investigate whether non-English courses in popular categories (e.g., Data Science) show similar spikes in demand.

#### **2. Subtitle Impact Analysis**
- Perform a detailed analysis of the relationship between subtitle languages and learner demographics.
- Investigate the impact of subtitle language availability on course completion rates.

#### **3. Course Duration and Drop-off Rates**
- Examine engagement patterns within courses to understand when learners drop off. This could help identify the optimal course length for maximizing completion rates.

#### **4. Instructor Insights**
- Analyze the teaching styles, course structures, or unique features of top-rated instructors to identify replicable best practices.
- Investigate whether the number of courses by top instructors correlates with their ratings and engagement levels.

#### **5. Skill Relevance**
- Cross-reference the identified in-demand skills with industry job postings to ensure alignment with market needs.
- Explore trends in emerging skills to pre-emptively develop courses in growing fields.

#### **6. Category-Specific Engagement Patterns**
- Investigate category-specific preferences for course types (e.g., certifications vs. standalone courses).
- Analyze whether specific subcategories (e.g., Algorithms in Computer Science) have different engagement patterns than others.

#### **7. Learner Demographics and Behavior**
- Integrate demographic data (age, region, profession) to better understand target audiences and their preferences.
- Investigate learner preferences for specific formats (video, reading materials, assessments).

#### **8. Pricing and Subscription Impact**
- Explore whether pricing models (free vs. paid) influence engagement patterns.
- Analyze subscription trends for specific categories to understand payment behaviors.

---

This detailed insights section provides actionable recommendations for improving course offerings while highlighting areas for additional analysis to enhance decision-making. Let me know if you’d like to expand on any of these points!

