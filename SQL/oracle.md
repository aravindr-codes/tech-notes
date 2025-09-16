
---

# 📘 Oracle SQL Quick Reference

```markdown
# 🗄️ Oracle SQL Quick Reference

## 📌 DDL (Data Definition Language)
- `CREATE TABLE employees (id NUMBER PRIMARY KEY, name VARCHAR2(50), salary NUMBER(10,2));`
- `ALTER TABLE employees ADD (department VARCHAR2(50));`
- `DROP TABLE employees CASCADE CONSTRAINTS;`
- `CREATE INDEX idx_emp_name ON employees(name);`
- `DROP INDEX idx_emp_name;`

---

## 📌 DML (Data Manipulation Language)
- `INSERT INTO employees (id, name, salary) VALUES (1, 'Alice', 50000);`
- `SELECT * FROM employees;`
- `UPDATE employees SET salary = 60000 WHERE id = 1;`
- `DELETE FROM employees WHERE id = 1;`
- `MERGE INTO employees e USING new_employees n ON (e.id = n.id) 
   WHEN MATCHED THEN UPDATE SET e.salary = n.salary 
   WHEN NOT MATCHED THEN INSERT (id, name, salary) VALUES (n.id, n.name, n.salary);`

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
FULL OUTER JOIN departments d ON e.department = d.id;
