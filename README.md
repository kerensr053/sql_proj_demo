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

<strong>5. Average salary, maximum salary and their difference for each department for the current employees</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT
    d.dept_name,
    AVG(s.salary) AS avg_salary,
    MAX(s.salary) AS max_salary,
    (MAX(s.salary) - AVG(s.salary)) AS difference
FROM salaries s
JOIN dept_emp de ON s.emp_no = de.emp_no
JOIN departments d ON d.dept_no = de.dept_no
WHERE s.to_date = '9999-01-01'
GROUP BY d.dept_name
ORDER BY avg_salary DESC;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| department         | avg_salary  | max_salary | difference |
|--------------------|-------------|------------|------------|
| Marketing          | 99651.0000  | 99651      | 0.0000     |
| Finance            | 97830.0000  | 97830      | 0.0000     |
| Quality Management | 84170.0000  | 94409      | 10239.0000 |
| Sales              | 81695.0000  | 97830      | 16135.0000 |
| Production         | 69399.0000  | 96646      | 27247.0000 |
| Human Resources    | 63894.0000  | 94692      | 25798.0000 |
| Research           | 68800.0000  | 88070      | 20041.0000 |
| Development        | 64027.5000  | 88070      | 24042.5000 |
| Customer Service   | 57790.0000  | 64254      | 6464.0000  |

</details>

<br>

<strong>6. Average salary, maximum salary and their difference for each job title for the current employees</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    t.title,
    AVG(s.salary) AS avg_salary,
    MAX(s.salary) AS max_salary,
    (MAX(s.salary) - AVG(s.salary)) AS difference
FROM titles t
JOIN salaries s ON t.emp_no = s.emp_no
WHERE s.to_date = '9999-01-01'
GROUP BY t.title
ORDER BY avg_salary DESC;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| title              | avg_salary   | max_salary | difference   |
|--------------------|--------------|------------|--------------|
| Assistant Engineer | 95527.5000   | 96646      | 1118.5000    |
| Senior Staff       | 73939.8571   | 99651      | 25711.1429   |
| Staff              | 73230.0000   | 99651      | 26421.0000   |
| Senior Engineer    | 70154.0476   | 94409      | 24254.9524   |
| Engineer           | 67008.0000   | 94409      | 27401.0000   |
| Technique Leader   | 58345.0000   | 58345      | 0.0000       |

</details>

<br>

<strong>7. Average salary, maximum salary and their difference for each gender for the current employees</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    gender,
    AVG(s.salary) AS avg_salary,
    MAX(s.salary) AS max_salary,
    (MAX(s.salary) - AVG(s.salary)) AS difference
FROM employees e
JOIN salaries s ON e.emp_no = s.emp_no
WHERE s.to_date = '9999-01-01'   -- current salaries only
GROUP BY gender; 
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| gender | avg_salary   | max_salary | difference   |
|--------|--------------|------------|--------------|
| F      | 73730.8889   | 99651      | 25920.1111   |
| M      | 66560.8214   | 97830      | 31269.1786   |

</details>

<br>

<strong>8. Current Number of Employees Under Each Manager</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    dm.emp_no AS manager_emp_no,
    CONCAT(m.first_name, ' ', m.last_name) AS manager_name,
    d.dept_name,
    COUNT(de.emp_no) AS num_emps
FROM dept_manager dm
JOIN employees m ON dm.emp_no = m.emp_no
JOIN departments d ON dm.dept_no = d.dept_no
JOIN dept_emp de ON dm.dept_no = de.dept_no
WHERE dm.to_date = '9999-01-01'
  AND de.to_date = '9999-01-01'
GROUP BY dm.emp_no, manager_name, d.dept_name
ORDER BY num_emps DESC;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| manager_emp_no | manager_name             | dept_name         | num_emps |
|----------------|--------------------------|-------------------|----------|
| 10567          | Diethrich Journel        | Development       | 22       |
| 10420          | Kaijung Riesenhuber      | Production        | 20       |
| 11133          | Nalini Azulay            | Sales             | 11       |
| 10228          | Karoline Cesareni        | Human Resources   | 9        |
| 11534          | Surveyors Besancenot     | Research          | 7        |
| 10854          | Jagoda Nannarelli        | Quality Management| 4        |
| 11939          | Chinhyun Basart          | Customer Service  | 4        |
| 10039          | Alejandro Brender        | Marketing         | 2        |
| 10114          | Munir Demeyer            | Finance           | 1        |

</details>

<br>

<strong>9. Five Most Recent Hires</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT *
FROM employees 
ORDER BY hire_date DESC
LIMIT 5; 
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| emp_no | birth_date | first_name | last_name | gender | hire_date  |
|--------|-------------|------------|-----------|--------|------------|
| 47291  | 1960-09-09  | Ulf        | Flexer    | M      | 2000-01-12 |
| 13246  | 1952-06-09  | Adil       | Siepmann  | F      | 1999-12-31 |
| 13919  | 1952-04-22  | Brewster   | Sinicrope | M      | 1999-12-04 |
| 28500  | 1954-04-28  | Fumiko     | Zwicker   | M      | 1999-11-23 |
| 17789  | 1956-03-16  | Snehasis   | Gilg      | F      | 1999-11-14 |

</details>

<br>

<strong>10. Top 3 Highest Salaries With Department</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    e.emp_no,
    CONCAT(e.first_name, ' ', e.last_name) AS full_name,
    s.salary,
    d.dept_name
FROM salaries s
JOIN employees e ON s.emp_no = e.emp_no
JOIN dept_emp de ON e.emp_no = de.emp_no
JOIN departments d ON de.dept_no = d.dept_no
WHERE s.to_date = '9999-01-01'
  AND de.to_date = '9999-01-01'
ORDER BY s.salary DESC
LIMIT 3;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| emp_no | full_name            | salary | dept_name        |
|--------|----------------------|--------|------------------|
| 10017  | Cristinel Bouloucos  | 99651  | Marketing        |
| 10050  | Yinghua Dredge       | 97830  | Sales            |
| 10022  | Suzette Pettey       | 96646  | Production       |

</details>

<br>

<strong>11. Engineers working for more than 5 years and earning less than 40k</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT DISTINCT
    e.emp_no, 
    CONCAT(e.first_name, ' ', e.last_name) AS full_name, 
    d.dept_name, 
    t.title,
    s.salary
FROM employees e 
JOIN salaries s ON s.emp_no = e.emp_no 
JOIN dept_emp de ON e.emp_no = de.emp_no
JOIN departments d ON d.dept_no = de.dept_no
JOIN titles t ON e.emp_no = t.emp_no
WHERE t.title LIKE '%Engineer%'
  AND e.hire_date <= DATE_SUB(CURDATE(), INTERVAL 5 YEAR)
  AND s.salary < 40000;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| emp_no | full_name        | dept_name   | title          | salary |
|--------|------------------|-------------|----------------|--------|
| 10022  | Shahaf Famili    | Development | Engineer       | 39335  |
| 10027  | Divier Reistad   | Development | Engineer       | 39520  |
| 10027  | Divier Reistad   | Development | Senior Engineer| 39520  |
| 10037  | Pradeep Makrucki | Development | Engineer       | 39765  |
| 10037  | Pradeep Makrucki | Development | Senior Engineer| 39765  |
| 10048  | Florian Syrotiuk | Development | Engineer       | 39507  |

</details>

<br>

<strong>12. Current Highest Paid Employee</strong>  

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

<strong>13. Current Highest Paid Employee</strong>  

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

<strong>14. Current Highest Paid Employee</strong>  

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

<strong>15. Current Highest Paid Employee</strong>  

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

<strong>16. Current Highest Paid Employee</strong>  

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

<strong>17. Current Highest Paid Employee</strong>  

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

<strong>18. Current Highest Paid Employee</strong>  

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

<strong>19. Current Highest Paid Employee</strong>  

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

<strong>20. Current Highest Paid Employee</strong>  

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

<strong>21. Current Highest Paid Employee</strong>  

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

<strong>22. Current Highest Paid Employee</strong>  

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

<strong>23. Current Highest Paid Employee</strong>  

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

<strong>24. Current Highest Paid Employee</strong>  

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

<strong>25. Managers earning salaries higher than the company's overall average salary</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    d.dept_no,
    d.dept_name,
    dm.emp_no AS manager_emp_no,
    CONCAT(e.first_name, ' ', e.last_name) AS manager_name,
    s.salary AS manager_salary,
    comp.avg_salary AS company_avg_salary
FROM departments d
JOIN dept_manager dm ON dm.dept_no = d.dept_no
JOIN employees e ON e.emp_no = dm.emp_no
JOIN salaries s ON s.emp_no = dm.emp_no
JOIN (
        SELECT AVG(salary) AS avg_salary
        FROM salaries
     ) comp
WHERE dm.to_date = '9999-01-01' -- current managers
  AND s.to_date = '9999-01-01' -- current salary 
  AND s.salary > comp.avg_salary
ORDER BY d.dept_no DESC;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| dept_no | dept_name | manager_emp_no | manager_name      | manager_salary | company_avg_salary |
| ------- | --------- | -------------- | ----------------- | -------------- | ------------------ |
| d001    | Marketing | 10039          | Alejandro Brender | 63918          | 61743.6612         |

</details>

<br>
