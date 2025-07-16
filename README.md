# 📊 Data Analyst Job Market Analysis

## 📌 Introduction
Dive into the data job market!  
Focusing on **data analyst roles**, this project explores:

- 💰 Top-paying jobs  
- 🔥 In-demand skills  
- 📈 Where high demand meets high salary in data analytics

🔍 **SQL Queries?** → See [`project_sql`](./project_sql) folder

---

## 🧠 Background
This project was born from a desire to navigate the **data analyst job market** effectively and pinpoint:

- Top-paid roles  
- High-demand skills  
- Most optimal paths for job seekers

📊 **Dataset** from my SQL Course  
Includes: Job titles, salaries, locations, and essential skills.

---

## ❓ Questions Answered
1. 💼 What are the top-paying data analyst jobs?  
2. 🛠️ What skills are required for these top-paying jobs?  
3. 📊 What skills are most in demand for data analysts?  
4. 💵 Which skills are associated with higher salaries?  
5. 🎯 What are the most optimal skills to learn?

---

## 🛠️ Tools Used
- 🗄️ **SQL** – Querying and analysis
- 🛢️ **PostgreSQL** – Database system
- 💻 **VS Code** – SQL execution and data exploration
- 🌐 **Git & GitHub** – Version control, collaboration

---

## 🔎 The Analysis

### 1️⃣ Top-Paying Data Analyst Jobs
```sql
SELECT	
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE job_title_short = 'Data Analyst' 
  AND job_location = 'Anywhere' 
  AND salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10;
```


## 📌 Insights: Top-Paying Data Analyst Roles

- 💰 **Salary Range**: $184K to $650K  
- 🏢 **Employers**: Meta, AT&T, SmartAsset  
- 📋 **Roles**: From *Data Analyst* to *Director of Analytics*  
- 📊 **Visualization**: Bar chart of top 10 highest-paying roles

---

## 2️⃣ Skills for Top-Paying Jobs

```sql
WITH top_paying_jobs AS (
  SELECT	
    job_id,
    job_title,
    salary_year_avg,
    name AS company_name
  FROM job_postings_fact
  LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
  WHERE job_title_short = 'Data Analyst' 
    AND job_location = 'Anywhere' 
    AND salary_year_avg IS NOT NULL
  ORDER BY salary_year_avg DESC
  LIMIT 10
)
SELECT 
  top_paying_jobs.*,
  skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY salary_year_avg DESC;
```


```
SELECT 
  skills,
  COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst' 
  AND job_work_from_home = True 
GROUP BY skills
ORDER BY demand_count DESC
LIMIT 5;
```


| Skill    | Demand Count |
| -------- | ------------ |
| SQL      | 7291         |
| Excel    | 4611         |
| Python   | 4330         |
| Tableau  | 3745         |
| Power BI | 2609         |
```SELECT 
  skills,
  ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst'
  AND salary_year_avg IS NOT NULL
  AND job_work_from_home = True 
GROUP BY skills
ORDER BY avg_salary DESC
LIMIT 25;
```

| Skill         | Avg Salary (\$) |
| ------------- | --------------- |
| pyspark       | 208,172         |
| bitbucket     | 189,155         |
| couchbase     | 160,515         |
| watson        | 160,515         |
| datarobot     | 155,486         |
| gitlab        | 154,500         |
| swift         | 153,750         |
| jupyter       | 152,777         |
| pandas        | 151,821         |
| elasticsearch | 145,000         |

```
SELECT 
  skills_dim.skill_id,
  skills_dim.skills,
  COUNT(skills_job_dim.job_id) AS demand_count,
  ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst'
  AND salary_year_avg IS NOT NULL
  AND job_work_from_home = True 
GROUP BY skills_dim.skill_id
HAVING COUNT(skills_job_dim.job_id) > 10
ORDER BY avg_salary DESC, demand_count DESC
LIMIT 25;
```



| Skill      | Demand | Avg Salary (\$) |
| ---------- | ------ | --------------- |
| go         | 27     | 115,320         |
| confluence | 11     | 114,210         |
| hadoop     | 22     | 113,193         |
| snowflake  | 37     | 112,948         |
| azure      | 34     | 111,225         |
| bigquery   | 13     | 109,654         |
| aws        | 32     | 108,317         |
| java       | 17     | 106,906         |
| ssis       | 12     | 106,683         |
| jira       | 20     | 104,918         |




## 📘 What I Learned

- 🧩 **Complex SQL**: Mastered advanced joins, `WITH` clauses for cleaner queries  
- 📊 **Aggregation Mastery**: Used `COUNT`, `AVG`, `GROUP BY` for insightful summaries  
- 💡 **Real-World Thinking**: Transformed business questions into actionable SQL queries  

---

## 📌 Conclusions

| 🔑 Key Insight        | 📌 Summary                                             |
|----------------------|--------------------------------------------------------|
| 💼 Top Paying Jobs    | Remote data analyst roles can pay up to **$650K**     |
| 🛠️ Top Skills         | High-paying jobs require advanced **SQL**             |
| 📊 In-Demand Skills   | **SQL** leads in job postings                         |
| 💵 Salary Skills      | Tools like **PySpark**, **DataRobot** = 💰💰💰         |
| 🎯 Optimal Skills     | Learn **SQL + Cloud + Big Data** for top career value |

---

## 🎯 Closing Thoughts

This project sharpened my **SQL** and **market analysis** skills, and offered a valuable roadmap for aspiring data analysts.

Focus on **high-demand** and **high-salary** skills like **SQL, Python, Snowflake, AWS** to boost your career prospects in today’s job market.
