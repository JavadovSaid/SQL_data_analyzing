# Introduction
This project explores job market trends for top-paying roles, focusing on positions such as Data Scientist and Business Analyst.
The primary goal is to analyze salaries, skill demands, and trends in remote job opportunities using SQL queries.
The insights are aimed at professionals seeking to align their skills with high-demand, high-paying roles in the job market.

# Background
Understanding job market trends is critical for career planning and professional growth.
Companies are increasingly seeking specialized skills for remote roles,
particularly in data-driven domains like Data Science and Business Analysis.
By analyzing job postings data, this project sheds light on the most sought-after skills,
average salaries, and the demand for remote jobs in these fields.

### The questions i wanted to answer though my SQL queries were:
1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

# Tools i used

For my deep dive into the data analyst job market,
I harnessed the power of several key tools:

- **SQL:** The backbone of my analysis, allowing me to
 query the database and unearth critical insights.
- **PostgreSQL:** The chosen database management system,
 ideal for handling the job posting data.
- **Visual Studio Code:** My go-to for database
 management and executing SQL queries.
- **Git & GitHub:** Essential for version control and
 sharing my SQL scripts and analysis,
 ensuring collaboration and project tracking.

# The Analysis
Key Analyses Performed:

### 1. Top-Paying Data Scientist Jobs:

Identified the top 10 highest-paying Data Scientist roles in the U.S., categorized by company and job title.
Enriched the dataset by including required skills for these roles.

```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_posted_date,
    salary_year_avg,
    name as company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON
    job_postings_fact.company_id = company_dim.company_id
WHERE
    job_location = 'United States' AND
    job_title_short = 'Data Scientist' AND
    salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10;
```


### 2. In-Demand Skills for Remote Roles:

Explored the most in-demand skills for remote Data Scientist and Business Analyst jobs.
Ranked skills by demand count and linked them with average salaries.

```sql
WITH top_paying_jobs as (
    SELECT
        job_id,
        job_title,
        job_location,
        job_posted_date,
        salary_year_avg,
        name as company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON
        job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_location = 'United States' AND
        job_title_short = 'Data Scientist' AND
        salary_year_avg IS NOT NULL
    ORDER BY salary_year_avg DESC
    LIMIT 10
)

SELECT
    top_paying_jobs.*,
    skills
FROM
    top_paying_jobs
INNER JOIN skills_job_dim ON
    top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON
    skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY salary_year_avg DESC;
```
### 3. Salary Trends by Skills:

Analyzed the average salary associated with specific skills for remote Business Analyst jobs.
Highlighted top skills with significant salary potential.

