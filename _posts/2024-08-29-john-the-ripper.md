---
layout: post
title: "John the Ripper: CPU password cracking"
date: 2024-08-29
description: "John the Ripper: cracking hashes on the CPU, the wordlist and incremental modes, and when it beats reaching for Hashcat."
categories:
  - TIL
tags:
  - Passwords
---

J**ohn the Ripper** (often referred to as "John") is a widely-used password cracking tool designed for detecting weak passwords. It supports various cryptographic hash types and can perform different attack modes to recover passwords.

**Key Usages:**

1.  **Basic Password Cracking**: By default, John uses a built-in wordlist and simple rules.

    ```bash
    john hashes.txt
    ```
2.  **Specifying a Wordlist**: Use a custom wordlist for dictionary attacks.

    ```bash
    john --wordlist=wordlist.txt hashes.txt
    ```
3.  **Incremental Mode (Brute Force)**: Attempts all possible character combinations.

    ```bash
    john --incremental hashes.txt
    ```
4.  **Single Crack Mode**: Uses the username and GECOS information to generate password guesses.

    ```bash
    john --single hashes.txt
    ```
5.  **External Mode**: Allows for custom cracking algorithms using C-like syntax.

    ```bash
    john --external=MODE hashes.txt
    ```

**Tips and Tricks:**

1.  **Using `--show` to Display Cracked Passwords**: After cracking, use `--show` to see the results.

    ```bash
    john --show hashes.txt
    ```
2.  **Resume Cracking**: If you need to stop and resume later, John supports session management.

    ```bash
    john --session=yourSessionName hashes.txt
    john --restore=yourSessionName
    ```
3.  **Using Rules to Enhance Dictionary Attacks**: Apply rules to generate variations of words from the wordlist.

    ```bash
    john --wordlist=wordlist.txt --rules hashes.txt
    ```
4.  **Adjusting Cracking Mode Settings**: Customize settings for incremental mode.

    ```bash
    john --incremental=alpha --min-length=8 --max-length=12 hashes.txt
    ```
5.  **Cracking Specific Hash Types**: Explicitly specify the hash format if John does not auto-detect it.

    ```bash
    john --format=NT hashes.txt
    ```
6.  **Combining Wordlists**: Use multiple wordlists to increase the chances of finding the password.

    ```bash
    john --wordlist=wordlist1.txt --wordlist=wordlist2.txt hashes.txt
    ```
7.  **Parallel Cracking with MPI**: For faster results, use MPI to run John in parallel on multiple processors.

    ```bash
    mpiexec -np 4 john hashes.txt
    ```
