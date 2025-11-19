# 👥📈 Employee Data Analysis & HR Insights <br> (SQL Project) 

This project uses SQL to analyze an employee database and extract meaningful HR insights, including workforce demographics, salary patterns and department performance. The goal is to run basic, intermediate and advanced analytical queries, and generate metrics that support real-world HR decision-making. It demonstrates how SQL can be applied to understand employee trends, improve operations, and drive data-based HR strategies. 

## SQL Queries 

<strong>1. Total number of employees<strong>

<details>
  <summary>Click to expand SQL query!</summary>

  ```sql
SELECT COUNT(*) AS total_num_emps 
FROM employees; 
  ```
</details>
<br />  
<details>
  <summary>Click to expand expected results!</summary>   
region   |country_count|
---------|-------------|
Africa   |           59|
Americas |           57|
Asia     |           50|
Europe   |           48|
Oceania  |           26|
Antartica|            1|

</details>

