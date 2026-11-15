---
layout: post
title: "Canarytokens: alerts when a file is opened"
date: 2024-08-21
categories:
  - TIL
---

Canarytokens are a type of security tool that help detect breaches by generating tokens that act as tripwires or decoys. When interacted with, these tokens send alerts, allowing organizations to identify unauthorized access or data exfiltration attempts early. They can be used in various formats, such as URLs, DNS records, file accesses, and more.

## How to Use Canarytokens

**1. Creating a Canarytoken**

To create a Canarytoken, you'll typically use a service like canarytokens.org, which provides a web interface for generating different types of tokens:

* **Choose the type of token**: Select the type of token you want to create, such as URL, email address, or file.
* **Generate the token**: Enter the necessary information and generate the token. For example, for a URL token, you might specify where the alert should be sent when the token is accessed.

**2. Deploying Canarytokens**

Once you have your token, the next step is to deploy it in a way that is likely to tempt an intruder but goes unnoticed by legitimate users:

* **Files**: Place a document token in strategic locations on your network. This could be a fake confidential file named "Employee Salary Details 2023.docx" that, when opened, triggers an alert.
* **Emails**: Embed email tokens in contact forms or employee directories. If someone uses this tokenized email, it sends an alert.
* **URLs**: Use URL tokens in your network's internal documentation. Accessing these URLs triggers a notification.

**3. Monitoring and Responding to Alerts**

* **Alert Setup**: When creating a token, you configure where alerts should be sent, such as an email address or a webhook.
* **Incident Response**: Once an alert is triggered, it should be followed up to determine if it was a false positive or if further investigation is needed.

## Example Use Cases

* **Honeypot Files**: Place a Canarytoken in a file named `config_backup.tar` on a server. If the file is accessed or downloaded, the token alerts you to the potential breach.
* **Fake Database Credentials**: Include a Canarytoken within database connection strings in application configuration files. Unauthorized attempts to connect using these credentials would trigger an alert.
* **Private URL Access**: Insert Canarytokens in unused but monitored internal URLs to detect scanning or unauthorized access within the network.

During social engineering assessments, Canarytokens can help gauge an individual’s or group’s susceptibility to phishing attacks:

* **Embed in Emails**: Insert URL Canarytokens into emails that mimic phishing attempts. Tracking who clicks on the links can help identify vulnerabilities in user training and awareness.

## Example Setup for Pen Testing

**Step 1**: Place a document with a file Canarytoken in a supposedly restricted network share.

**Step 2**: During the pen-testing phase, access this file to simulate an insider or an external attacker accessing sensitive data.

**Step 3**: Monitor for alerts and assess how long it takes for the security team to respond and what actions they take.

## Similar useful tools:

* [https://grabify.link/](https://grabify.link/)
* [https://github.com/fingerprintjs/fingerprintjs](https://github.com/fingerprintjs/fingerprintjs)
