# SQL-Project
Here we have few queries to get data for different scenarios. We have below four tables 


-  JOB_POSTINGS_FACT - It has columns like Job_id, company_id, job_title_short,job_title,job_location,job_via,job_posted_date,salary_year_avg,salary_hour_avg
-  SKILLS_JOB_DIM- It has columns like job_id,skill_id
-  SKILLS_DIM - It has skill_id,skills,type
- COMPANY_DIM - It has company_id, company_name

## Query 1:

Fetches data for ***' which location has top paying for data analyst'*** using below joins with 'Group By'

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

## Query 2:

Fetches data for ***'which Company pays the most in top paying country for data analyst'*** using CTE. We get the company id detail in temp table TOP_PAYING_COUNTRY and then using COMPANY_DIM table we get the company name for top 10.

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

## Query 3:

Fetches data for ***'Which skills are related to 'Data Analys'*** using subquery. We get the skill detail for 'Data Analyst' in subquery and then using SKILL_DIM table we get the Skills name .

```
SELECT 
       SD.SKILLS   
     
FROM
     (SELECT DISTINCT
     JP.JOB_TITLE_SHORT,
     JP.JOB_ID,
     SJ.SKILL_ID
FROM JOB_POSTINGS_FACT JP
INNER JOIN SKILLS_JOB_DIM SJ
ON JP.JOB_ID = SJ.JOB_ID
AND JP.JOB_TITLE_SHORT = 'Data Analyst' ) AS SK
INNER JOIN SKILLS_DIM SD
ON SD.SKILL_ID = SK.SKILL_ID
GROUP BY SD.SKILLS
ORDER BY SKILLS 
```

And the result is
<img width="269" height="477" alt="Screenshot 2026-01-05 234223" src="https://github.com/user-attachments/assets/02722242-894b-40d5-b980-d851044ada39" />


## Query 4:

Fetches data for ***'which top skills related to salary'*** using GROUP BY. We get the skill detail for 'Data Analyst' along with their salary details.

```
SELECT 
      SD.SKILLS,
      ROUND(AVG(JP.SALARY_YEAR_AVG),0) AS AVG_SALARY
FROM JOB_POSTINGS_FACT JP
INNER JOIN SKILLS_JOB_DIM SJ
ON JP.JOB_ID = SJ.JOB_ID
INNER JOIN SKILLS_DIM SD
ON SD.SKILL_ID = SJ.SKILL_ID
WHERE 
     JP.JOB_TITLE_SHORT = 'Data Analyst'
   AND JP.SALARY_YEAR_AVG IS NOT NULL
   AND JP.JOB_WORK_FROM_HOME = True
GROUP BY 
  SD.SKILLS
ORDER BY AVG_SALARY DESC
```

And the result is

<img width="1474" height="473" alt="Screenshot 2026-01-05 234940" src="https://github.com/user-attachments/assets/650ebc71-2d66-4fa9-a939-287338fd3179" />


## Query 5:

Fetches data for ***'Which company has high demand and more salary'*** using GROUP BY. We get the company,demand count and salary details for 'Data Analyst'.

```
SELECT
  CD.COMPANY_ID,
  CD.NAME,
  COUNT(JP.JOB_ID) AS DEMAND_COUNT,
  ROUND(AVG(JP.SALARY_YEAR_AVG),0) AS HIGH_SALARY
FROM JOB_POSTINGS_FACT JP
INNER JOIN COMPANY_DIM CD
ON JP.COMPANY_ID = CD.COMPANY_ID
AND JP.JOB_TITLE_SHORT = 'Data Analyst'
AND JP.SALARY_YEAR_AVG IS NOT NULL
GROUP BY CD.COMPANY_ID
HAVING COUNT(JP.JOB_ID) > 10
ORDER BY
HIGH_SALARY DESC,
DEMAND_COUNT DESC

```

And the result is

<img width="1720" height="423" alt="Screenshot 2026-01-05 235356" src="https://github.com/user-attachments/assets/16354b2c-d390-4c42-a6ea-1e0a6eeaf440" />
