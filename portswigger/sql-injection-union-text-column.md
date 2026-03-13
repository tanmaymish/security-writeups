# SQL Injection – Finding a Text Column

Platform: PortSwigger Web Security Academy  

## Overview

To retrieve text data using UNION attacks, attackers must find a column that accepts string values.

## Exploitation

Payload example:

' UNION SELECT NULL,'test',NULL--

The column displaying "test" indicates a text-compatible column.

## Result

The attacker identifies a column suitable for displaying extracted data.

## Impact

Allows further data extraction from the database.

## Mitigation

• Parameterized queries  
• Input validation
