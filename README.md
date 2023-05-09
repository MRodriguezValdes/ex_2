# ex_2
## 1

```sql
CREATE OR REPLACE FUNCTION get_num_emp_dept(p_deptno INTEGER)
RETURNS INTEGER AS $$
DECLARE
  emp_count INTEGER;
BEGIN
  -- Abrimos el cursor que recupera el número de empleados en el departamento especificado
  FOR emp IN SELECT COUNT(*) AS emp_count FROM employees WHERE department_id = p_deptno
  LOOP
    -- Asignamos el valor de emp_count a la variable emp_count
    emp_count := emp.emp_count;
  END LOOP;
  -- Devolvemos el número de empleados en el departamento
  RETURN emp_count;
END;
$$ LANGUAGE plpgsql;

```


## 2

```sql

DO $$
DECLARE
  emp_name VARCHAR(50);
BEGIN
  -- Usamos un bucle FOR con una subconsulta que recupera los nombres de los empleados cuyo trabajo sea "ST_CLERK"
  FOR emp IN SELECT first_name || ' ' || last_name AS emp_name FROM employees WHERE job_id = 'ST_CLERK'
  LOOP
    -- Asignamos el nombre del empleado a la variable emp_name
    emp_name := emp.emp_name;
    -- Mostramos el nombre del empleado por consola
    RAISE NOTICE 'Empleado: %', emp_name;
  END LOOP;
END;
$$ LANGUAGE plpgsql;

```
## 3 

```sql
DECLARE
    emp_cursor CURSOR FOR
        SELECT employee_id, salary
        FROM employees
        FOR UPDATE;
    emp_record RECORD;
BEGIN
    OPEN emp_cursor;
    LOOP
        FETCH emp_cursor INTO emp_record;
        EXIT WHEN NOT FOUND;
        IF emp_record.salary > 8000 THEN
            UPDATE employees SET salary = salary * 1.02 WHERE CURRENT OF emp_cursor;
        ELSEIF emp_record.salary < 800 THEN
            UPDATE employees SET salary = salary * 1.03 WHERE CURRENT OF emp_cursor;
        END IF;
    END LOOP;
    CLOSE emp_cursor;
    -- Comprobar que los salarios se han actualizado correctamente
    SELECT * FROM employees;
END;


```
