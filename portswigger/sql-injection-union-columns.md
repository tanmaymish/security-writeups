# SQL Injection – Determining Number of Columns

Platform: PortSwigger Web Security Academy  

## Overview

To perform UNION attacks, attackers must first determine the number of columns returned by the query.

## Exploitation

Using ORDER BY:

' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--

The query fails when exceeding the number of columns.

## Result

The number of columns in the query is identified.

## Impact

Allows attackers to craft UNION SELECT payloads.

## Mitigation

• Parameterized queries  
• Input validation
