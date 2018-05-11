---
layout: default
title: php app/console plugin:develop を利用したプラグイン開発
---

---

# Developing Plugin using php app/console plugin:develop 


When develop Plugin, once we need to install plugin have achieved 
but from EC-CUBE 3.0.9, the procedure to develop basing on command is provided.

```
php app/console plugin:develop
```

By using command you can install and enable plugins on a command basis, 
so it becomes easy to test such as DB migration.


#### How to use plugin:develop


Plugin do not need to install ```plugin:develop``` in screen, 
you can install, uninstall, enable, disable and update on a command basis.


* example

```
php app/console plugin:develop install
php app/console plugin:develop uninstall
php app/console plugin:develop enable
php app/console plugin:develop disable
php app/console plugin:develop update
```


* How to use command 

There is ```--code[=CODE]``` in Option specification,  
If code is specified, the method corresponding with PluginManager.php of specified code will be executed.

```
php app/console plugin:develop enable --code=plugincode
```
In this example, enable function of PluginManager.php is executed


* Command can be used only by  ```plugin:develop install``` 

There is ```--path[=PATH]``` in Option specification,  
If path is specified, install the achieved Plugin   
but ```code``` does not need to specify.  

```
php app/console plugin:develop install --path=/aaa/bbb/plugin.tar.gz
```

* Command can be used only by ```plugin:develop uninstall``` 

There is ```--uninstall-force[=UNINSTALL-FORCE]``` in Option specification,  
If specify ```true```, directory of corresponding plugin will be deleted.

Default is false.

```
php app/console plugin:develop uninstall --code=plugincode --uninstall-force=true
```


