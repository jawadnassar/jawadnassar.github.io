---
layout: post
title: "SQLi: schemas"
date: 2024-05-15
description: "Querying the system catalog in MySQL, MSSQL, Oracle, and PostgreSQL to list tables and columns before dumping data."
categories:
  - TIL
tags:
  - SQLi
  - Web
---

Understanding the tables and schemas in different databases involves querying the system catalog or information schema, which provides metadata about the database's structure. Here’s a brief overview for a few common database systems:

## SQL Server

*   **Query System Catalog:**

    ```sql
    SELECT * FROM INFORMATION_SCHEMA.TABLES;
    ```

    This command lists all tables including their table schema in the database.

## MySQL

*   **Show Tables and Schemas:**

    ```sql
    SHOW TABLES;
    ```

    This command displays all tables in the current database. To find out which database you are currently using:

    ```sql
    SELECT DATABASE();
    ```

    To list all schemas (databases):

    ```sql
    SHOW DATABASES;
    ```

## PostgreSQL

*   **Query Information Schema:**

    ```sql
    SELECT table_schema, table_name FROM information_schema.tables
    WHERE table_schema NOT IN ('information_schema', 'pg_catalog');
    ```

    This command filters out system tables and shows user-defined tables and their schemas.

## Oracle

*   **List Tables:**

    ```sql
    SELECT table_name FROM user_tables;
    ```

    This command shows all tables owned by the current user. To see tables across all schemas you can query:

    ```sql
    SELECT owner, table_name FROM all_tables;
    ```

Each database system has its own set of system tables or information schema views that can be queried to understand the database structure, such as schemas, tables, columns, and relationships. These tools are crucial for database management and schema exploration.
