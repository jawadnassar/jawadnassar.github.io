---
layout: post
title: "Setting up FoxyProxy with Burp Suite"
date: 2023-12-28
categories:
  - TIL
tags:
  - Web
---

To begin, download Burp Suite Community Edition and ensure that the Proxy listener is enabled.

![Burp Suite Proxy listeners showing 127.0.0.1:8080 listener enabled](https://jawad.ca/images/sqli-lab01/sqli-lab1-6.png)

Download the FoxyProxy extension and add the Burp Suite proxy address and port.

![FoxyProxy Add Proxy dialog set to HTTP 127.0.0.1 port 8080](https://jawad.ca/images/sqli-lab01/sqli-lab1-5.png)

![FoxyProxy toolbar menu showing burp proxy enabled for all URLs](https://jawad.ca/images/sqli-lab01/sqli-lab1-7.png)

Enable the Burp proxy and navigate to the interface where Burp is running `127.0.0.1:8080`. Save the CA certificate when prompted.

![Burp Suite Community Edition welcome page at 127.0.0.1:8080 with CA Certificate link](https://jawad.ca/images/sqli-lab01/sqli-lab1-10.png)

To prevent Firefox SSL errors when using Burp, import the certificate we just downloaded by going to `about:preferences#privacy` in Firefox.

![Firefox Certificate Manager Authorities tab showing imported PortSwigger CA](https://jawad.ca/images/sqli-lab01/sqli-lab1-11.png)
