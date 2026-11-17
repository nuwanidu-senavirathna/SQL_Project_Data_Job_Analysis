# Introduction
📊 Dive into the data job market! Focusing on data analyst roles, this project explores 💰 top-paying jobs, 🔥 in-demand skills, and 📈 where high demand meets high salary in data analytics.

🔍 SQL queries? Check them out here: [project_sql folder](/project_sql/)

# Background
The questions answered through my analysis were:

1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

# Tools I used
The project involved several tools and technologies to perform database querying, data exploration, analysis, and version control.
- SQL: Used to extract, filter, join, and analyze job posting data to identify salary trends, skill requirements, and employment opportunities within the Data Analyst job market.
- PostgreSQL: Served as the relational database management system for storing and managing the job market dataset and executing analytical queries.
- Visual Studio Code (VS Code): Used as the primary development environment for writing, managing, and executing SQL queries through database extensions.
- Git: Utilized for tracking changes, managing project versions, and maintaining an organized history of SQL scripts and project updates.
- GitHub: Used to host the project repository, publish SQL scripts, document analytical findings, and make the project accessible for collaboration and future reference.

# The Analysis
### 1. Top paying Data Analytics Jobs

```sql
SELECT
    job_id,
    name AS company_name,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short= 'Data Analyst' AND
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10;
```
The analysis of the highest-paying remote Data Analyst positions revealed several interesting findings:
- Attractive Salary Opportunities: The top 10 positions offer annual salaries ranging from $184,000 to $650,000, demonstrating the substantial earning potential available in the data analytics field.
- Opportunities Across Different Industries: Organizations such as SmartAsset, Meta, and AT&T feature among the highest-paying employers, highlighting the demand for analytical talent across various business sectors.
- Diverse Career Opportunities: The highest-paying positions include different designations, ranging from Data Analyst to Director of Analytics, illustrating the variety of career paths available within the analytics domain.
### 2. Skills Required for Top Paying Jobs
Examining the technical requirements of the highest-paying positions helped identify the capabilities employers prioritize when recruiting highly compensated data professionals.

```sql
WITH top_paying_jobs AS (
    SELECT
        job_id,
        name AS company_name,
        job_title,
        salary_year_avg
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short= 'Data Analyst' AND
        job_location = 'Anywhere' AND
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id=skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id=skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
```
**Key Insights**
- **SQL as a Core Requirement**: SQL appeared in 8 of the top 10 job postings, reinforcing its importance in database querying, data extraction, and analysis.
- **Python for Advanced Analysis**: With 7 mentions, Python emerged as another highly valued skill, reflecting its importance in data manipulation, automation, and analytical tasks.
- **Strong Demand for Visualization**: Tableau appeared in 6 job postings, highlighting the importance of transforming analytical findings into meaningful visual representations.
- **Additional Technical Competencies**: Other technologies, including R, Snowflake, Pandas, and Excel, also appeared in the requirements, suggesting that employers value a combination of analytical, programming, and data management skills.

### 3. In-Demand Skills for Data Analysts
The analysis of job posting data highlights the five most frequently requested skills by employers seeking Data Analysts, providing a clear picture of the technical competencies valued in the job market. 
```sql
SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id=skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id=skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```

**Analyzed Insights**
- **SQL Dominates Skill Demand**: SQL ranks as the most in-demand skill, with 92,628 mentions. This emphasizes the importance of database querying and data extraction as fundamental requirements for Data Analyst positions.
- **Excel Remains Essential**: With 67,031 mentions, Excel holds the second position, demonstrating its continued relevance in data manipulation, reporting, and spreadsheet-based analysis.
- **Python Strengthens Analytical Capabilities**: Recording 57,326 mentions, Python ranks third, highlighting the growing importance of programming for data processing, automation, and advanced analytical tasks.
- **Strong Demand for Visualization Tools**: Tableau appears with 46,554 mentions, while Power BI records 39,468 mentions. Both tools demonstrate the importance of developing interactive dashboards and communicating insights through visualizations.
- **A Combination of Skills Is Valuable**: The results suggest that employers place considerable emphasis on a combination of database management, spreadsheet analysis, programming, and business intelligence capabilities rather than relying on a single technical skill.
### 4. Skills Based on Salary
The salary analysis reveals that certain specialized technical skills are associated with significantly higher average salaries in Data Analyst roles.
- **High Salary Potential for Specialized Tools**: SVN ranks highest with an average annual salary of $400,000, followed by Solidity at $179,000, highlighting the strong compensation associated with niche technical expertise.
- **Advanced Data and AI Technologies**: Skills such as Couchbase ($160,515), DataRobot ($155,486), MXNet ($149,000), PyTorch ($125,226), and TensorFlow ($120,647) demonstrate the salary potential of expertise in advanced data platforms and machine learning technologies.
- **Growing Value of Cloud and Automation Skills**: Terraform, VMware, Ansible, and Puppet feature among the higher-paying skills, indicating the value of infrastructure management and automation capabilities.
- **Data Engineering and Development Tools**: Technologies such as Kafka, Airflow, Cassandra, and GitLab also offer competitive average salaries, reflecting the importance of data infrastructure, workflow orchestration, and development practices.
```sql
SELECT
    skills,
    ROUND(AVG(salary_year_avg), 2) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id=skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id=skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' AND
    salary_year_avg IS NOT NULL
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;
```
### 5.Most Optimal Skills to Learn
By considering both job demand and average salary, the analysis identifies several skills that offer promising career opportunities for Data Analysts.
- **Strong Foundation in Core Analytical Tools**: SQL leads with 398 job postings, followed by Excel (256) and Python (236). These skills combine strong market demand with competitive average salaries, making them essential priorities for aspiring Data Analysts.
- **Programming and Visualization Skills**: Python ($101,397), R ($100,499), and Tableau ($99,288) demonstrate attractive salary potential alongside considerable demand, highlighting the value of programming and data visualization capabilities.
- **Cloud and Data Warehouse Technologies**: Snowflake ($112,948), Azure ($111,225), and AWS ($108,317) offer some of the strongest salary opportunities, indicating the growing value of cloud computing and modern data platforms.
- **Specialized Data Technologies**: Hadoop ($113,193) and Go ($115,320) record relatively high average salaries, suggesting additional opportunities for professionals who develop expertise beyond traditional analytics tools.
- **Business Intelligence and Database Skills**: Looker ($103,795), Oracle ($104,534), and SQL Server ($97,786) also demonstrate competitive compensation, reinforcing the importance of BI platforms and database technologies.
```sql
WITH skills_demand AS (
    SELECT
        skills_dim.skill_id,
        skills_dim.skills,
        COUNT(skills_job_dim.job_id) AS demand_count
    FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id=skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id=skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst'
        AND salary_year_avg IS NOT NULL
        AND job_work_from_home = True
    GROUP BY
        skills_dim.skill_id
), average_salary AS(
    SELECT
        skills_job_dim.skill_id,
        ROUND(AVG(salary_year_avg), 2) AS avg_salary
    FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id=skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id=skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst'
        AND salary_year_avg IS NOT NULL
        AND job_work_from_home = True
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
WHERE demand_count>10
ORDER BY
    demand_count DESC,
    avg_salary DESC
LIMIT
    25;
```

# What I Learned
Through this project, I strengthened my SQL and data analysis skills while gaining a better understanding of the Data Analyst job market. Some of the key learnings include:
- **Advanced SQL Techniques**: Improved my ability to write complex SQL queries using JOINs, Common Table Expressions (CTEs), aggregate functions, filtering, sorting, and subqueries to extract meaningful insights.
- **Database Management**: Gained practical experience working with PostgreSQL, managing relational datasets, and understanding relationships between multiple tables.
- **Data Exploration and Analysis**: Learned how to investigate job market trends by analyzing salary distributions, skill demand, and the relationship between technical skills and compensation.
- **Combining Multiple Data Sources**: Developed a better understanding of how to connect job posting data with company and skills datasets to perform more comprehensive analysis.
- **Market and Career Insights**: Identified the most востребованый technical skills for Data Analysts and explored how specialized technologies, cloud platforms, and programming skills relate to salary opportunities.
- **Version Control**: Gained experience using Git and GitHub to manage SQL scripts, track project changes, and document analytical findings.
# Conclusions
This project provided valuable insights into the Data Analyst job market by examining job opportunities, salary trends, and the technical skills employers seek.

The findings highlight that SQL, Excel, and Python are among the most frequently requested skills in Data Analyst job postings, while Tableau and Power BI remain important for data visualization and business intelligence. The salary analysis also revealed competitive compensation associated with specialized technologies such as cloud platforms, data engineering tools, and machine learning technologies.

Overall, the analysis demonstrates the importance of developing a balanced skill set that combines core analytical capabilities with specialized technical knowledge. These findings provide a useful foundation for identifying in-demand competencies, prioritizing future learning, and understanding the evolving requirements of the Data Analyst profession.