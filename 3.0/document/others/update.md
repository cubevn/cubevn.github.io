---
layout: default
title: アップデート方法
---

---

# How to Update

Here is explanation about how to version up the migration installed from EC-CUBE 3.0.2.

About preparing migration used when development, please refer [マイグレーションガイド](migration.html).

## Process

- Overwrite the Directory below
    - src/
    - html/
    - vendor/
+ Access `http://インストール先/install.php/migration`and run migration.
+ Delete `html/install.php`

## Notes

### About updating vendor

* If composer.json doesn't have any change, do not need to overwrite vendor.
* If the environment can use composer, it's not overwriting vendor but after overwriting, the below file can use composer.json, composer.lock.

```
> php composer.phar install --no-dev --no-interaction --optimize-autoloader
```

### In case file below html and app directory was changed 

* If the `html` and `app` files below have been changed, it's neccesary to reflect the differences between each file.
* Refer `各バージョンでの変更差分` then apply these differences.

### In case customize the template 

* When customize the template, please check the changes difference of `src/Eccube/Resource/template`.
* Refer `各バージョンでの変更差分` then apply these differences.

## Changes difference in each Version 

### 3.0.2→3.0.3

[https://github.com/EC-CUBE/ec-cube/compare/3.0.2...3.0.3](https://github.com/EC-CUBE/ec-cube/compare/3.0.2...3.0.3)

### 3.0.3→3.0.4

[https://github.com/EC-CUBE/ec-cube/compare/3.0.3...3.0.4](https://github.com/EC-CUBE/ec-cube/compare/3.0.3...3.0.49

### 3.0.4→3.0.5

[https://github.com/EC-CUBE/ec-cube/compare/3.0.4...3.0.5](https://github.com/EC-CUBE/ec-cube/compare/3.0.4...3.0.5)

### 3.0.5→3.0.6

[https://github.com/EC-CUBE/ec-cube/compare/3.0.5...3.0.6](https://github.com/EC-CUBE/ec-cube/compare/3.0.5...3.0.6)

・This time, `autoload.php`  also becomes update target, please be careful.

### 3.0.6→3.0.7

[https://github.com/EC-CUBE/ec-cube/compare/3.0.6...3.0.7](https://github.com/EC-CUBE/ec-cube/compare/3.0.6...3.0.7)

### 3.0.7→3.0.8

[https://github.com/EC-CUBE/ec-cube/compare/3.0.7...3.0.8](https://github.com/EC-CUBE/ec-cube/compare/3.0.7...3.0.8)

・Who use index_dev.php, please run  
``` php composer.phar update symfony/var-dumper symfony/debug-bundle ```
and install the required libraries.

### 3.0.8→3.0.9

[https://github.com/EC-CUBE/ec-cube/compare/3.0.8...3.0.9](https://github.com/EC-CUBE/ec-cube/compare/3.0.8...3.0.9)

Below files also become update targets

- app/console
- cli-config.php
- composer.json
- composer.lock
- eccube_install.sh
- html/index.php
- html/index_dev.php
- html/template/install/assets/js/function.js


・Because it also includes libraries using for dump from 3.0.9, so
```
php composer.phar update symfony/var-dumper symfony/debug-bundle
```  
is unneccesary 

### 3.0.9→3.0.10

[https://github.com/EC-CUBE/ec-cube/compare/3.0.9...3.0.10](https://github.com/EC-CUBE/ec-cube/compare/3.0.9...3.0.10)

Below files also become update targets

- autoload.php
- html/index.php
- html/index_dev.php
- html/template/admin/assets/css/dashboard.css
- html/template/default/css/style.css
