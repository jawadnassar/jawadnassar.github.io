---
layout: post
title: "Google dorking"
date: 2024-08-19
categories:
  - TIL
tags:
  - Recon
  - Web
---

**Search Engine** hacking also known as Google Hacking or Google Dorks; involves using advanced search operators in Google to find specific information or vulnerabilities on websites.

**Commands:**

* **intitle:** Search for words in the title of a webpage.
* **inurl:** Search for words in the URL.
* **filetype:** Search for a specific file type.
* **site:** Search within a specific site or domain.

**Examples:**

1. **Search for Specific File Types:**
   * Command: `filetype:`
   * Example: `filetype:pdf site:example.com` (Finds PDF files on the specified site)
2. **Explore Directory Listings:**
   * Command: `intitle:"index of"`
   * Example: `intitle:"index of" /secret` (Finds directories named "secret")
3. **Find Login Pages:**
   * Example: `intitle:"login" site:example.com` (Locates login pages on the specified site)
4. **Search for Vulnerabilities:**
   * Example: `site:example.com ext:php intitle:"Index of" "db"`
5. **Locate Exposed Documents:**
   * Example: `filetype:doc site:example.com` (Finds Microsoft Word documents on the specified site)
6. **Search for Backup Files:**
   * Example: `filetype:bkf intitle:"index of" site:example.com` (Finds backup files on the specified site)
7. **Explore Subdomains:**
   * Example: `site:*.example.com` (Lists subdomains of example.com)
8. **Search for Passwords:**
   * Example: `filetype:log inurl:"password.log"` (Looks for log files containing passwords)
9. **Find Apache Default Pages:**
   * Example: `intitle:"Apache2 Debian Default Page"` (Finds Apache default pages on Debian)

Useful resources:

* [https://dorksearch.com](https://dorksearch.com/)
* [https://www.exploit-db.com/google-hacking-database](https://www.exploit-db.com/google-hacking-database)
