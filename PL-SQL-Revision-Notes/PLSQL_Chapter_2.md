# 📘 PL/SQL Chapter 2: Fetching and Inserting Data

---

## 🎯 Fetching Data from the Database

Use `SELECT INTO` to fetch data into PL/SQL variables.

### 🧾 Syntax

```plsql
SELECT column1, column2
INTO   variable1, variable2
FROM   table_name
WHERE  condition;
```

---

### ✅ Example

```plsql
DECLARE
  c_id      NUMBER := 10;
  c_name    VARCHAR2(50);
  c_address VARCHAR2(50);
BEGIN
  SELECT first_name, country
  INTO   c_name, c_address
  FROM   customer
  WHERE  customer_id = c_id;

  DBMS_OUTPUT.PUT_LINE('Name = ' || c_name);
  DBMS_OUTPUT.PUT_LINE('Country = ' || c_address);
END;
```

---

## 📐 Using `%TYPE` for Variable Declarations

### 🧾 Syntax

```plsql
variable_name table_name.column_name%TYPE;
```

### ✅ Example

```plsql
DECLARE
  c_id      customer.customer_id%TYPE := 10;
  c_name    customer.first_name%TYPE;
  c_address customer.country%TYPE;
BEGIN
  SELECT first_name, country
  INTO   c_name, c_address
  FROM   customer
  WHERE  customer_id = c_id;

  DBMS_OUTPUT.PUT_LINE('Name = ' || c_name);
  DBMS_OUTPUT.PUT_LINE('Country = ' || c_address);
END;
```

### 🧠 Why Use `%TYPE`?

| Benefit              | Description                                      |
|----------------------|--------------------------------------------------|
| Auto adapts to schema| Syncs with DB column automatically               |
| Low maintenance      | No need to update variable type manually         |
| Reduces errors       | Prevents type mismatch                           |

---

## ➕ Inserting Data into a Table using PL/SQL

### 🧾 Syntax

```plsql
DECLARE
  -- Declare variables with %TYPE
  c_id      customer.customer_id%TYPE := 13;
  c_fname   customer.first_name%TYPE := 'Jeff';
  c_lname   customer.last_name%TYPE := 'Smith';
  c_email   customer.email%TYPE := 'jeff@example.com';
  c_phone   customer.phone%TYPE := '1234567890';
  c_address customer.address%TYPE := '456 King St';
  c_city    customer.city%TYPE := 'Austin';
  c_state   customer.state%TYPE := 'TX';
  c_zip     customer.zip%TYPE := '73301';
  c_region  customer.region%TYPE := 'South';
BEGIN
  INSERT INTO customer (
    customer_id, first_name, last_name, email, phone,
    address, city, state, zip, region
  )
  VALUES (
    c_id, c_fname, c_lname, c_email, c_phone,
    c_address, c_city, c_state, c_zip, c_region
  );

  COMMIT;
  DBMS_OUTPUT.PUT_LINE('Data successfully inserted.');
END;
```

---

### 🧪 Example Output

| customer_id | first_name | region |
|-------------|------------|--------|
| 10          | John       | East   |
| 11          | Tom        | West   |
| 13          | Jeff       | South  |

### ✅ Tips

- Use `%TYPE` for all variables to future-proof your code
- Always `COMMIT` after insert
- Display a message using `DBMS_OUTPUT.PUT_LINE`

---

> 🔚 This chapter focused on retrieving and inserting data using SELECT INTO, %TYPE, and INSERT in PL/SQL.
