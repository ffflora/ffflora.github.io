---
title: "Error When Copy Table to PostgreSQL"
date: 2022-05-27T14:44:17-07:00
draft: false
toc: false
images:
tags:
  - debugging
categories:
  - Database
  - Data Science
---



I have encountered these error messages when uploading a table from local to a remote server:



```sql
COPY schemaname.tablename 
FROM 'path/src/data/file.csv' WITH delimiter E'\t' CSV QUOTE AS '"' HEADER;
```

```shell
ERROR .....
HINT:  COPY FROM instructs the PostgreSQL server process to read a file. You may want a client-side facility such as psql's \copy.
```

And when I added the backslash to the `copy` command, this shown:

```shell
error: \copy: parse error at end of line
psql:sql/file.sql:60: ERROR:  syntax error at or near "FROM"
LINE 1: FROM 'path/src/data/file.c...
```

QUICK FIX:

- Add the backslash in front of the `copy` command - `\COPY`
- Make the whole command in **<u>one</u>** line instead of two. 

```shell
\COPY schemaname.tablename FROM 'path/src/data/file.csv' WITH delimiter E'\t' CSV QUOTE AS '"' HEADER;
```

