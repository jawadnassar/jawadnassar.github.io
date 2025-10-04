---
layout: post
title: "Setting up FoxyProxy with Burp Suite"
date: 2023-12-28
categories: [Lab]
---

To begin, download Burp Suite Community Edition and ensure that the Proxy listener is enabled.

![](https://jawad.ca/images/sqli-lab01/sqli-lab1-6.png)

Download the FoxyProxy extension and add the Burp Suite proxy address and port.

![](https://jawad.ca/images/sqli-lab01/sqli-lab1-5.png)

![](https://jawad.ca/images/sqli-lab01/sqli-lab1-7.png)

Enable the Burp proxy and navigate to the interface where Burp is running `127.0.0.1:8080`. Save the CA certificate when prompted.

![](https://jawad.ca/images/sqli-lab01/sqli-lab1-10.png)

To prevent Firefox SSL errors when using Burp, import the certificate we just downloaded by going to `about:preferences#privacy` in Firefox.

![](https://jawad.ca/images/sqli-lab01/sqli-lab1-11.png)
