---
layout: default
title: インストール方法
---

---

# HOW TO INSTALL

## Preparation

- Please create database by MySQL or PostgreSQL
- Set html folder become DocumentRoot
- If html is not DocumentRoot, it will become http://{DocumentRoot}/{htmlへのパス} 

## Installation

There are 2 ways to install EC-CUBE

- Shell script installer
- Web Installer

## Insalling by Shell script installer

That fits with the environment, change setting content which below Configuration and near line 51 of `eccube_install.sh` then run.

- Incase of PostgreSQL

```
eccube_install.sh pgsql
```

- Incase of MySQL

```
eccube_install.sh mysql
```

After installation, access  `http://{インストール先URL}/admin`.
If Login screen of EC-CUBE Management appear, installation is success.Please log in by ID/Password below.

`ID: admin PW: password`

## Insalling by Web installer

- Use the composer and install an external library

```
curl -sS https://getcomposer.org/installer | php
php ./composer.phar install --dev --no-interaction
```

- Access Web installer 

After access to `http://{インストール先URL}/install/`, install according to displayed installer's instruction.




