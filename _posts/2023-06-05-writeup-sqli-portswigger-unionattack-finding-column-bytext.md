---
layout: post
title: "SQLi UNION attack, finding a column containing text"
date: 2023-06-05
categories:
  - CTF
  - PortSwigger
tags:
  - SQLi
---

## Description

This [lab](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text){:target="_blank"} contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you first need to determine the number of columns returned by the query. The next step is to identify a column that is compatible with string data.

The lab will provide a random value that you need to make appear within the query results. To solve the lab, perform a SQL injection UNION attack that returns an additional row containing the value provided. This technique helps you determine which columns are compatible with string data.

## Steps

First, we need to determine the number of columns returned by the query. Let's intercept the HTTP request when filtering for the `Gifts` category and attempting an `ORDER BY` command. 

An error will occur when we exceed the number of columns. In this case, the error manifests when attempting to order by the 4th column. 

`https://0a5500f304f46a11815d48b800270079.web-security-academy.net/filter?category=Gifts' ORDER BY 4--`

Therefore, we can deduce that we have only 3 columns.

The second step involves determining the data type of the columns. We will start by assessing the data type of the 1st column. To test whether it's a String, we can employ a `UNION SELECT 'char', NULL, NULL--`. Upon testing this, an error is generated, indicating that the first column is not a string.

Consequently, let's attempt to utilize an integer for the first column:
`category=Gifts' UNION SELECT 1, NULL, NULL--`

The error disappears, affirming that the first column is indeed of integer type. As shown in the image below:

<img src="https://jawad.ca/images/june2023/1.png">

Continuing with this reasoning, we can deduce that the second column is of string type. We can solve this lab by injecting the suggested String  `hX7PcU`, as follows:
`category=Gifts' UNION SELECT 1, 'hX7PcU', NULL--`, 

and it's solved! 

<img src="https://jawad.ca/images/june2023/2.png">
