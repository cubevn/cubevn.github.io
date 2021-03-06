---
title: Installation
keywords: howto install 
tags: [quickstart, getting_started]
sidebar: home_sidebar
forder: quickstart
---

## Advanced preparation

- Prepare database with MySQL or PostgreSQL in advance.
- Set DocumentRoot of the site to be the EC-CUBE html folder

※If DocumentRoot can not be changed, Top page URL will be http://サイトURL/html/

※However, from 3.0.11 you can install without html URL by doing this step [こちらの手順](/quickstart_remove-html)


## How to install

There are 2 ways to install EC - CUBE as follows:

- Install by install script
- Install by web installer

## Install by install script

You can install by command line `eccube_install.php`.

Please execute as below

`php eccube_install.php [mysql|pgsql|sqlite3] [none] [options]`

The following are examples of command execution

In case of PostgreSQL

```
php eccube_install.php pgsql
```

In case of MySQL

```
php eccube_install.php mysql
```

When you want to change host and name of database, you need to specify it by environment variables. 

The following are examples of setting DBSERVER and DBNAME

```
export DBSERVER=xxx.xxx.xxx.xxx
export DBNAME=eccube_dev_db

php eccube_install.php mysql
```

About other settings and options, you can refer with `--help`

```
php eccube_install.php --help
```

When installation completed, access to `http: // {installation URL} / admin`

If the EC-CUBE administration login screen is displayed, the installation is successful. Please login with the following ID / Password:

`ID: admin PW: password`

Because Web installer I described below is not neccessary anymore, you can delete it 

```
rm html/install.php
```

## Install by web installer

- Get source code by using composer

```
curl -sS https://getcomposer.org/installer | php
php composer.phar create-project ec-cube/ec-cube ec-cube "^3.0"
```

- Access the web installer

Access to `http://{インストール先URL}/install.php` and follow the instructions of the displayed installer to install

