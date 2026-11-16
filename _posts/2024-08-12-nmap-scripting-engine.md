---
layout: post
title: "Nmap Scripting Engine (NSE)"
date: 2024-08-12
description: "Nmap Scripting Engine: what NSE scripts add to a scan, the categories they fall into, and the flags that run them."
categories:
  - TIL
tags:
  - Recon
---

**Introduction:** Nmap Scripting Engine (NSE) is a powerful feature of Nmap that allows users to extend the functionality of the tool by running scripts to perform a wide range of tasks during the scanning process. NSE scripts provide additional capabilities, including service version detection, vulnerability scanning, and more.

**Usage:**

* `-sC`: Equivalent to running default scripts
* `--script=<script(s)>`: Specify individual scripts or script categories to run
* `--script-args=<args>`: Pass arguments to scripts

**Common NSE Script Categories:**

1. **Discovery:**
   * `discovery` category includes scripts for discovering additional information about hosts.
2. **Vulnerability Detection:**
   * `vuln` category includes scripts for identifying potential vulnerabilities on scanned hosts.
3. **Service Version Detection:**
   * `version` category scripts help in identifying service versions running on open ports.

**Examples:**

*   Run default scripts against a target:

    ```bash
    nmap -sC example.com
    ```
*   Run specific script categories:

    ```bash
    nmap --script=default,vuln example.com
    ```
*   Run individual scripts with arguments:

    ```bash
    nmap --script=http-title --script-args="path=/index.html" example.com
    ```

**Custom Script Development:**

* Users can create (or download new) custom NSE scripts to address specific needs.
* on kali, list of NSEs are found under

```shell
kali@kali:~$ ll /usr/share/nmap/scripts
total 4960
-rw-r--r-- 1 root root  3901 Nov  1 22:10 acarsd-info.nse
-rw-r--r-- 1 root root  8749 Nov  1 22:10 address-info.nse
-rw-r--r-- 1 root root  3345 Nov  1 22:10 afp-brute.nse
-rw-r--r-- 1 root root  6463 Nov  1 22:10 afp-ls.nse
-rw-r--r-- 1 root root  7001 Nov  1 22:10 afp-path-vuln.nse
-rw-r--r-- 1 root root  5600 Nov  1 22:10 afp-serverinfo.nse
-rw-r--r-- 1 root root  2621 Nov  1 22:10 afp-showmount.nse
-rw-r--r-- 1 root root  2262 Nov  1 22:10 ajp-auth.nse
-rw-r--r-- 1 root root  2983 Nov  1 22:10 ajp-brute.nse
-rw-r--r-- 1 root root  1329 Nov  1 22:10 ajp-headers.nse
-rw-r--r-- 1 root root  2590 Nov  1 22:10 ajp-methods.nse
-rw-r--r-- 1 root root  3051 Nov  1 22:10 ajp-request.nse
-rw-r--r-- 1 root root  6719 Nov  1 22:10 allseeingeye-info.nse
-rw-r--r-- 1 root root  1678 Nov  1 22:10 amqp-info.nse
...

```

You need to update the script DB by executing

```shell
kali@kali:~$ sudo nmap --script-updatedb
[sudo] password for kali:
Starting Nmap 7.92 ( https://nmap.org )
NSE: Updating rule database.
NSE: Script Database updated successfully.
Nmap done: 0 IP addresses (0 hosts up) scanned in 0.54 seconds

```

**http-enum**

helps in fingerprinting the webserver

```shell
kali@kali:~$ sudo nmap -p80 --script=http-enum example.com
```
