# Introduction
Dive into the data job market! Focusing on data 
analyst roles, this explores top-paying jobs, 
in demands skills, and where high demand meets
high salary in data analytics.

SQL queries? Check them out here: [project_sql folder](/project_sql/).
# Background
Driven by a quest to navigate the data analyst job
market more effectively, this project was born
from a desire to pinpoint top-paid and in-demand
skills, streamlining others work to find optimal
jobs.

Data hails from my [SQL Course] (http://lukebarousse.com/sql). It's packed with insights
on job titles, salaries, location, and essential skills.

### The questions I wanted to answer through my SQL queries were:

1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analyst?
4. What skills are associated with higher salaries?
5. What are the most optimal skills to learn?
# Tools I Used
For my deep dive into the data analyst job market,
I harnessed the power of several kwy tools:

- **SQL:** The backbone of my analysis, allowing me to
query the database and unearth critical insights.
-**PostgreSQL:** The chosen database management
system, ideal for handling the job posting data.
- **Visual Studio Code:** My go-to for database management and executing SQL queries.
- **Git & Github:** Essential for version control and
collaboration andproject tracking.
# The Analysis
Each query for this project aimed at investigating
specific aspecst of the data analyst job market.
Here's how I approached each question:

### 1. Top paying Data Analyst Jobs
To identify the highest-paying roles, I filtered 
data analyst positions by average yearly salary
and location, focuses on remote jobs. This query
highlights the high paying opportunities in the
field.

```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id    
WHERE
    job_title_short = 'Data Analyst'  AND
    job_location = 'Anywhere'  AND
    salary_year_avg IS NOT NULL
ORDER BY 
    salary_year_avg DESC   
LIMIT 10
```
Here's the breakdown of the top data analyst jobs
in 2023:
- **Wide Salary Range:** Top 10 paying data
analyst roles span from $184,000 to $650,000,
indicating significant salary potential in the field.
- **Diverse Employers:** Companies like
SmartAsset, Meta, and AT&T are among those
offering high salaries, showing a broad interest
across different industries.
- **Job Title Variety:** There's a high diversity
in job titles, from Data Analyst to Director of Analytics, reflecting varied roles and
specializations within data analytics.

<img src="/assets/1_top_paying_roles.png" alt="Top Paying Roles" style="width:50%; height: auto;">
     
![Top Paying Roles](assets\1_top_paying_roles.png)
*Bar graph visualizing the salary for the top 10
salaries for data analyst; ChatGPT generated this graph from my SQL query results*

### 2. Skills for Top Paying Jobs
To understand what skills are required for the top-paying jobs, Ijoined the job postings with 
the skills data, providing insights into what 
employers value for high-compensation roles.
```sql
 WITH top_paying_jobs AS (

    SELECT
        job_id,
        job_title,      
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id    
    WHERE
       job_title_short = 'Data Analyst'  AND
        job_location = 'Anywhere'  AND
        salary_year_avg IS NOT NULL
    ORDER BY 
        salary_year_avg DESC   
    LIMIT 10
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY 
    salary_year_avg DESC
```
Here 's the breakdown of the most demanded skills for the top 10 highest paying data analyst
jobs in 2023.

- **SQL**is leading with a bold count of 8.
- **Python** follows closely with a bold count of 6.
- **Tableau** is also highly sought after, with a bold count of 6. Other skills like **R**,
**Snowflake**, **Pandas**, and **Excel** show varying degrees of demand.

 <img src="/assets/2_top_paying_job_skills.png" alt="Top Paying Roles" style="width:50%; height: auto;"> 

![Top Paying Roles](assets\2_top_paying_job_skills.png)
*Bar graph visualizing the count of skills for the top 10 paying jobs for data analysts; chatGPT generated this graph from my SQL query results.*

### 3. In-Demand Skills for Data Analysts
This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.
```sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' AND
    job_work_from_home = TRUE
GROUP BY
    skills
ORDER BY
    demand_count DESC    
LIMIT 5;
```
Here's the breakdown of the most demanded skills for data analysts in 2023.

- **SQL** and **Excel** remain fundamental, emphasizing the need for strong foundational skills in data processing and manipulation.
- **Programming** and **Visualization Tools** like **Python**, **Tableau**,  and **Power BI** are essential, pointing towards the increasing importance of technical skills in data storytelling and decision support.

| Skills    | Demand Count    |
|-----------|-----------------|
| SQL       | 7291            |
| Excel     | 4611            |
| Phyton    | 4330            |
| Tableau   | 3745            |
|Power BI   | 2609            |

*Table of the demand for the top 5 skills in data analyst job postings*

### 4. Skills Based on Salary
Exploring the average salaries associated with different skills revealed which skills are the highest paying.

```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL 
    AND job_work_from_home = TRUE
GROUP BY
    skills
ORDER BY
    avg_salary DESC        
LIMIT 25;
```

Here's a breakdown of the results for top paying skills for Data Analysts:


- **High Demand for Big Data & ML Skills**: Top salaries are commanded by analysts skilled in big data technology (Couchbase), machine learning tools (DataRobot, Jupiter), Phyton libraries (Pandas, Numpy), reflecting the valuation od data processing and predictive modeling capabilities.
- **Software Development & Deployment Proficiency**: Knoeledge in development and deployment tools (Airflow) indicates a lucrative crossover between data analysis and engineering, with a premium on skills that efficient data pipeline management.
- **Cloud Computing Expertise**: Familiarity with cloud and data engineering tools (Elasticsearch, Databricks, GCP) underscore the growing importance of cloud-based analytics enviroments, suggesting that cloud proficiency significantly boosts earning potential in data analytics.


### 5. Most Optimal Skills to learn
Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries, offering a strategic focus for skill development.

```sql
WITH skills_demand AS (
    SELECT
        skills_dim.skill_id,
        skills_dim.skills,
        COUNT(skills_job_dim.job_id) AS demand_count
    FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst' 
        AND salary_year_avg IS NOT NULL 
        AND job_work_from_home = TRUE
    GROUP BY
        skills_dim.skill_id
), average_salary AS (
    SELECT
        skills_job_dim.skill_id, 
        ROUND(AVG(salary_year_avg), 0) AS avg_salary
    FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst'
        AND salary_year_avg IS NOT NULL 
        AND job_work_from_home = TRUE
    GROUP BY
        skills_job_dim.skill_id          
)

SELECT
    skills_demand.skill_id,
    skills_demand.skills,
    demand_count,
    avg_salary
FROM
    skills_demand
INNER JOIN average_salary ON skills_demand.skill_id = average_salary.skill_id
WHERE
    demand_count > 10  
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25;
```

Here's a breakdown of the most optimal skills for Data Analyst in 2023:
- **High-Demand Programming Languages**: Python and R stand out for their high demand, with demand counts of 236 and 148 respectively. Despite their high demand, their average salaries are around $101,397 for Python and $100,499 for R, indicating that proficiency in these languages is highly valued but also widely available.
- **Cloud Tools and Technologies**: Skills in specialized technologies such as Snowflake, Azure, AWS, and Bigquery show significat demand with relatively high average salaries, pointing towards the growing importance of cloud platforms and big datatechnologies in data analysis.
- **Business Intelligence and Visualization Tools**: Tableau and Looker, with demand counts of 230 and 49 respectively, and average salaries around $99,288 and $103,795, highlight the critical role of data visualization and business intelligence in deriving actionable insights from data.
 - **Database Technologies**: The demand for skills in traditional and NoSQL database (Oracle, SQL Server, NoSQL) with average  


# What I learned
Throughout this adventure, I've turbocharged my SQL toolkit with some serious firepower:
- **Complex Query Crafting:** Mastered the art of advanced SQL, merging tables like a pro and wielding WITH clauses for ninja-level temp table maneuvers.
- **Data  aggregation:** Got cozy with GROUP BY and turned aggregate functions like COUNT() and AVG() into my data-summarizing sidekicks.
- **Analytical Wizardry:** Leveled up my real-world puzzle-solving skills, turning questions into actionable, insightful SQL queries.
# Conclusions

### Insights
1. **Top-Paying data analyst Jobs**: The highest-paying jobs for data analyst that allow remote work offer a wide range of salaries, the highest at $650,000.
2. **Skills for Top-Paying Jobs**: High-paying data analyst jobs require advanced proficienca in SQL, suggesting it's a critical skill for earning a top salary.
3. **Most In-Demand Skills**: SQL is also the most demanded skill in the data analyst job market, thus making it essential for job seekers.
4. **Skills with Higher Salaries**: Specialized skills, such as SVN and Solidity, are associated with the highest average salaries, indicating  a premiun on niche expertise.
5. **Optimal Skills for Job Market Value**: SQL leads in demand and offers for a high average salary, positioning it as one of the most optimal skills for data analysts to learn to maximize their market value.  

### Closing Thoughts
I would like to thank Luke Barousse for the course.
I have enhanced my SQL skills with this project. 

