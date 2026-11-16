---
layout: post
title: "Searchsploit: searching Exploit-DB offline"
date: 2024-08-27
description: "Searchsploit: searching Exploit-DB offline, filtering by product and version, and copying a hit into the working directory."
categories:
  - TIL
tags:
  - Linux
---

[Searchsploit ](https://www.exploit-db.com/searchsploit)is an essential command-line utility bundled with Kali Linux, designed to help security professionals and penetration testers search the Exploit Database for known vulnerabilities and associated exploits. This tool streamlines the process of finding relevant exploits for various software and hardware, making it a valuable asset in security assessments.

## Basic Usage of Searchsploit

To use Searchsploit, you simply enter the command followed by a search term related to the software or system you're investigating. Here are a few command examples:

**Searching for Exploits:** To search for all exploits related to Apache:

```bash
searchsploit apache
```

This command will list all exploits in the database that mention "Apache."

**Refined Search:** To narrow down the search results to specific terms, you can use:

```bash
searchsploit apache 2.4
```

This command will return exploits specifically targeted at Apache version 2.4.

**Viewing Detailed Information:** If you want to see more details about a particular exploit, including the file path:

```bash
searchsploit -p 1337
```

Here, "1337" would be the exploit ID, and the `-p` flag tells Searchsploit to show the path to the exploit file.

**Copying an Exploit to Your Working Directory:** If you find an exploit that you wish to examine or use, you can copy it to your current working directory:

```bash
searchsploit -m linux/remote/39215.c
```

The `-m` option copies the exploit file (in this case, `39215.c`) from the Exploit Database to your current directory.

**Updating Searchsploit:** To ensure you have the latest exploits from the Exploit Database, you can update Searchsploit:

```bash
searchsploit -u
```

This command updates the local copy of the Exploit Database, ensuring your searches return the most recent information.
