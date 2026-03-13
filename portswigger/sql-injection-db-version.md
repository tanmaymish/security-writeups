# SQL Injection – Database Version Enumeration

Platform: PortSwigger Web Security Academy  
Category: SQL Injection  

## Overview

This lab demonstrates how attackers can determine the backend database type.

## Exploitation

Payload used:

' UNION SELECT @@version--

The query retrieves version information from the database.

## Result

The response contains database version information such as MySQL or Microsoft SQL Server.

## Impact

Database fingerprinting allows attackers to tailor further attacks.

## Mitigation

• Use prepared statements  
• Hide database errors  
• Sanitize inputs
