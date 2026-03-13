# SQL Injection – Retrieving Data from Other Tables

Platform: PortSwigger Web Security Academy  

## Overview

After identifying column count and text columns, attackers can extract sensitive data.

## Exploitation

Payload used:

' UNION SELECT username,password FROM users--

## Result

Usernames and passwords are displayed in the application response.

## Impact

Attackers can steal user credentials.

## Mitigation

• Use prepared statements  
• Hash passwords  
• Restrict database permissions
