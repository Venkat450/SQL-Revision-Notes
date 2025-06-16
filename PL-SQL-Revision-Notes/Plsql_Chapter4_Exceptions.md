# 📘 PL/SQL Chapter 4: Exceptions

---

## 🚨 What are Exceptions?

Exceptions are **error conditions** that occur during program execution. In PL/SQL:
- When an error occurs, execution **stops immediately** unless handled.
- To handle these gracefully, PL/SQL offers the `EXCEPTION` block.

There are two types of exceptions:
- **System-defined exceptions**: Predefined by Oracle (e.g., `NO_DATA_FOUND`, `TOO_MANY_ROWS`).
- **User-defined exceptions**: Custom exceptions declared and handled by the programmer.

---

## 🛠️ Syntax: Exception Handling Block
```plsql
BEGIN
  -- Executable statements
EXCEPTION
  WHEN exception_name THEN
    -- Handling statements
  WHEN others THEN
    -- Fallback error handling
END;
```

> ✅ `WHEN OTHERS` catches any exceptions not explicitly named.

---

## 🔁 Exception Handling Flow
| Step     | Description                            |
|----------|----------------------------------------|
| DECLARE  | Optional: Declare custom exceptions    |
| BEGIN    | Logic that might raise exceptions      |
| EXCEPTION| Catch and handle raised exceptions     |

---

## 👤 User-Defined Exceptions
To define and raise your own exceptions, follow 3 steps:

### 📌 Step 1: Declare the Exception
```plsql
ex_customer_ID EXCEPTION;
```

### 📌 Step 2: Raise the Exception
```plsql
IF c_id <= 0 THEN
  RAISE ex_customer_ID;
END IF;
```

### 📌 Step 3: Handle the Exception
```plsql
EXCEPTION
  WHEN ex_customer_ID THEN
    DBMS_OUTPUT.PUT_LINE('Customer ID must be greater than zero');
```

---

## 🔄 Full Example: Raising User-Defined Exception
```plsql
CREATE OR REPLACE PROCEDURE validate_customer_id (
  c_id IN NUMBER
)
AS
  ex_customer_ID EXCEPTION;
BEGIN
  IF c_id <= 0 THEN
    RAISE ex_customer_ID;
  END IF;

  DBMS_OUTPUT.PUT_LINE('Valid customer ID: ' || c_id);

EXCEPTION
  WHEN ex_customer_ID THEN
    DBMS_OUTPUT.PUT_LINE('Customer ID must be greater than zero');
END;
```

### ▶️ Calling the Procedure
```plsql
BEGIN
  validate_customer_id(0);
END;
```

> 🧠 This prints: `Customer ID must be greater than zero`

---

## 📚 Summary
| Concept              | Description                                  |
|----------------------|----------------------------------------------|
| System Exceptions    | Predefined by Oracle                         |
| User Exceptions      | Declared and raised explicitly               |
| RAISE                | Keyword to raise an exception                |
| WHEN others THEN     | Catch-all fallback for unhandled exceptions  |

Next: Let’s see how exceptions apply in nested or complex PL/SQL blocks.
