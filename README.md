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

<strong>13. Identify employees whose job titles start with the word "Senior".</strong>  

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

<strong>14. Total number of employees hired in each year.</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql

SELECT 
    year(hire_date) as hire_year, 
    COUNT(emp_no) as total_emps
FROM employees 
GROUP BY hire_year
ORDER BY total_emps DESC;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| hire_year | total_emps |
|-----------|------------|
| 1985      | 4396       |
| 1986      | 4368       |
| 1987      | 4115       |
| 1988      | 3984       |
| 1989      | 3448       |
| 1990      | 3225       |
| 1991      | 2815       |
| 1992      | 2568       |
| 1993      | 2265       |
| 1994      | 1943       |
| 1995      | 1515       |
| 1996      | 1199       |
| 1997      | 764        |
| 1998      | 498        |
| 1999      | 187        |
| 2000      | 1          |

</details>

<br>

<strong>15. Average salary for each job title in the company.</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    t.title, 
    AVG(s.salary) as avg_salary 
FROM titles t
JOIN salaries s
ON s.emp_no = t.emp_no 
GROUP BY title;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| title              | avg_salary   |
|--------------------|--------------|
| Assistant Engineer | 77304.1154   |
| Staff              | 66016.4182   |
| Senior Staff       | 64738.9767   |
| Senior Engineer    | 61319.0794   |
| Engineer           | 59970.7276   |
| Technique Leader   | 58343.2143   |

</details>

<br>

<strong>16. Current Highest Paid Employee</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    e.first_name,
    e.last_name,
    COUNT(t.title) AS title_changes
FROM employees e
JOIN titles t ON e.emp_no = t.emp_no
GROUP BY 
    e.emp_no, 
    e.first_name, 
    e.last_name
ORDER BY title_changes DESC
LIMIT 1;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| first_name | last_name | title_changes  |
|------------|-----------|----------------|
| Sumant     | Peac      | 3              |

</details>

<br>

<strong>17. Employee who has undergone the most number of salary changes in the company.</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql

SELECT 
    e.first_name, 
    e.last_name, 
    COUNT(s.salary) AS salary_changes
FROM employees e
JOIN salaries s ON e.emp_no = s.emp_no
GROUP BY 
    e.emp_no, 
    e.first_name, 
    e.last_name
ORDER BY salary_changes DESC
LIMIT 1;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| first_name | last_name | salary_changes |
|------------|-----------|----------------|
| Sumant     | Peac      | 18             |

</details>

<br>

<strong>18. Calculate the total salary expenditure for each department within a specific period (from January 1, 1990, to December 31, 2000).</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
	d.dept_no,
    d.dept_name,
    SUM(s.salary) AS total_salary
FROM dept_emp de
JOIN departments d ON de.dept_no = d.dept_no
JOIN salaries s ON de.emp_no = s.emp_no
WHERE s.from_date >= '1990-01-01' 
AND  s.to_date <= '2000-12-31'
GROUP BY d.dept_name;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| dept_no | dept_name           | total_salary |
|---------|---------------------|--------------|
| d005    | Development         | 7566943      |
| d004    | Production          | 6113439      |
| d007    | Sales               | 2667998      |
| d003    | Human Resources     | 2572902      |
| d006    | Quality Management  | 1968008      |
| d002    | Finance             | 1578833      |
| d009    | Customer Service    | 1234808      |
| d001    | Marketing           | 918284       |
| d008    | Research            | 1719342      |

</details>

<br>

<strong>19. Employees who have changed their title within the same department.</strong>  

<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    e.emp_no,
    CONCAT(e.first_name, ' ', e.last_name) AS full_name,
    d.dept_no,
    d.dept_name,
    count(DISTINCT t.title) as title_count
FROM employees e
JOIN titles t ON e.emp_no = t.emp_no
JOIN dept_emp de ON de.emp_no = e.emp_no
JOIN departments d ON d.dept_no = de.dept_no
GROUP BY 
    e.emp_no, 
    e.first_name,
    e.last_name, 
    d.dept_name
HAVING title_count > 1
ORDER BY title_count DESC
LIMIT 5;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| emp_no | full_name          | dept_no | dept_name           | title_count |
|--------|--------------------|---------|---------------------|-------------|
| 10009  | Sumant Peac        | d006    | Quality Management  | 3           |
| 10066  | Kwee Schusler      | d005    | Development         | 3           |
| 10005  | Kyoichi Maliniak   | d003    | Human Resources     | 2           |
| 10007  | Tzvetan Zielinski  | d008    | Research            | 2           |
| 10012  | Patricio Bridgland | d005    | Development         | 2           |

</details>

<br>

<strong>20. Salary Subquery</strong>  
Employees who earn the highest salary in their respective departments along with their salaries.
<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    e.emp_no,
    CONCAT(e.first_name, ' ', e.last_name) AS full_name,
    d.dept_no,
    d.dept_name,
    s.salary
FROM employees e
JOIN salaries s ON e.emp_no = s.emp_no
JOIN dept_emp de ON e.emp_no = de.emp_no
JOIN departments d ON d.dept_no = de.dept_no
WHERE s.salary = (
        SELECT MAX(s2.salary)
        FROM salaries s2
        JOIN dept_emp de2 ON s2.emp_no = de2.emp_no
        WHERE de2.dept_no = de.dept_no
	)
ORDER BY s.salary DESC;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| emp_no | full_name           | dept_no | dept_name          | salary |
|--------|---------------------|---------|--------------------|--------|
| 10017  | Cristinel Bouloucos | d001    | Marketing          | 99651  |
| 10050  | Yinghua Dredge      | d002    | Finance            | 97830  |
| 10050  | Yinghua Dredge      | d007    | Sales              | 97830  |
| 10024  | Suzette Pettey      | d004    | Production         | 96646  |
| 10005  | Kyoichi Maliniak    | d003    | Human Resources    | 94692  |
| 10006  | Sumant Peac         | d006    | Quality Management | 94443  |
| 10001  | Georgi Facello      | d005    | Development        | 88958  |
| 10007  | Tzvetan Zielinski   | d008    | Research           | 88070  |
| 10038  | Huan Lortz          | d009    | Customer Service   | 64254  |

</details>

<br>

<strong>21. Department Salary Subquery</strong>  
Departments where the average salary is greater than the overall average salary across all departments.
<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    d.dept_no,
    d.dept_name,
    AVG(s.salary) AS dept_avg_salary,
    comp.company_avg_salary
FROM departments d
JOIN dept_emp de ON d.dept_no = de.dept_no
JOIN salaries s ON de.emp_no = s.emp_no
JOIN (
        SELECT AVG(s2.salary) AS company_avg_salary
        FROM salaries s2
     ) AS comp
GROUP BY 
    d.dept_no, 
    d.dept_name
HAVING dept_avg_salary > comp.company_avg_salary
ORDER BY dept_avg_salary DESC;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| dept_no | dept_name           | dept_avg_salary  | company_avg_salary  |
|---------|---------------------|------------------|---------------------|
| d002    | Finance             | 88724.0000       | 61743.6612          |
| d001    | Marketing           | 84357.7333       | 61743.6612          |
| d007    | Sales               | 73773.3878       | 61743.6612          |
| d006    | Quality Management  | 72665.5476       | 61743.6612          |

</details>

<br>

<strong>22. Average Salary Subquery</strong>  
Department with the highest average salary.
<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    d.dept_no, 
    d.dept_name
FROM departments d
JOIN dept_emp de ON d.dept_no = de.dept_no
JOIN salaries s ON de.emp_no = s.emp_no
GROUP BY 
    d.dept_no, 
    d.dept_name
ORDER BY AVG(s.salary) DESC
LIMIT 1;
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| dept_no | dept_name    |
|---------|--------------|
| d002    | Finance      |

</details>

<br>

<strong>23. Longest Time Subquery</strong>  
Employee(s) who have been with the company the longest.
<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    emp_no,
    CONCAT(first_name, '  ', last_name) AS full_name,
    hire_date,
    TIMESTAMPDIFF(YEAR, hire_date, CURDATE()) AS years_worked
FROM employees
WHERE hire_date = (
    SELECT MIN(hire_date)
    FROM employees
);
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| emp_no |   full_name   | hire_date  | years_worked |
| ------ | ------------- | ---------- | ------------ | 
| 20539  | Poorav Gecsei | 1985-02-01 | 40           |

</details>

<br>

<strong>24. Salary Threshold</strong>  
The department that has the highest number of employees earning more than a specified salary threshold (e.g., salary > 50,000).
<details>
  <summary>Click to expand answer!</summary>

```sql
SELECT 
    d.dept_no,
    d.dept_name
FROM departments d
WHERE d.dept_no = (
    SELECT de.dept_no
    FROM dept_emp de
    JOIN salaries s ON de.emp_no = s.emp_no
    WHERE s.salary > 50000
    GROUP BY de.dept_no
    ORDER BY COUNT(*) DESC
    LIMIT 1
);
```
</details>

<details>
  <summary>Click to expand results!</summary>

#### 📌 Output:

| dept_no | dept_name    |
|---------|--------------|
| d005    | Development  |

</details>

<br>

<strong>25. Company Average</strong>  
The departments that have had managers earning salaries higher than the company's overall average salary.
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
