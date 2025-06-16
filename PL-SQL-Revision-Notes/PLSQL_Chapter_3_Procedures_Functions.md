# 📘 PL/SQL Chapter 3: Procedures and Functions

---

## 🆓 Anonymous Blocks

An **anonymous block** is a block of PL/SQL code that:
- Has **no name**
- Is **not stored** in the database
- Is typically used for **temporary tasks, quick tests, or throwaway scripts**

### 🧾 Characteristics
- Cannot be reused or called from another block
- Cannot be stored in the database permanently
- Typically saved locally as `.sql` files if needed

### 📌 Example of an Anonymous Block
```plsql
DECLARE
  v_message VARCHAR2(50) := 'Hello from anonymous block';
BEGIN
  DBMS_OUTPUT.PUT_LINE(v_message);
END;
```

> ⚠️ Anonymous blocks are **not suitable** for large, reusable, or complex applications. For those, use **procedures or functions**.

---

## 🔧 What is a Procedure?
A **procedure** is a named block of PL/SQL statements that can be stored in the database and **executed later by name**.

Instead of rewriting the same PL/SQL logic repeatedly, you can define a procedure **once** and then call it wherever required.

This procedure can accept input parameters, process logic, and optionally return output.

---

## 🧾 Syntax: Creating a Procedure
```plsql
CREATE OR REPLACE PROCEDURE procedure_name (
    parameter1 [IN | OUT | IN OUT] datatype DEFAULT default_value,
    parameter2 [IN | OUT | IN OUT] datatype
)
AS
  -- Declaration Section
BEGIN
  -- Executable Section
EXCEPTION
  -- Exception Handling
END procedure_name;
```

- `IN`: Value passed **into** the procedure (read-only)
- `OUT`: Value passed **back** from the procedure (write-only)
- `IN OUT`: Value **passed in and modified**, then returned

> ❗ While defining data types for parameters, **do not specify lengths** (e.g., avoid `VARCHAR2(20)` or `NUMBER(10)`). Just use `VARCHAR2` or `NUMBER`.

---

## 📌 Parameter Modes Explained
| Mode     | Description                                 | Direction |
|----------|---------------------------------------------|-----------|
| `IN`     | Accepts input, value cannot be modified     | Caller ➡ Procedure |
| `OUT`    | Returns output, uninitialized at start      | Procedure ➡ Caller |
| `IN OUT` | Accepts and returns a modifiable value      | ↔ Bidirectional |

---

## ✅ Benefits of Using Procedures
- **Reusability**: Define once, reuse many times
- **Modularity**: Break down complex logic into smaller units
- **Maintainability**: Update logic in one place
- **Security**: Can hide implementation details using private procedures

---

## 🧪 Example: Procedure with IN OUT Mode
```plsql
CREATE OR REPLACE PROCEDURE ADD_CUSTOMER(
    c_id         IN OUT NUMBER,
    c_fname      IN VARCHAR2,
    c_lname      IN VARCHAR2,
    c_email      IN VARCHAR2,
    c_phone      IN VARCHAR2,
    c_address    IN VARCHAR2,
    c_city       IN VARCHAR2,
    c_state      IN VARCHAR2,
    c_zip        IN VARCHAR2,
    c_region     IN VARCHAR2
)
AS
BEGIN
  INSERT INTO customer (
    customer_id, first_name, last_name, email, phone,
    address, city, state, zip, region
  ) VALUES (
    c_id, c_fname, c_lname, c_email, c_phone,
    c_address, c_city, c_state, c_zip, c_region
  );

  SELECT COUNT(1) INTO c_id FROM customer;

  COMMIT;
  DBMS_OUTPUT.PUT_LINE('Data successfully inserted.');
END ADD_CUSTOMER;
```

### ▶️ Calling the IN OUT Procedure
```plsql
DECLARE
  tcount NUMBER := 45;
BEGIN
  ADD_CUSTOMER(
    c_id => tcount,
    c_fname => 'Aria',
    c_lname => 'White',
    c_email => 'aria.white@example.com',
    c_phone => '9993331111',
    c_address => '101 Maple St',
    c_city => 'Atlanta',
    c_state => 'GA',
    c_zip => '30301',
    c_region => 'Southeast'
  );
  DBMS_OUTPUT.PUT_LINE('Total records = ' || tcount);
END;
```

> 🔄 The same `c_id` variable is used to send a value **into** the procedure and then **receive a new value** (total customer count) back.

---

## 🧠 Summary of Procedure Calls
| Method             | Key Benefit                  |
|--------------------|------------------------------|
| Positional         | Simpler, but strict on order |
| Named Association  | Flexible ordering            |

> Both methods run the same compiled procedure and insert data correctly. Use named association for clarity in complex calls.

---

## 📌 Structure Recap
| Section     | Purpose                                |
|-------------|----------------------------------------|
| `DECLARE`   | Declare variables/constants (optional) |
| `BEGIN`     | Write logic and SQL statements         |
| `EXCEPTION` | Handle errors (optional)              |

> In the upcoming sections, we’ll implement procedures and also explore **functions**—another modular unit that returns a value.
