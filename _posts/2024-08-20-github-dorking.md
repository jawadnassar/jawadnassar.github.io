---
layout: post
title: "GitHub dorking: finding exposed secrets"
date: 2024-08-20
categories:
  - TIL
tags:
  - Recon
  - Web
---

Open source software, while fostering collaboration, can introduce information security risks during the information gathering phase.

Searching for potentially sensitive information on GitHub, sometimes referred to as "GitHub hacking" or "GitHub dorking," involves using advanced search queries to discover repositories or code snippets containing unintentionally exposed credentials, proprietary code, or other sensitive data. While GitHub provides a powerful platform for collaboration, it's essential to be aware that information might inadvertently be disclosed.

Here's a brief overview:

1. **Exposed Credentials:**
   * **Example:** `filename:.env password`
   * **Purpose:** Searches for files named ".env" that may contain sensitive information like passwords.
2. **API Keys and Tokens:**
   * **Example:** `filename:config.js token`
   * **Purpose:** Looks for configuration files that might include API keys or authentication tokens.
3. **Specific Code Patterns:**
   * **Example:** `filename:docker-compose.yml aws_access_key_id`
   * **Purpose:** Identifies Docker Compose files that might contain AWS access key information.
4. **Vulnerable Dependencies:**
   * **Example:** `filename:package.json "dependencies" "vulnerable"`
   * **Purpose:** Searches for package.json files indicating dependencies with known vulnerabilities.
5. **Source Code Leaks:**
   * **Example:** `filename:.gitattributes password`
   * **Purpose:** Scans for Git attribute files that may inadvertently expose sensitive information.
6. **Public API Keys:**
   * **Example:** `filename:config.json api_key`
   * **Purpose:** Searches for configuration files containing API keys in JSON format.

**Resources:**

* [https://github.com/features/code-search](https://github.com/features/code-search)
* [https://github.com/search](https://github.com/search)
