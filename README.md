# SQL-Project
Here we have few queries to get data for different scenarios. We have below four tables 


-  JOB_POSTINGS_FACT - It has columns like Job_id, company_id, job_title_short,job_title,job_location,job_via,job_posted_date,salary_year_avg,salary_hour_avg
- ** SKILLS_JOB_DIM- It has columns like job_id,skill_id
- ** SKILLS_DIM - It has skill_id,skills,type
- ** COMPANY_DIM - It has company_id, company_name


Query 1:

Fetches data for ' which location has top paying for data analyst' using below joins with 'Group By'

```
FROM
     JOB_POSTINGS_FACT
     
GROUP BY    
      JOB_LOCATION
HAVING JOB_TITLE_SHORT = 'Data Analyst'
AND SALARY_YEAR_AVG IS NOT NULL
AND JOB_LOCATION <> 'Anywhere'

ORDER BY AVG_SALARY DESC
LIMIT 10
```

And the result is

<img width="1800" height="579" alt="Screenshot 2026-01-05 230254" src="https://github.com/user-attachments/assets/00bc62ff-fe4f-4f5b-b647-df9f777811f4" />

Query 2:

Fetches data for 'which Company pays the most in top paying country for data analyst' using CTE. We get the company id detail in temp table TOP_PAYING_COUNTRY and then using COMPANY_DIM table we get the company name for top 10.

```
WITH TOP_PAYING_COUNTRY AS
(
SELECT     
     COMPANY_ID,
     JOB_LOCATION,
     AVG(SALARY_YEAR_AVG) AS AVG_SALARY
FROM
     JOB_POSTINGS_FACT 
GROUP BY
       COMPANY_ID,    
      JOB_LOCATION
HAVING JOB_TITLE_SHORT = 'Data Analyst'
AND SALARY_YEAR_AVG IS NOT NULL
AND JOB_LOCATION NOT LIKE '%Any%'
ORDER BY AVG_SALARY DESC
)

SELECT  DISTINCT CD.NAME AS COMPANY_NAME,
       TP.JOB_LOCATION AS COUNTRY,
       TP.AVG_SALARY AS SALARY
FROM  TOP_PAYING_COUNTRY TP
INNER JOIN COMPANY_DIM CD
ON TP.COMPANY_ID = CD.COMPANY_ID
ORDER BY SALARY DESC
LIMIT 10
```

And the result is

<img width="1805" height="540" alt="Screenshot 2026-01-05 231359" src="https://github.com/user-attachments/assets/8c00bc09-1daf-4c77-a985-3fc892699f64" />

