# 📦 PL/SQL Chapter 5: Packages

---

## 📁 What Are Packages?

A **PL/SQL package** is a schema object that groups logically related **types, variables, procedures, functions, cursors**, and **exceptions** together.

This is similar to organizing files in folders:

- All movies go into a `Movies` folder
- All documents go into a `Documents` folder

Likewise, in PL/SQL:

- Group related subprograms and declarations into **packages**

---

## 🎯 Advantages of Using Packages

| Advantage                    | Description                                                                |
| ---------------------------- | -------------------------------------------------------------------------- |
| ✅ Modularity                 | Group related logic under a single name                                    |
| 🧠 Easier Application Design | Specification can be shared before implementation starts                   |
| 🔒 Information Hiding        | Hide implementation details; expose only what's needed                     |
| 🚀 Better Performance        | Package loads once into memory; all components access faster               |
| 🔁 Shared State              | Variables/cursors can be shared across procedures/functions in the package |

---

## 🧱 Structure of a Package

A package consists of:

- **Specification**: Public interface — declares procedures, functions, and variables
- **Body**: Actual implementation of those declarations

---

## ✍️ Writing a Package Specification

The **specification** includes only **declarations**, not the actual code logic.

### 🔧 Syntax

```plsql
CREATE OR REPLACE PACKAGE package_name AS
  -- Procedure and Function headers
  -- Variable and Cursor declarations
END package_name;
```

---

## 👨‍💻 Example: Customer Package Specification

We have six subprograms: four procedures and two functions — all related to customer logic. Let’s group them.

### 🧾 Code

```plsql
CREATE OR REPLACE PACKAGE CUSTOMER_PACKAGE AS

  PROCEDURE ADD_CUSTOMER(
    c_id NUMBER,
    c_fname VARCHAR2,
    c_lname VARCHAR2,
    c_region VARCHAR2,
    c_country VARCHAR2,
    c_zip NUMBER,
    c_contact NUMBER,
    c_email VARCHAR2,
    c_address VARCHAR2,
    c_created DATE
  );

  PROCEDURE DISPLAY_NAMES;

  PROCEDURE GET_CUSTOMER(
    c_id NUMBER
  );

  PROCEDURE SHOW_CUSTOMER(
    c_region VARCHAR2
  );

  FUNCTION FIND_SALESCOUNT(
    p_sales_date DATE
  ) RETURN NUMBER;

  FUNCTION GET_NAMES RETURN VARCHAR2;

END CUSTOMER_PACKAGE;
```

📌 **Note**: This is only the **specification**. It defines what each program does, not how. Implementation is handled in the **package body**.

---

## ✅ Output

Once compiled, the package `CUSTOMER_PACKAGE` appears under your `Packages` list in SQL Developer. Clicking on it displays all declared procedures and functions.

---

## 🧩 Writing the Package Body

The **body** implements the logic for all procedures and functions declared in the package specification.

### 🛠 Syntax

```plsql
CREATE OR REPLACE PACKAGE BODY package_name AS
  -- Full definitions of procedures/functions
END package_name;
```

---

## 🧾 Example: Customer Package Body

```plsql
CREATE OR REPLACE PACKAGE BODY CUSTOMER_PACKAGE AS

  PROCEDURE ADD_CUSTOMER(
    c_id NUMBER,
    c_fname VARCHAR2,
    c_lname VARCHAR2,
    c_region VARCHAR2,
    c_country VARCHAR2,
    c_zip NUMBER,
    c_contact NUMBER,
    c_email VARCHAR2,
    c_address VARCHAR2,
    c_created DATE
  ) IS
  BEGIN
    -- Logic to add a new customer
    DBMS_OUTPUT.PUT_LINE('Customer added: ' || c_fname || ' ' || c_lname);
  END;

  PROCEDURE DISPLAY_NAMES IS
  BEGIN
    -- Logic to display customer names
    DBMS_OUTPUT.PUT_LINE('Displaying customer names...');
  END;

  PROCEDURE GET_CUSTOMER(
    c_id NUMBER
  ) IS
  BEGIN
    -- Logic to retrieve a customer by ID
    DBMS_OUTPUT.PUT_LINE('Getting customer with ID: ' || c_id);
  END;

  PROCEDURE SHOW_CUSTOMER(
    c_region VARCHAR2
  ) IS
  BEGIN
    -- Logic to show customers in a region
    DBMS_OUTPUT.PUT_LINE('Showing customers in region: ' || c_region);
  END;

  FUNCTION FIND_SALESCOUNT(
    p_sales_date DATE
  ) RETURN NUMBER IS
    sales_count NUMBER := 0;
  BEGIN
    -- Logic to find number of sales on a given date
    sales_count := 2; -- Example count
    RETURN sales_count;
  END;

  FUNCTION GET_NAMES RETURN VARCHAR2 IS
  BEGIN
    -- Logic to return a concatenated name
    RETURN 'John Doe';
  END;

END CUSTOMER_PACKAGE;
```

---

## ▶️ Calling Subprograms from a Package

You can call **procedures and functions** in a package using the following syntax:

### ⚙️ Syntax

```plsql
EXECUTE package_name.procedure_name(arguments);
```

### 📌 Example

```plsql
-- Call a procedure without parameters
EXECUTE CUSTOMER_PACKAGE.DISPLAY_NAMES;

-- Call a procedure with parameters
EXECUTE CUSTOMER_PACKAGE.GET_CUSTOMER(16);
```

The above will output:

```
Displaying customer names...
Getting customer with ID: 16
```

This is how you invoke stored procedures and functions from a package.

