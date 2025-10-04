---
layout: post
title: "Canarytokens"
date: 2023-05-27
categories: [CHEATSHEET]
---

To create a Canarytoken, you'll typically use a service like [canarytokens.org](https://canarytokens.org){:target="_blank"} , which provides a web interface for generating different types of tokens.
<br>

## Example Use Cases

* **Honeypot Files**: Place a Canarytoken in a file named `config_backup.tar` on a server. If the file is accessed or downloaded, the token alerts you to the potential breach.
* **Fake Database Credentials**: Include a Canarytoken within database connection strings in application configuration files. Unauthorized attempts to connect using these credentials would trigger an alert.
* **Private URL Access**: Insert Canarytokens in unused but monitored internal URLs to detect scanning or unauthorized access within the network.
* During social engineering assessments, Canarytokens can help gauge an individual’s or group’s susceptibility to phishing attacks.

<br>

### Similar useful tools:

* [https://grabify.link/](https://grabify.link/)
* [https://github.com/fingerprintjs/fingerprintjs](https://github.com/fingerprintjs/fingerprintjs)
