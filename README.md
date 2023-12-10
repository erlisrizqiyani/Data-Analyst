
# Data Science Job Role Analysis

The objective of this project is to analyze uncleaned data science job salary dataset from Kaggle using data cleaning and normalization techniques in Google BigQuery, and then create a dashboard on Looker Studio that visualizes the insights from the data.

## Project Overview

The data for this project was collected from the Kaggle website. The dataset contains information about data science job roles in different countries, including salary estimates from various companies. The data was collected from a variety of sources, including job postings, company websites, and salary surveys.

The data is stored in a CSV file format. The file contains the following columns:

* Country: The country in which the job is located.
* Job title: The title of the data science job role.
* Company: The company that is hiring for the job.
* Salary estimate: The estimated salary for the job.

The data contains a total of 600+ rows. The data is relatively unclean, there are a few errors and inconsistencies that need to be addressed.

## Data Cleaning and Normalization
The data will be cleaned and normalized using SQL on Google BigQuery. The following steps will be taken:

* Errors and inconsistencies: The data will be checked for errors and inconsistencies. Any errors or inconsistencies will be corrected.
* Data types: The data types of the columns will be checked and corrected as needed.
* Missing values: Missing values will be handled using imputation techniques.
* Normalization: The data will be normalized to a common scale.
## Usage/Examples

```javascript
SELECT Company_Name  FROM `rolejob.02_dataset_uncleaned.job` LIMIT 1000;
SELECT Rating  FROM `rolejob.02_dataset_uncleaned.job` LIMIT 1000;

UPDATE `rolejob.02_dataset_uncleaned.job`
SET Company_Name = REGEXP_REPLACE(Company_Name, '.[0-9]+', '')
WHERE Company_Name is NOT NULL;

UPDATE `rolejob.02_dataset_uncleaned.job`
SET Company_Name = REGEXP_REPLACE(Company_Name,'[0-9]$', '')
WHERE Company_Name is NOT NULL;

UPDATE `rolejob.02_dataset_uncleaned.job`
SET Rating = 0
WHERE Rating = -1;

-- ALTER TABLE `rolejob.02_dataset_uncleaned.job`
-- ADD COLUMN loc_by_city STRING(100),
-- ADD COLUMN loc_by_country STRING(100);

UPDATE `rolejob.02_dataset_uncleaned.job`
SET loc_by_city = TRIM(SPLIT(Location, ',')[SAFE_OFFSET(0)]),
    loc_by_country = TRIM(SPLIT(Location, ',')[SAFE_OFFSET(1)])
WHERE Location IS NOT NULL;

SELECT Location, loc_by_city, loc_by_country  FROM `rolejob.02_dataset_uncleaned.job` LIMIT 1000;

SELECT DISTINCT REVENUE FROM  `rolejob.02_dataset_uncleaned.job` LIMIT 1000;

UPDATE `rolejob.02_dataset_uncleaned.job`
SET Revenue = 'Unknown'
WHERE Revenue = '-1' OR Revenue = 'Unknown / Non-Applicable';

ALTER TABLE `rolejob.02_dataset_uncleaned.job`
ADD COLUMN Seniority String(100),
ADD COLUMN Job_Simp String(100);

SELECT DISTINCT JOB_TITLE FROM `rolejob.02_dataset_uncleaned.job` LIMIT 1000;

UPDATE `rolejob.02_dataset_uncleaned.job` 
SET Seniority = 
  CASE 
    WHEN REGEXP_CONTAINS(LOWER(Job_Title), '(\\b(sr\\.|sr|senior|head)\\b)') THEN 'Senior DS'
    WHEN REGEXP_CONTAINS(LOWER(Job_Title), 'data science|data scientist|data analyst|machine learning engineer') THEN 'DS'
    WHEN REGEXP_CONTAINS(LOWER(Job_Title), '(\\b(jr\\.|jr|junior)\\b)') THEN 'Junior DS'
    ELSE 'N/A'
  END
WHERE Job_Title IS NOT NULL;


SELECT DISTINCT JOB_TITLE, Seniority FROM `rolejob.02_dataset_uncleaned.job` LIMIT 1000;

UPDATE `rolejob.02_dataset_uncleaned.job` 
SET Job_Simp = 
  CASE 
    WHEN REGEXP_CONTAINS( LOWER(Job_Title), '(\\b(scientist|science)\\b)') THEN 'Data Scientist'
    WHEN REGEXP_CONTAINS( LOWER(Job_Title) ,  '(\\b(data engineer)\\b)') THEN 'Data Engineer'
    WHEN REGEXP_CONTAINS( LOWER(Job_Title), '(\\b(machine learning)\\b)') THEN 'Machine Learning Engineer'
    WHEN REGEXP_CONTAINS( LOWER(Job_Title) ,  '(\\b(analytics|analyst)\\b)') THEN 'Data Analyst'
    ELSE 'N/A' -- Atau nilai default lainnya
  END
WHERE Job_Title is not null;

SELECT DISTINCT JOB_TITLE, Job_Simp FROM `rolejob.02_dataset_uncleaned.job` LIMIT 1000;

UPDATE `rolejob.02_dataset_uncleaned.job` 
SET loc_by_country = 
  CASE
    WHEN REGEXP_CONTAINS(loc_by_country, r'^AL$') THEN 'Alabama'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^AZ$') THEN 'Arizona'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^CA$') THEN 'California'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^CO$') THEN 'Colorado'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^CT$') THEN 'Connecticut'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^DC$') THEN 'District of Columbia'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^DE$') THEN 'Delaware'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^FL$') THEN 'Florida'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^GA$') THEN 'Georgia'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^IA$') THEN 'Iowa'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^IL$') THEN 'Illinois'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^IN$') THEN 'Indiana'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^KS$') THEN 'Kansas'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^LA$') THEN 'Louisiana'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^MA$') THEN 'Massachusetts'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^MD$') THEN 'Maryland'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^MI$') THEN 'Michigan'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^MN$') THEN 'Minnesota'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^MO$') THEN 'Missouri'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^MS$') THEN 'Mississippi'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^NC$') THEN 'North Carolina'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^NE$') THEN 'Nebraska'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^NH$') THEN 'New Hampshire'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^NJ$') THEN 'New Jersey'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^NY$') THEN 'New York'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^OH$') THEN 'Ohio'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^OK$') THEN 'Oklahoma'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^OR$') THEN 'Oregon'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^PA$') THEN 'Pennsylvania'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^RI$') THEN 'Rhode Island'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^SC$') THEN 'South Carolina'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^TN$') THEN 'Tennessee'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^TX$') THEN 'Texas'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^UT$') THEN 'Utah'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^VA$') THEN 'Virginia'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^WA$') THEN 'Washington'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^WI$') THEN 'Wisconsin'
    WHEN REGEXP_CONTAINS(loc_by_country, r'^WV$') THEN 'West Virginia'
    ELSE loc_by_country  -- if the code doesn't match any of the above, keep it as is
  END
WHERE loc_by_country is not null;

SELECT DISTINCT loc_by_country FROM `rolejob.02_dataset_uncleaned.job` LIMIT 1000;

-- SALARY_ESTIMATE
--- Update 
----- Replace "(Glassdoor est.)"
UPDATE rolejob.02_dataset_uncleaned.job
SET salary_estimate = REGEXP_REPLACE(salary_estimate, '\\(Glassdoor est.\\)', '')
WHERE 1=1;
----- Replace "$" 
UPDATE rolejob.02_dataset_uncleaned.job
SET salary_estimate = REGEXP_REPLACE(salary_estimate, '[$]', '')
WHERE 1=1;
----- Replace 'K' 
UPDATE rolejob.02_dataset_uncleaned.job
SET salary_estimate = REGEXP_REPLACE(salary_estimate, 'K', '000')
WHERE 1=1;

--- Add New Column
---- Min Salary
ALTER TABLE rolejob.02_dataset_uncleaned.job
ADD COLUMN min_salary INT64;
UPDATE rolejob.02_dataset_uncleaned.job
SET min_salary = CAST(REGEXP_EXTRACT(salary_estimate, r'(\d+)-') AS INT64)
WHERE 1=1;
---- Max Salary
ALTER TABLE rolejob.02_dataset_uncleaned.job
ADD COLUMN max_salary INT64;
UPDATE rolejob.02_dataset_uncleaned.job
SET max_salary = CAST(REGEXP_EXTRACT(salary_estimate, r'-(\d+)') AS INT64)
WHERE 1=1;
-- Avg Salary
ALTER TABLE rolejob.02_dataset_uncleaned.job
ADD COLUMN avg_salary FLOAT64;
UPDATE rolejob.02_dataset_uncleaned.job
SET avg_salary = (min_salary + max_salary) / 2.0
WHERE 1=1;


-- Show All Table
SELECT * FROM rolejob.02_dataset_uncleaned.job;
```


## Documentation

[Documentation](Data_Science_Job_Report_page-0001.jpg)

