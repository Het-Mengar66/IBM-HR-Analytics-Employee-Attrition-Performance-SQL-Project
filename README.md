# IBM-HR-Analytics-Employee-Attrition-Performance-SQL-Project

![IBM Logo](https://upload.wikimedia.org/wikipedia/commons/5/51/IBM_logo.svg)

## Description

This project focuses on analyzing employee attrition and performance using SQL, based on the **IBM HR Analytics Employee Attrition & Performance** dataset. The goal is to uncover insights into why employees leave the organization, identify key factors influencing attrition, and evaluate employee performance metrics. The dataset includes various attributes such as employee demographics, job roles, satisfaction levels, work-life balance, and more.

By leveraging SQL queries, this project explores trends, patterns, and correlations within the data to help organizations make data-driven decisions to reduce attrition and improve employee retention.

## Objective

SELECT 
    *
FROM
    ibm;

-- This script analyzes employee attrition trends, identifying key factors that contribute to employee turnover.


-- 1) What is the average attrition rate?


SELECT 
    (SUM(Attrition = 'Yes') * 100.0 / COUNT(*)) AS Attrition_rate
FROM
    ibm;


-- 2) Which department has highest Attrition rate?


SELECT 
    Department,
    (SUM(Attrition = 'Yes') * 100.0 / COUNT(*)) AS Attrition_rate,
    COUNT(*) AS Total_employee,
    SUM(Attrition = 'Yes') AS Employee_left
FROM
    ibm
GROUP BY Department
ORDER BY Attrition_rate;


-- 3) Is there a correlation between Age and Attrition?


SELECT 
    Age,
    (SUM(Attrition = 'Yes') * 100.0 / COUNT(*)) AS Attrition_rate
FROM
    ibm
GROUP BY Age
ORDER BY Age;


-- 4) Maximum and Minimum Attrition rate from the Age group


WITH AgeAttrition AS (
    SELECT 
        Age,
        (SUM(Attrition = 'Yes') * 100 / COUNT(*)) AS Attrition_Rate
    FROM ibm
    GROUP BY Age
)
SELECT Age, Attrition_Rate
FROM AgeAttrition
WHERE Attrition_Rate = (SELECT MAX(Attrition_Rate) FROM AgeAttrition)
   OR Attrition_Rate = (SELECT MIN(Attrition_Rate) FROM AgeAttrition);


-- 5) How does the Job satisfaction relate to Attrition


SELECT 
    JobSatisfaction,
    (SUM(Attrition = 'Yes') * 100 / COUNT(*)) AS Attrition_Rate_Left,
    (SUM(Attrition = 'No') * 100 / COUNT(*)) AS Attrition_Rate_Stayed
FROM
    ibm
GROUP BY JobSatisfaction
ORDER BY JobSatisfaction DESC;


-- 6) Is Salary a significant factor in employee Attrition?


SELECT 
    MonthlyIncome,
    (SUM(Attrition = 'Yes') * 100 / COUNT(*)) AS Attrition_Rate_Left,
    (SUM(Attrition = 'No') * 100 / COUNT(*)) AS Attrition_Rate_Stayed
FROM
    ibm
GROUP BY MonthlyIncome
ORDER BY MonthlyIncome;


-- 7) How does Attrition vary by Job role and Job level?


SELECT 
    JobRole,
    (SUM(Attrition = 'Yes') * 100 / COUNT(*)) AS Attrition_Rate
FROM
    ibm
GROUP BY JobRole
ORDER BY JobRole DESC;


-- 8) How does the Distance from home can relate to Attrition?


SELECT 
    DistanceFromHome,
    (SUM(Attrition = 'Yes') * 100 / COUNT(*)) AS Attrition_rate_percentage
FROM
    ibm
GROUP BY DistanceFromHome
ORDER BY Attrition_rate_percentage DESC;


-- 9) What is the Impact of work life balance on Attrition?


SELECT 
    WorkLifeBalance,
    (SUM(Attrition = 'Yes') * 100 / COUNT(*)) AS Attrition_Rate
FROM
    ibm
GROUP BY WorkLifeBalance
ORDER BY WorkLifeBalance DESC;

-- This script analyzes employee Demographics trends, identifying key factors that contribute to employee turnover.


-- 10) What is the distribution of employee by age, gender &  material status


SELECT 
    Age, Gender, MaritalStatus, COUNT(*) AS Employee_Count
FROM
    ibm
GROUP BY Age , Gender , MaritalStatus
ORDER BY Age , Gender , MaritalStatus;


-- 11) Which Education field is most common employee?


SELECT 
    EducationField,
    COUNT(*) AS Employee_count,
    (COUNT(*) * 100.0 / (SELECT 
            COUNT(*)
        FROM
            ibm)) AS Percentage
FROM
    ibm
GROUP BY EducationField
ORDER BY Percentage DESC;


-- 12) How does gender distribution vary across job roles?


SELECT 
    JobRole, Gender, COUNT(*) AS Employee_count
FROM
    ibm
GROUP BY JobRole , Gender
ORDER BY JobRole , Gender;


-- This script analyzes employee job role and performance, identifying key factors that contribute to employee turnover.


-- 13) Which Job role has highest and lowest monthly income?

WITH salary_table AS (
SELECT 
    JobRole, 
    ROUND(AVG(MonthlyIncome), 2) AS Avg_Salary
FROM ibm
GROUP BY JobRole
)
SELECT JobRole, Avg_salary
FROM salary_table
WHERE Avg_salary = (SELECT MAX(AVG_salary) FROM salary_table)
OR	  Avg_salary = (SELECT MIN(AVG_salary) FROM salary_table);


-- 14) Relantionship between Job level and performance rating


SELECT 
    JobLevel,
    COUNT(*) AS Employee_Count,
    ROUND(AVG(PerformanceRating), 2) AS Avg_Performance_Rating
FROM
    ibm
GROUP BY JobLevel
ORDER BY JobLevel;


-- This script analyzes carrer growth and promotions, identifying key factors that contribute to employee turnover.

-- 15) What is the Average salary hikes in there job role

SELECT 
    JobRole,
    ROUND(AVG(PercentSalaryHike), 2) AS Avg_Per_salary_hikes
FROM
    ibm
GROUP BY JobRole
ORDER BY Avg_Per_salary_hikes;


-- 16) What is the average number of years employee stay in there current role?


SELECT 
    JobRole,
    ROUND(AVG(YearsInCurrentRole), 2) AS AVG_Years_In_CurrentRole
FROM
    ibm
GROUP BY JobRole
ORDER BY AVG_Years_In_CurrentRole DESC;


-- 17) What is the average salary hike percentage who left VS who stayed?


SELECT 
    Attrition,
    ROUND(AVG(PercentSalaryHike), 2) AS Avg_Salary_Hike
FROM
    ibm
GROUP BY Attrition;

