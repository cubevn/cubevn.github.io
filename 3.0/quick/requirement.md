---
title: System Requirements
keywords: sample homepage
tags: [quickstart, getting_started]
sidebar: home_sidebar
permalink: quickstart_requirement
---


# System Requirement

| Type | Software |Version|Already confirmed|
|---|-------|---|-------|
|WebServer|IIS | 8.x | 8.0 |
|WebServer|Apache |2.2.x / 2.4.x <br> (mod_rewrite / mod_ssl REQUIRED) | 2.2.15 |
|PHP | PHP | 5.3.9 ～ 7.1.x |5.4.39 / 7.0.9 / 7.1.2 |
|Database|PostgreSQL| 8.4.x / 9.x <br> (Reference authority of pg_settings table REQUIRED) |8.4.20|
|Database|MySQL|5.1.x / 5.5.x / 5.6.x / 5.7.x <br> (InnoDB engine REQUIRED) |5.1.73|
|Database|SQLite(For development use) |3.x |3.6.20|

# PHP library

| Type | Library |
|---|---|
|Required library|pgsql / mysql / mysqli (Same with Database you use) <br> pdo_pgsql / pdo_mysql / pdo_sqlite (Same with Database you use) <br> pdo <br> phar <br> mbstring <br> zlib <br> ctype <br> session <br> JSON <br> xml <br> libxml <br> OpenSSL <br> zip <br> cURL <br> fileinfo |
|Recommended library|hash <br> APCu / WinCache (Same with Environment you use) <br> Zend OPcache <br> mcrypt |

# Operation confirmation browser

Administration screen and Standard front template

| OS | Browser|
|---|-------|
|Windows (From Windows7) | From Internet Explorer10|
||FireFox latest |
|| Google Chrome latest |
|Mac (From OS X)|Safari latest|
|iOS (From 7)|Safari latest|
|Android (From 4.1)| Standard browser latest|
