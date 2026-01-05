# SQL-Project
Here we have few queries to get data for different scenarios. We have below four tables 


1.JOB_POSTINGS_FACT - It has columns like Job_id, company_id, job_title_short,job_title,job_location,job_via,job_posted_date,salary_year_avg,salary_hour_avg
2.SKILLS_JOB_DIM- It has columns like job_id,skill_id
3.SKILLS_DIM - It has skill_id,skills,type
3.COMPANY_DIM - It has company_id, company_name


Query 1:
Fetches data for ' which country has top paying for data analyst' using below joins
'FROM
     JOB_POSTINGS_FACT 
GROUP BY    
      JOB_LOCATION
HAVING JOB_TITLE_SHORT = 'Data Analyst'
AND SALARY_YEAR_AVG IS NOT NULL
AND JOB_LOCATION <> 'Anywhere'
ORDER BY AVG_SALARY DESC
LIMIT 10'
