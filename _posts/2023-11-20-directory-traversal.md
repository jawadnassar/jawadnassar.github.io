---
layout: post
title: "Directory Traversal"
date: 2023-11-20
categories: [CHEATSHEET]
---

A critical web vulnerability enabling attackers to access files outside of the web server's root directory. It exploits inadequate validation of user-supplied file names, leading to unauthorized file system access.

## Mechanism:

  
* Attackers manipulate file path inputs to traverse the server's directory structure (e.g., using `../../../../etc/passwd` to access system files).
* The attack exploits web applications that construct file paths from user input without thorough sanitization.

### Example of Vulnerable Code:
  

Consider a PHP script where file paths are generated from user input without sufficient validation:

```php
$file = $_GET['file']; 
include('/var/www/html/' . $file);
```

An attacker can manipulate the `file` parameter to traverse directories, such as `/var/www/html/?file=../../../../etc/passwd`.

## Common Attack Patterns:

  
* Basic Traversal: Using `../` to move up in the directory hierarchy.
* Encoded Traversal: Utilizing URL or Unicode encoding to bypass naive filters (e.g., `%2e%2e%2f` for `../`).
* Nested Traversal: Combining multiple traversal sequences (e.g., `....//....//`).


  

## Test Case Example:

  
* If you encounter a parameter like `?file=report.pdf`, try altering it to `?file=../../../../etc/passwd`. If the server returns system files, it's vulnerable.
* File Upload vulnerability: Using path traversal sequences (e.g., ../../) in uploaded file names to overwrite critical files.
* Testing should include encoded and double-encoded traversal sequences to bypass common security filters.
* You should test if you can find any ssh private keys under `/home/user/.ssh/id_rsa`  download it via `curl` then `ssh -i private_key -port 22 user@hostname`

> ***Note:***
> 
> make sure to `chmod private_key 400` to bypass ssh unprotected private key file error


## Directory Traversal in Windows Environments:

In Windows environments, Directory Traversal vulnerabilities manifest uniquely due to the different file system structure. 

Attackers might use sequences like `..\` to traverse directories, aiming to access critical system files such as `C:\Windows\System32\`.  

A typical attack might involve manipulating web application input to redirect or access files using patterns like `..\..\..\Windows\win.ini` or `%SYSTEMROOT%\..\..\Windows\System32\drivers\etc\hosts`. 

Windows systems also have different character sets and path naming conventions (like using `\` instead of `/`), which can lead to variations in exploitation techniques. 

For instance, URL-encoded representations (`%5C` for `\` and `%2E%2E%5C` for `..\`) might be used to bypass basic input filters. When testing in Windows environments, it's crucial to consider these variations and test for both forward and backward slashes, as well as URL-encoded characters in traversal sequences.


> ***Note:***
>
> On Windows environments, If you are in the root directory, you can skip `C:\`

