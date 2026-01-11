#  SQL Filter Application & Security Investigation

> **Project Context:** Google Cybersecurity Professional Certificate

<div align="center">

  ![Google](https://img.shields.io/badge/Google-Cybersecurity-4285F4?style=for-the-badge&logo=google&logoColor=white)
  ![SQL](https://img.shields.io/badge/SQL-MariaDB-4479A1?style=for-the-badge&logo=mariadb&logoColor=white)
  ![Data](https://img.shields.io/badge/Data-Security-FFD700?style=for-the-badge&logo=databricks&logoColor=black)
  ![Database](https://img.shields.io/badge/Database-Analysis-00758F?style=for-the-badge&logo=mysql&logoColor=white)

</div>

---

## Project Overview

**Scenario:** As a security professional, I was responsible for investigating potential security issues related to login attempts and managing employee machine updates. The task involved querying the organization's database to identify suspicious activity and isolate specific datasets for administrative actions.

This repository documents the **strategic use of SQL filters** (AND, OR, NOT) and **pattern matching** (LIKE) to audit logs and ensure system integrity within a MariaDB environment.

---

##  Technical Implementation: Security Queries

The investigation focused on two primary tables: `log_in_attempts` and `employees`.

### 1. Investigating Login Incidents
To address potential breaches, I filtered login logs to isolate high-risk events.

* **After-Hours Failed Logins:** Identified unsuccessful attempts occurring after 18:00 to investigate potential unauthorized access during off-hours.
    ```sql
    SELECT * FROM log_in_attempts WHERE login_time > '18:00' AND success = FALSE;
    ```
* **Suspicious Date Range:** Audited all login activity from a specific incident date (2022-05-09) and the preceding day to track attacker movement.
    ```sql
    SELECT * FROM log_in_attempts WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';
    ```
* **Geographic Filtering:** Isolated login attempts originating from outside of Mexico to investigate potential spoofing or external threats.
    ```sql
    SELECT * FROM log_in_attempts WHERE NOT country LIKE 'MEX%';
    ```

### 2. Asset & Compliance Management
SQL was used to target specific employee groups for mandatory security updates.

* **Regional Department Updates:** Retrieved employee data for the Marketing department within the East building for targeted patching.
    ```sql
    SELECT * FROM employees WHERE department = 'Marketing' AND office LIKE 'East%';
    ```
* **Cross-Departmental Targeting:** Identified all machines in the Finance or Sales departments for specialized security updates.
    ```sql
    SELECT * FROM employees WHERE department = 'Finance' OR department = 'Sales';
    ```
* **Exclusionary Audits:** Filtered for all employees outside the IT department who required a manual security update.
    ```sql
    SELECT * FROM employees WHERE NOT department = 'Information Technology';
    ```

---

## SQL Operators & Methods Applied



| Operator/Method | Security Application |
| :--- | :--- |
| **AND** | Combined conditions to pinpoint specific high-risk events (e.g., failed AND after-hours). |
| **OR** | Expanded queries to capture multiple target groups in a single audit (e.g., Sales OR Finance). |
| **NOT** | Excluded specific criteria to focus on remaining vulnerabilities (e.g., NOT IT department). |
| **LIKE & %** | Performed pattern matching to handle data variations like country codes and building locations. |

---

## Key Takeaways

*  **Precise Incident Scoping:** SQL filters allow security analysts to quickly narrow down thousands of logs to a handful of suspicious entries, speeding up response times.
*  **Pattern Recognition:** Mastering the `%` wildcard is essential for handling inconsistent data entries, such as varying country (MEX vs MEXICO) or office labels.
*  **Efficiency in Response:** Using boolean logic ensures that security updates are applied to the correct machines without affecting already-compliant departments.

---

<div align="center">
  <sub><i>Disclaimer: This is a fictional case study completed for educational purposes as part of the Google Cybersecurity Certificate.</i></sub>
</div>
