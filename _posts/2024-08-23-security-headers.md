---
layout: post
title: "Security headers: auditing HTTP responses"
date: 2024-08-23
description: "SecurityHeaders.com grades a site's response headers, and what a missing CSP, HSTS, or X-Frame-Options really costs."
categories:
  - TIL
tags:
  - Web
---

**[SecurityHeaders.com](https://securityheaders.com)** is a web-based tool that analyzes HTTP response headers for a given website, evaluating its security posture.

**Key Features:**

* Evaluate HTTP Security Headers
* Identify Missing or Misconfigured Headers
* Provide Security Recommendations

**Example:**

![Security Headers report for notes.jawad.ca showing an A grade, with Strict-Transport-Security, Content-Security-Policy, Referrer-Policy and X-Content-Type-Options present, and X-Frame-Options and Permissions-Policy missing.](https://jawad.ca/images/security-headers.png)
