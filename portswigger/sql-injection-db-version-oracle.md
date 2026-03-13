# SQL Injection – Database Version Enumeration (Oracle)

Platform: PortSwigger Web Security Academy  
Category: SQL Injection  

## Overview

The application is vulnerable to SQL injection which allows attackers to determine database type and version.

## Exploitation

Payload used:

' UNION SELECT banner, NULL FROM v$version--

This query retrieves version information from Oracle's system tables.

## Result

The database version information is displayed in the application response.

## Impact

Attackers can identify the backend database system which helps plan further attacks.

## Mitigation

• Parameterized queries  
• Restrict database error messages  
• Input sanitization
