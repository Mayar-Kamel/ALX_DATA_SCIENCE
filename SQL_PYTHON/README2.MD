# Data Analysis Project: Auditor Report Integration and Error Analysis

## Overview

This project involves integrating an auditor report into an existing database, joining relevant data from multiple tables, identifying discrepancies, and analyzing employee performance based on error rates.

## Database Schema

The project uses the following tables:
- `auditor_report`
- `visits`
- `water_quality`
- `employee`

## Steps and Queries

### 1. Adding Auditor Report to the Database

1. **Create the `auditor_report` table**:
    ```sql
    CREATE TABLE auditor_report (
        location_id INT,
        true_water_source_score INT,
        statements TEXT
    );
    ```

2. **Insert data into `auditor_report`**:
    ```sql
    INSERT INTO auditor_report (location_id, true_water_source_score, statements)
    VALUES (1, 90, 'Statement about the location...');
    ```

### 2. Create View for Incorrect Records

1. **Create the `Incorrect_records` view**:
    ```sql
    CREATE VIEW Incorrect_records AS
    SELECT
        auditor_report.location_id,
        visits.record_id,
        employee.employee_name,
        auditor_report.true_water_source_score AS auditor_score,
        wq.subjective_quality_score AS surveyor_score,
        auditor_report.statements AS statements
    FROM
        auditor_report
    JOIN
        visits ON auditor_report.location_id = visits.location_id
    JOIN
        water_quality AS wq ON visits.record_id = wq.record_id
    JOIN
        employee ON employee.assigned_employee_id = visits.assigned_employee_id
    WHERE
        visits.visit_count = 1
        AND auditor_report.true_water_source_score != wq.subjective_quality_score;
    ```

### 3. Analyze Errors by Employees

1. **Calculate the number of mistakes per employee**:
    ```sql
    WITH error_count AS (
        SELECT
            employee_name,
            COUNT(employee_name) AS number_of_mistakes
        FROM
            Incorrect_records
        GROUP BY
            employee_name
    )
    SELECT * FROM error_count;
    ```

2. **Calculate the average number of mistakes**:
    ```sql
    SELECT AVG(number_of_mistakes) AS avg_mistakes FROM error_count;
    ```

3. **Identify employees with above-average mistakes**:
    ```sql
    WITH suspect_list AS (
        SELECT
            employee_name,
            number_of_mistakes
        FROM
            error_count
        WHERE
            number_of_mistakes > (SELECT AVG(number_of_mistakes) FROM error_count)
    )
    SELECT * FROM suspect_list;
    ```

### 4. Detailed Analysis of Suspect Employees

1. **Retrieve detailed records for suspect employees**:
    ```sql
    SELECT
        *
    FROM
        Incorrect_records
    WHERE
        employee_name IN (SELECT employee_name FROM suspect_list);
    ```

2. **Filter records containing the keyword "cash"**:
    ```sql
    SELECT
        *
    FROM
        Incorrect_records
    WHERE
        employee_name IN (SELECT employee_name FROM suspect_list)
        AND statements LIKE '%cash%';
    ```

3. **Check for employees mentioning "cash" not in the suspect list**:
    ```sql
    SELECT
        *
    FROM
        Incorrect_records
    WHERE
        statements LIKE '%cash%'
        AND employee_name NOT IN (SELECT employee_name FROM suspect_list);
    ```

### Python Integration

1. **Establish a connection to the MySQL database and execute the queries**:
    ```python
    import mysql.connector
    import pandas as pd

    # Establish a connection to the MySQL database
    cnx = mysql.connector.connect(
        user='root',
        password='Mayar123@',
        host='127.0.0.1',
        database='md_water_services',
        auth_plugin='mysql_native_password'
    )

    # Define the query to create the Incorrect_records view and get the data
    query = """
    DROP TABLE IF EXISTS Incorrect_records;

    CREATE VIEW Incorrect_records AS
    SELECT
        auditor_report.location_id,
        visits.record_id,
        employee.employee_name,
        auditor_report.true_water_source_score AS auditor_score,
        wq.subjective_quality_score AS surveyor_score,
        auditor_report.statements AS statements
    FROM
        auditor_report
    JOIN
        visits ON auditor_report.location_id = visits.location_id
    JOIN
        water_quality AS wq ON visits.record_id = wq.record_id
    JOIN
        employee ON employee.assigned_employee_id = visits.assigned_employee_id
    WHERE
        visits.visit_count = 1
        AND auditor_report.true_water_source_score != wq.subjective_quality_score;
    """

    # Execute the query and load the data into a Pandas DataFrame
    df = pd.read_sql(query, con=cnx)

    # Close the connection
    cnx.close()
    ```

## Conclusion

This project demonstrates how to integrate an auditor report into a database, identify discrepancies, and analyze employee performance using SQL and Python. The use of CTEs and views helps in organizing complex queries and improving the readability of the code.
