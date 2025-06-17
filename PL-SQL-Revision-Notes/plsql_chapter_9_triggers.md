# PL/SQL Chapter 9: Triggers

## What are Triggers?

Triggers are stored programs in Oracle that are automatically executed in response to certain events on a particular table or view.

They are useful when:

- You want something to happen automatically when `INSERT`, `UPDATE`, or `DELETE` occurs
- Enforcing referential integrity
- Automatically updating derived columns
- Performing event logging or audit trails
- Restricting invalid transactions

Triggers can be compared to a gun’s trigger — pulling it causes an action, just like an event causes the execution of the PL/SQL trigger.

---

## Types of Triggers

### Based on Execution Scope:

1. **Row-Level Triggers**

   - Executed **once per row** affected.
   - Syntax contains `FOR EACH ROW`
   - Example: If an `UPDATE` affects 3 rows, trigger fires 3 times.

2. **Statement-Level Triggers**

   - Executed **once per SQL statement**, regardless of the number of rows affected.
   - Default if `FOR EACH ROW` is not specified.

### Based on Timing:

1. **BEFORE Trigger**

   - Fires **before** the triggering DML statement executes.
   - Useful for validations and derivations.

2. **AFTER Trigger**

   - Fires **after** the DML statement has executed.
   - Useful for logging and cascading operations.

---

## Valid Trigger Types

By combining timing (BEFORE/AFTER), operation (INSERT/UPDATE/DELETE), and scope (ROW/STATEMENT), we get 12 types:

- BEFORE INSERT STATEMENT
- BEFORE INSERT ROW
- AFTER INSERT STATEMENT
- AFTER INSERT ROW
- BEFORE UPDATE STATEMENT
- BEFORE UPDATE ROW
- AFTER UPDATE STATEMENT
- AFTER UPDATE ROW
- BEFORE DELETE STATEMENT
- BEFORE DELETE ROW
- AFTER DELETE STATEMENT
- AFTER DELETE ROW

---

## Example: Statement-Level Trigger (Audit Logging)

```sql
CREATE OR REPLACE TRIGGER customer_before_update
BEFORE UPDATE ON customer
BEGIN
    DECLARE
        v_username VARCHAR2(30);
    BEGIN
        SELECT USER INTO v_username FROM DUAL;
        INSERT INTO audit_table (table_name, user_id, operation_date, operation)
        VALUES ('CUSTOMER', v_username, SYSDATE, 'BEFORE UPDATE');
    END;
END;
```

---

## Example: Multi-Event Trigger

```sql
CREATE OR REPLACE TRIGGER customer_after_action
AFTER INSERT OR UPDATE OR DELETE ON customer
BEGIN
    DECLARE
        v_username VARCHAR2(30);
    BEGIN
        SELECT USER INTO v_username FROM DUAL;

        IF INSERTING THEN
            INSERT INTO audit_table VALUES ('CUSTOMER', v_username, SYSDATE, 'INSERT');
        ELSIF DELETING THEN
            INSERT INTO audit_table VALUES ('CUSTOMER', v_username, SYSDATE, 'DELETE');
        ELSIF UPDATING THEN
            INSERT INTO audit_table VALUES ('CUSTOMER', v_username, SYSDATE, 'UPDATE');
        END IF;
    END;
END;
```

---

## Row-Level Trigger with Old and New Pseudo Records

PL/SQL engine creates special records for each row trigger:

- `:OLD` – refers to the original value
- `:NEW` – refers to the new value

### Usage by Trigger Type

| Trigger Type | :OLD | :NEW |
| ------------ | ---- | ---- |
| INSERT       | NULL | Yes  |
| UPDATE       | Yes  | Yes  |
| DELETE       | Yes  | NULL |

### Example: Tracking Before and After Values

```sql
CREATE OR REPLACE TRIGGER customer_after_update_values
AFTER UPDATE ON customer
FOR EACH ROW
BEGIN
    DECLARE
        v_username VARCHAR2(30);
    BEGIN
        SELECT USER INTO v_username FROM DUAL;
        INSERT INTO audit_log (user_id, operation_date, b_customerid, a_customerid, b_first_name, a_first_name)
        VALUES (v_username, SYSDATE, :OLD.customerid, :NEW.customerid, :OLD.first_name, :NEW.first_name);
    END;
END;
```

---

## Row-Level Trigger with `WHEN` Clause

Use `WHEN` to restrict the trigger based on a condition in the `OLD` record.

### Example:

```sql
CREATE OR REPLACE TRIGGER customer_after_update_values
AFTER UPDATE ON customer
FOR EACH ROW
WHEN (OLD.region = 'SOUTH')
BEGIN
    DECLARE
        v_username VARCHAR2(30);
    BEGIN
        SELECT USER INTO v_username FROM DUAL;
        INSERT INTO audit_log (user_id, operation_date, b_customerid, a_customerid, b_first_name, a_first_name)
        VALUES (v_username, SYSDATE, :OLD.customerid, :NEW.customerid, :OLD.first_name, :NEW.first_name);
    END;
END;
```

---

## Row-Level Trigger with `OF` Clause

Use `OF` to restrict the trigger to changes in specific columns.

### Example:

```sql
CREATE OR REPLACE TRIGGER customer_after_update_of_id
AFTER UPDATE OF customerid ON customer
FOR EACH ROW
BEGIN
    DECLARE
        v_username VARCHAR2(30);
    BEGIN
        SELECT USER INTO v_username FROM DUAL;
        INSERT INTO audit_log (user_id, operation_date, b_customerid, a_customerid)
        VALUES (v_username, SYSDATE, :OLD.customerid, :NEW.customerid);
    END;
END;
```

---

## Summary

- **Row-Level** triggers fire per row; **Statement-Level** fire once per DML operation.
- Use **:OLD** and **:NEW** for capturing before/after values.
- Use `WHEN` to restrict row-level triggers based on old data.
- Use `OF` to restrict to specific column updates.

These trigger types are crucial in audit logging, enforcing rules, or controlling behavior during DML operations.

