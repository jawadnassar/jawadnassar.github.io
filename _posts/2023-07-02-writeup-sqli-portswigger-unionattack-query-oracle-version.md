---
layout: post
title: "SQLi attack, querying the database type and version on Oracle"
date: 2023-07-02
categories:
  - CTF
  - PortSwigger
tags:
  - SQLi
---

## Description
This [lab](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle){:target="_blank"} contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

## Querying the database type and version
Different databases provide different ways of querying their version. You often need to try out different queries to find one that works, allowing you to determine both the type and version of the database software.

The queries to determine the database version for some popular database types are as follows:

|Database type |Query |
|---|---|
|Oracle|`SELECT banner FROM v$version` <br>  `SELECT version FROM v$instance`|
|Microsoft|`SELECT @@version`|
|PostgreSQL|`SELECT version()`|
|MySQL|`SELECT @@version`|

## Steps

Given that it's an Oracle DB, it is mandatory for every `SELECT` statement to include a `FROM` clause. Oracle offers a placeholder table named `DUAL`, which can be utilized to help us determine the number of columns and their types.

For instance, through this union attack `?category=Pets' UNION SELECT banner,'a' FROM v$version--`, we can execute the subsequent query: `SELECT banner FROM v$version`.

<img src="https://jawad.ca/images/july2023/1.png" >

and it's solved! 
