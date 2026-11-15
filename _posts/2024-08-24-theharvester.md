---
layout: post
title: "theHarvester: emails, hosts, subdomains"
date: 2024-08-24
categories:
  - TIL
tags:
  - Recon
---

theHarvester is a tool designed for open-source intelligence (OSINT) gathering, specifically to help penetration testers in the early stages of reconnaissance. It collects information from various public sources to aid in the enumeration of emails, subdomains, hosts, employee names, open ports, and banners.

## Basic Usage of theHarvester

**Collecting Information:** To use theHarvester, specify the target domain and the source for data collection. For instance, to gather emails and subdomains related to a specific domain from Google:

```bash
theharvester -d example.com -b google
```

This command searches Google for any listings that mention emails or subdomains associated with "example.com".

## Advanced Usage Examples

**Specifying the Limit of Results:** You can limit the number of search results returned by the tool:

```bash
theharvester -d example.com -b google -l 500
```

This command tells theHarvester to fetch up to 500 records from Google.

**Combining Sources:** theHarvester can aggregate information from multiple sources to maximize the data collection:

```bash
theharvester -d example.com -b all
```

This runs the search across all supported sources, including search engines, social media platforms, and specialized databases.

**Saving Output to a File:** For analysis and reporting, you can direct the output of theHarvester to a file in different formats like plain text, XML, or HTML:

```bash
theharvester -d example.com -b google -f report.html
```

This saves the gathered data into an HTML file named "report.html".

**Using Advanced Search Operators:** You can use advanced search operators to refine the data collection. For instance, if you only want to collect email addresses:

```bash
theharvester -d example.com -b google -l 100 -h my
```

This limits the search to email addresses up to 100 entries, ensuring focused and relevant results.

## Practical Applications

* **Penetration Testing Pre-Assessment:** Gather data about potential target domains, which can be useful for crafting social engineering attacks or for understanding the target's external footprint.
* **Security Auditing:** Companies can use theHarvester to check what information about them is freely available on the internet, which may pose a security risk if exposed.
* **Competitive Intelligence:** It's also used for competitive research, to see what kind of online presence and public-facing infrastructure competitors have.

theHarvester is a simple yet powerful tool for the first stages of a security analysis, helping gather extensive data about a target from publicly accessible sources with minimal effort.

## Similar Usefull Tool(s):

* [https://github.com/xmendez/wfuzz](https://github.com/xmendez/wfuzz)
