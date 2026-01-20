# 📂Overview

Welcome to my exploration of the data analyst job market. This project stems from a practical need: understanding what makes a data analyst competitive in today's employment landscape. I investigate which skills command the highest salaries and which are most desired by employers.

The analysis utilizes data from <a href="https://lukebarousse.com/python" rel="nofollow">Luke Barousse's Python Course</a>, offering rich details on job postings including titles, compensation, locations, and required competencies. Through Python-driven analysis, I tackle fundamental questions about skill demand, salary distributions, and the sweet spot where market need meets financial reward.

# 🔧Questions Answered
Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

# 🧰Tools Used for the Analysis
This analysis leverages several key tools to examine the data analyst job market:

**Python:** The foundation of the analysis, enabling data manipulation and insight extraction. Key libraries include:
- **Pandas:** For data cleaning, transformation, and analysis
- **Matplotlib:** For creating data visualizations
- **Seaborn:** For generating advanced statistical graphics

**Jupyter Notebooks:** The primary environment for running Python scripts, allowing seamless integration of code, visualizations, and narrative analysis.

**Visual Studio Code:** The integrated development environment (IDE) used for writing and executing Python scripts.

**Git & GitHub:** Version control and project sharing platforms that enable code management, collaboration, and project documentation.

# 🧼Data Preparation and Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

# 🧹Importing & Cleaning Up the Data

The analysis begins with importing required libraries and loading the dataset, followed by data cleaning procedures to ensure accuracy and consistency.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```
# 📍Filtering Specifically for Jobs in the US

I filter the dataset to focus exclusively on US-based positions, ensuring the analysis reflects the American job market.

```python
df_US = df[df['job_country'] == 'United States']
```

# 📊The Analysis
Each Jupyter notebook addresses a specific aspect of the data job market. Here's the approach taken for each analysis:

## 1. What are the most in-demand skills for the top 3 most popular data roles?

To identify the most in-demand skills for the top 3 data roles, I filtered job postings by popularity and extracted the top 5 skills for each role. This analysis reveals which skills are most critical for each position, helping inform targeted skill development based on career goals.

View my notebook with detailed steps right here: [2_Skills_Demand.ipynb](2_Project\2_Skills_Count.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

### The Results

![Visualization of the Top Skills for Data Nerds](2_Project\images\In_Demand_Skills_of_all_Data_Roles.png)

### Here are the key insights from this visualization:

- **SQL dominates across all roles** - It's the most or  second-most requested skill for Data Analysts (51%), Data Engineers (68%), and Data Scientists (51%)

- **Python is essential for technical roles** - Highly valued for Data Engineers (65%) and Data Scientists (72%), but less critical for Data Analysts (27%)

- **Clear role distinctions emerge**:
  - Data Analysts favor BI tools (Excel 41%, Tableau 28%) over programming
  - Data Engineers require cloud platforms (AWS 43%, Azure/Spark both 32%)
  - Data Scientists are heavily Python-focused with some SQL and SAS

- **Traditional tools persist** - Excel remains relevant for analysts (41%), while SAS appears across analyst and scientist roles (19-24%)

- **Cloud skills matter for engineers** - AWS, Azure, and Spark are specifically relevant to Data Engineering positions, not the other two roles

## 2. How are in-demand skills trending for Data Analysts?

To analyze skill trends for Data Analysts in 2023, I filtered for data analyst roles and aggregated skills by posting month. This revealed the top 5 skills each month, illustrating their popularity throughout the year.

View my notebook with detailed steps right here: [3_Skills_Trend.ipynb](2_Project\3_Skills_Trend.ipynb)


### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```

### The Results

![Trending Top Skills for Data Analysts in the US](2_Project\images\skills_trend_DA.png)
*Bar Graph Visualizing the Trending Top Skills for Data Analysts in the US in 2023*

### Here are the key insights from this visualization: 

- **SQL remains the dominant skill** - Consistently the most requested skill throughout 2023, though showing a gradual decline from ~63% in January to ~53% by December

- **Excel shows significant volatility** - Spiked to ~45% in July-August before dropping sharply to ~34% in October-November, then recovering to ~40% by year-end

- **Python, Tableau, and Power BI cluster in mid-range** - Python and Tableau converge around 30-35% by year-end, while Power BI maintains the most stable trend at ~20-23% throughout the year

- **Overall demand softening** - Most skills show slight downward trends in the latter half of 2023, with SQL declining from peaks above 60% and traditional tools still outpacing modern analytics platforms

## 3. How well do jobs and skills pay for Data Analysts?

## Salary Analysis for Data Nerds

To identify the highest-paying roles and skills, I filtered for US-based positions and analyzed median salaries. I began by examining salary distributions across common data positions—Data Scientist, Data Engineer, and Data Analyst—to determine which roles command the highest compensation.

View my notebook with detailed steps right here: [4_Salary_Analysis.ipynb](2_Project\4_Salary_Analysis.ipynb)


### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

### The Results

![Salary Distribution for Data Jobs in th US](2_Project\images\salary_boxplot.png)
*Box plot visualizing the salary distributions for the top 6 data job titles.*

### Here are the key insights from this visualization: 

- **Senior roles command premium salaries** - Senior Data Scientists and Senior Data Engineers have median salaries around $175K-$200K, with upper ranges extending beyond $500K

- **Clear salary hierarchy exists** - Scientist roles consistently earn more than Engineer roles at the same level, and both significantly outpace Analyst positions

- **Data Analyst salaries are most constrained** - Median around $90K-$100K with fewer high-end outliers, and Senior Data Analysts show only modest increases to ~$120K median

- **Wide salary variability at senior levels** - Senior positions show extensive outlier distributions, particularly for Data Scientists and Engineers, indicating diverse compensation packages based on company, location, and specialization

- **Experience premium is substantial** - The gap between entry-level and senior positions can exceed $75K-$100K in median salary, with even larger differences at the high end of the distribution

### Highest Paid & Most Demanded Skills for Data Analysts

I then narrowed the analysis to Data Analyst positions specifically, examining both the highest-paid skills and the most in-demand skills. Two bar charts were used to visualize these distinct skill categories.

```python
fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```

![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](2_Project\images\Highest_Paid_and_In_Demand_Skills_for_Data_Analysts_in_the_US.png)
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US.*

### The Results

### Here are the key insights from this visualization:

- **Specialized technical skills command premium salaries** - Advanced tools like <code>dplyr</code> ($200K), <code>bitbucket</code> ($190K), and <code>gitlab</code> ($185K), along with cloud/DevOps platforms (Hugging Face, Couchbase, Ansible, VMware at $150K+) offer significant salary premiums over mainstream skills

- **Demand doesn't equal compensation** - The most in-demand skills (Python, Tableau, R, SQL Server, SQL, SAS, Power BI, Excel) cluster around $90K-$100K median salary, substantially lower than specialized technical skills

- **Strategic skill selection matters** - Learning high-paid niche skills in MLOps, version control, and cloud infrastructure can potentially double earning potential compared to focusing solely on foundational analytics tools

## 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills (those offering both high compensation and strong demand) I analyzed the percentage of jobs requiring each skill alongside their median salaries. This dual-metric approach reveals which skills provide the best return on investment for skill development.

View my notebook with detailed steps right here: [5_Optimal_Skills.ipynb](2_Project\5_Optimal_Skills.ipynb)

### Visualize Data

```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()
```

### The Results

![Most Optimal Skills for Data Analysts in the US](2_Project\images\Most_Optimal_Skills_for_Data_Analysts_in_the_US_With_Coloring__by_Technology.png)
*A Scatter Plot Visualizing the Most Optimal Skills (High Paying & High Demand) for Data Analysts in the US.*

### Here are the key insights from this visualization:

- **SQL is the ultimate optimal skill** - Appears in ~58% of job postings with a $90K median salary, offering the best combination of widespread demand and solid compensation

- **Python and Tableau offer strong ROI** - Python (~33% demand, $97K salary) and Tableau (~32% demand, $92K salary) provide excellent balance of market demand and above-average compensation

- **Traditional tools show diminishing returns** - While Excel (~43% demand) appears frequently, it offers lower median salary ($85K), and Word/PowerPoint have both low demand and compensation, making them less strategic skill investments

- **Specialized skills command premiums but limited opportunities** - Oracle ($98K) and SQL Server ($92K) offer higher salaries but appear in fewer postings (5-7%), representing niche opportunities rather than foundational investments

# 🔍Key Takeaways

This project deepened my understanding of the data analyst job market while significantly enhancing my Python proficiency, particularly in data manipulation and visualization. Several key insights emerged:

- **Enhanced Python Skills:** Deep work with Pandas, Seaborn, and Matplotlib improved my ability to manipulate large datasets and create compelling visualizations efficiently.

- **Data Quality is Foundational:** Rigorous data cleaning and preparation proved essential for generating reliable insights—poor data quality inevitably leads to flawed conclusions.

- **Strategic Career Planning:** The analysis reinforced that successful career development requires aligning personal skills with market realities. Understanding how demand, compensation, and availability intersect enables more informed decisions about skill acquisition and career direction.

# 📚Key Insights

This analysis revealed several important patterns in the data analyst job market:

- **Demand Drives Compensation:** A strong relationship exists between skill demand and salary levels. Specialized technical skills such as Python and Oracle consistently command premium compensation, reflecting their value to employers.

- **Evolving Skill Landscape:** The data analytics field demonstrates continuous evolution in skill requirements. Staying current with emerging trends is critical for maintaining competitiveness and advancing in the profession.

- **Strategic Skill Investment:** Identifying skills that offer both high market demand and strong compensation enables data analysts to make informed decisions about professional development, maximizing both career opportunities and earning potential.

# 💪Challenges Encountered

Several obstacles shaped the learning process:

- **Inconsistent Data:** Missing and irregular data entries required extensive cleaning and validation, highlighting how data quality directly impacts analysis credibility.

- **Effective Visualization:** Creating visualizations that were both informative and accessible proved challenging. Complex data needed to be distilled into understandable graphics without oversimplifying.

- **Analysis Depth:** Balancing detailed investigation with broad market overview required discipline—going too deep risked losing perspective, while staying too shallow missed important nuances.

# 🏁Conclusion

This analysis of the data analyst job market reveals the critical skills and emerging trends shaping the profession. Beyond deepening my understanding of the field, this project offers practical guidance for data professionals seeking to advance their careers strategically.

The data analytics landscape continues to evolve rapidly, making ongoing market analysis essential for staying competitive. This project establishes a framework for future investigations and reinforces a fundamental truth: success in data analytics requires commitment to continuous learning and adaptation.

