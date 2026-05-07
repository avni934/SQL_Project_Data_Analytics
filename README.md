# SQL Data Job Market Analysis 📊

## Introduction
This project explores the **Data Analyst job market** using SQL by analyzing:
- 💰 Top-paying data analyst jobs
- 🔥 Most in-demand skills
- 📈 Skills associated with higher salaries
- 🎯 Optimal skills to learn for maximizing salary and demand

The analysis was performed using SQL queries on job posting datasets and focuses mainly on remote Data Analyst roles.

---

# Background

The goal of this project was to better understand:
- Which data analyst jobs pay the most
- Which skills employers demand most frequently
- Which skills provide the best salary opportunities
- Which skills are both highly paid and highly demanded

This project uses a dataset containing:
- Job postings
- Salaries
- Companies
- Skills required for each job

---

# Tools Used 🛠️

| Tool | Purpose |
|------|----------|
| SQL | Data querying and analysis |
| PostgreSQL | Database management |
| Visual Studio Code | Writing and executing SQL queries |
| Git & GitHub | Version control and project sharing |

---

# Project Structure 📁

```bash
SQL_Project_Data_Job_Analysis/
│
├── advanced_sql/
├── assets/
├── project_sql/
├── sql_load/
├── README.md
└── .gitignore
```

---

# The Analysis 🔍

## 1. Top Paying Data Analyst Jobs

This query identifies the highest-paying remote Data Analyst jobs.

### SQL Query

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
LEFT JOIN company_dim
    ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst'
    AND job_location = 'Anywhere'
    AND salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10;
```

### Key Insights
- Salaries ranged from **$184K to $650K**
- Companies like **Meta**, **AT&T**, and **SmartAsset** offered top salaries
- Job titles varied from Analyst roles to Director-level positions

---

## 2. Skills Required for Top Paying Jobs

This analysis identifies which skills appear most frequently in top-paying jobs.

### SQL Query

```sql
WITH top_paying_jobs AS (
    SELECT
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim
        ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst'
        AND job_location = 'Anywhere'
        AND salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)

SELECT
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim
    ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim
    ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
```

### Key Insights
- **SQL** appeared most frequently
- **Python** and **Tableau** were also highly demanded
- Skills like **Snowflake**, **Pandas**, and **Excel** appeared often

---

## 3. Most In-Demand Skills for Data Analysts

This query finds the most requested skills across Data Analyst job postings.

### SQL Query

```sql
SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim
    ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim
    ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND job_work_from_home = TRUE
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```

### Top Skills

| Skill | Demand Count |
|------|------|
| SQL | 7291 |
| Excel | 4611 |
| Python | 4330 |
| Tableau | 3745 |
| Power BI | 2609 |

### Key Insights
- SQL remains the most important skill
- Excel still plays a major role in analytics
- Visualization tools are highly valued

---

## 4. Highest Paying Skills

This analysis determines which skills are associated with the highest salaries.

### SQL Query

```sql
SELECT
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim
    ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim
    ON skills_job_dim.skill_id = skills_dim.skill_id
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

### Top Paying Skills

| Skill | Average Salary |
|------|------|
| pyspark | $208,172 |
| bitbucket | $189,155 |
| couchbase | $160,515 |
| datarobot | $155,486 |
| pandas | $151,821 |

### Key Insights
- Big Data and ML-related skills pay the most
- Cloud and engineering tools increase salary potential
- Specialized technical skills are highly rewarded

---

## 5. Most Optimal Skills to Learn

This query combines both salary and demand to identify the best skills to learn.

### SQL Query

```sql
SELECT
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim
    ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim
    ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = TRUE
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25;
```

### Best Skills to Learn

| Skill | Demand Count | Avg Salary |
|------|------|------|
| Go | 27 | $115,320 |
| Hadoop | 22 | $113,193 |
| Snowflake | 37 | $112,948 |
| Azure | 34 | $111,225 |
| AWS | 32 | $108,317 |

### Key Insights
- Cloud technologies are extremely valuable
- Big data tools have strong salary potential
- Programming skills remain highly relevant

---

# What I Learned 📚

Throughout this project I improved my skills in:
- Advanced SQL querying
- JOIN operations
- Common Table Expressions (CTEs)
- Aggregation functions
- Real-world data analysis
- Data-driven storytelling

---

# Conclusions ✅

- Remote Data Analyst jobs can pay extremely high salaries
- SQL is both the most demanded and one of the most valuable skills
- Cloud and big data technologies are rapidly growing in importance
- Combining high-demand and high-paying skills creates better career opportunities

---


# Acknowledgements 🙌

Inspired by Luke Barousse's SQL Data Analytics project and course content.

---