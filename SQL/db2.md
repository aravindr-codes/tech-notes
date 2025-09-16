# 🗄️ Db2 SQL Quick Reference

## 📌 DDL (Data Definition Language)
- `CREATE TABLE employees (id INT PRIMARY KEY, name VARCHAR(50), salary DECIMAL(10,2));`
- `ALTER TABLE employees ADD COLUMN department VARCHAR(50);`
- `DROP TABLE employees;`
- `CREATE INDEX idx_emp_name ON employees(name);`
- `DROP INDEX idx_emp_name;`

---

## 📌 DML (Data Manipulation Language)
- `INSERT INTO employees (id, name, salary) VALUES (1, 'Alice', 50000);`
- `SELECT * FROM employees;`
- `UPDATE employees SET salary = 60000 WHERE id = 1;`
- `DELETE FROM employees WHERE id = 1;`

---

## 📊 Aggregate Functions & GROUP BY
- `SELECT COUNT(*) FROM employees;`
- `SELECT AVG(salary) FROM employees;`
- `SELECT MIN(salary), MAX(salary) FROM employees;`
- `SELECT SUM(salary) FROM employees;`
- `SELECT department, AVG(salary) FROM employees GROUP BY department;`
- `SELECT department, COUNT(*) FROM employees GROUP BY department HAVING COUNT(*) > 5;`

---

## 🔗 Joins
```sql
-- LEFT JOIN: All employees, even if no department match
SELECT e.name, e.salary, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.department = d.id;

-- RIGHT JOIN: All departments, even if no employees
SELECT e.name, e.salary, d.dept_name
FROM employees e
RIGHT JOIN departments d ON e.department = d.id;

-- FULL OUTER JOIN: Everything, even if no match
SELECT e.name, e.salary, d.dept_name
FROM employees e
FULL JOIN departments d ON e.department = d.id;
