# 👥📈 Employee Data Analysis & HR Insights <br> (SQL Project) 

This project uses SQL to analyze an employee database and extract meaningful HR insights, including workforce demographics, salary patterns and department performance. The goal is to run basic, intermediate and advanced analytical queries, and generate metrics that support real-world HR decision-making. It demonstrates how SQL can be applied to understand employee trends, improve operations, and drive data-based HR strategies. <br><br>

<div align="center">
    
![image](https://github.com/kerensr053/sql_proj_demo/blob/6c19bac76e1a7adbddb8fee637e56b7b97bc923d/EmployeeDB-ER.jpg)

</div>     

## SQL Queries 

<strong>1. Total Number of Employees</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT COUNT(*) AS total_num_emps
FROM employees;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| total_num_emps |
|----------------|
| 37291          |

</details>

<br>

<strong>2. Number of employees per department</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    d.dept_name,
    COUNT(de.emp_no) AS total_num_emps
FROM departments d
LEFT JOIN dept_emp de ON d.dept_no = de.dept_no
GROUP BY d.dept_name
ORDER BY total_num_emps DESC;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| department         | total_num_emps |
|--------------------|-----------------|
| Development        | 32              |
| Production         | 23              |
| Sales              | 15              |
| Research           | 11              |
| Human Resources    | 9               |
| Quality Management | 6               |
| Customer Service   | 6               |
| Finance            | 4               |
| Marketing          | 3               |

</details>

<br>

<strong>3. Employees and Their Departments</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
	e.emp_no, 
    CONCAT(first_name, '  ', last_name) AS full_name, 
    d.dept_name
FROM employees e
JOIN dept_emp de ON e.emp_no = de.emp_no
JOIN departments d ON de.dept_no = d.dept_no
LIMIT 10;
```
</details>

<details>
  <summary>Click to expand output</summary>

#### 📌 Output:

| emp_no | full_name               | dept_name         |
|--------|-------------------------|-------------------|
| 10001  | Georgi  Facello         | Development       |
| 10002  | Bezalel  Simmel         | Sales             |
| 10003  | Parto  Bamford          | Production        |
| 10004  | Chirstian  Koblick      | Development       |
| 10005  | Kyoichi  Maliniak       | Research          |
| 10006  | Anneke  Preusig         | Development       |
| 10007  | Tzvetan  Zielinski      | Sales             |
| 10008  | Saniya  Kalloufi        | Production        |
| 10009  | Sumant  Peac            | Development       |
| 10010  | Duangkaew  Piveteau     | Production        |

*Only first 10 rows shown for readability.*

</details>

<br>

<strong>4. Current Highest Paid Employee</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    e.emp_no,
    CONCAT(e.first_name, ' ', e.last_name) AS full_name,
    s.salary
FROM employees e
JOIN salaries s ON e.emp_no = s.emp_no
WHERE s.to_date = '9999-01-01'
ORDER BY s.salary DESC
LIMIT 1;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| emp_no | full_name             | salary |
|--------|-----------------------|--------|
| 10017  | Cristinel Bouloucos   | 99651  |

</details>

<br>

<strong>5. Average Salary by Department</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
	d.dept_name, 
    AVG(s.salary) AS avg_salary
FROM salaries s
JOIN dept_emp de ON s.emp_no = de.emp_no
JOIN departments d ON de.dept_no = d.dept_no
GROUP BY d.dept_name
ORDER BY avg_salary DESC;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| department         | avg_salary     |
|--------------------|----------------|
| Finance            | 88724.0000     |
| Marketing          | 84357.7333     |
| Sales              | 73773.3878     |
| Quality Management | 72665.5476     |
| Production         | 61706.2685     |
| Research           | 59911.5652     |
| Human Resources    | 59192.5000     |
| Development        | 58138.6324     |
| Customer Service   | 49972.0645     |

</details>

<br>



