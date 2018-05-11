---
title: Installation
keywords: howto install 3n
tags: [quickstart, getting_started]
sidebar: home_sidebar
forder: quickstart
---


## How to install

There are 2 ways to install EC - CUBE as follows:

- Install by command line
- Install by web installer

## Install by command line

As a prerequisite, you need to [install Composer](https://getcomposer.org/download/)

```
php composer.phar create-project ec-cube/ec-cube ec-cube "dev-experimental/sf"
```

In the above example, SQLite 3 is selected for database.

Change directory to folder ec-cube, execute command `bin/console server:run`, the build-in web server will launches.

```
cd ec-cube
bin/console server:run
```

Access to URL `http://127.0.0.1:8000/admin`, if EC-CUBE admin login screen is displayed that mean installation is successful.

Login with ID/Password as follows:
```
ID: admin PW: password
```

To quit, press `Ctrl + C`

### When you want to change database type
After install, execute command `bin/console eccube:install` , setting `Database Url` as follows:
```
## for MySQL
mysql://<user>:<password>@<host>/<database name>

## for PostgreSQl
postgres://<user>:<password>@<host>/<database name>
```

Execute 4 commands as follows:
```
php bin/console doctrine:database:create
php bin/console doctrine:schema:create
php bin/console doctrine:schema:update --force
php bin/console eccube:fixtures:load
```

## Install by web installer
As a prerequisite, you need to [install Composer](https://getcomposer.org/download/)

```
php composer.phar create-project --no-scripts ec-cube/ec-cube ec-cube "dev-experimental/sf"
```

Change directory to folder ec-cube, execute command `bin/console server:run`, the build-in web server will launches.

```
cd ec-cube
bin/console server:run
```

Access to URL `http://127.0.0.1:8000`, the web installer will be displayed. Install according to the instructions.

