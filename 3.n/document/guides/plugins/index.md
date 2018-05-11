---
layout: default
title: plugin with version EC-CUBE 3.n
---

---

# A. What is plugins in EC-CUBE?
 - is a package( include folders, files ) that adds a specific feature to EC-CUBE core.

## 1. Plugin vs module vs add-in, add-on, extension
 - same same idea.

## 2. Plugin EC-CUBE vs [bundle of Symfony](https://symfony.com/doc/3.4/bundles.html)
 - A bundle is similar to a plugin in other software, but even better.
 - The core features of Symfony framework are implemented with bundles.
 - Plugin extend and inheritance features of EC-CUBE.
 
# B. Why do we need plugins?
 - Not change EC-CUBE core code.
    - reduce risk, reduce big problem, loose control.
    - can update new version of EC-CUBE.
 - Can enable or disable plugin but not affect your business.
 - Can update & compatible with other plugins.
 
# C. When do not we need plugins?
 - Change Symfony workflow.
 - Change EC-CUBE workflow.
 - Change theme of site.
  
# D. How about plugin ?
## **Position**

```
EC-CUBE3n Root Directory
├──    app
│       ├── cache
│       ├── config
│       ├── console
│       ├── log
│       ├── Plugin ★Plugins Folder
│       │      ├── [plugin code]  ☆root folder of the plugin
```

## I. Workflow

## 1. when you install a plugin:
* by GUI
1. Archive file to template folder
1. Validation plugin
1. Moving plugin into folder app/Plugin/
1. Insert information into table Plugin
1. Call function {install} which was define in folder [plugin code]/PluginManager.php
1. Create database for plugin if had defined in folder [plugin code]/Entity

* by command
1. Validation plugin
1. Insert information into table Plugin
1. Call function {install} which was define in folder [plugin code]/PluginManager.php
1. Create database for plugin if had defined in folder [plugin code]/Entity

## 2. when you enable a plugin:
*  GUI & command
1. Change flag to enable in table Plugin
1. Clear cache and rebuild Schema of database in folder app/Proxy/

## 3. when you disable a plugin:
*  GUI & command
1. Change flag to disable in table Plugin
1. Clear cache and rebuild Schema of database in folder app/Proxy/

## 4. when you uninstall a plugin:
*  GUI & command
1. Remove out table Plugin
1. Remove folder in app/Plugin/[plugin code]
1. Clear cache and rebuild Schema of database in folder app/Proxy/

## 5. when you update a plugin:
*  GUI & command
1. update information table Plugin
1. Replace folder in app/Plugin/[plugin code]
1. Clear cache and rebuild Schema of database in folder app/Proxy/

## 6. when running:
1. Load all Symfony and Bundles
1. Load EC-CUBE core
1. Listen feedback of plugin with each event
   * If plugin have register interactive with the event
   * Call function handle event of the event
1. Finish EC-CUBE core
1. Finish Symfony and Bundle

## II. Elements specification

### Require elements

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                               ☆Folder contain all plugins inside
│       │      ├── [plugin code]                 ★Root folder of specification plugin
│       │      │     ├── config.yml              ★Declare & information of plugin
│       │      │     ├── PluginManager.php       ★Declare & information of plugin
```

* config.yml

```
# Required elements
name: [plugin name]
code: [plugin code]
version: 1.0.0

# Optional elements with PSR-4

```

* PluginManager.php

```
class PluginManager extends AbstractPluginManager
{
    /**
     * @param array       $config
     * @param Application $app
     * will be called when install plugin
     */
    public function install($config, $app)
    {
    }
    /**
     * @param array       $config
     * @param Application $app
     * will be called when uninstall plugin
     */
    public function uninstall($config, $app)
    {
    }
    /**
     * @param array       $config
     * @param Application $app
     * will be called when enable plugin
     */
    public function enable($config, $app)
    {
    }
    /**
     * @param array       $config
     * @param Application $app
     * will be called when disable plugin
     */
    public function disable($config, $app)
    {
    }
    /**
     * @param array       $config
     * @param Application $app
     * will be called when update plugin
     */
    public function update($config, $app)
    {
    }
}
```

### Optional elements
* [Entity & Repository](https://www.doctrine-project.org/projects/doctrine-orm/en/2.6/reference/working-with-objects.html) [[more...]](https://symfony.com/doc/3.4/doctrine.html)

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   ☆Folder contain all plugins inside
│       │      ├── [plugin code]                     ☆Root folder of specification plugin
│       │      │     ├── Entity                      ★Folder contain all entity inside
│       │      │     │    ├── [Name].php             ★Declare affect to table
│       │      │     │    ├── [Name].php         
│       │      │     ├── Repository                  ★Folder contain all repository inside
│       │      │     │    ├── [Name]Repository.php   ★Declare repository
│       │      │     │    ├── [Name]Repository.php    

```

    
* [Event](https://symfony.com/doc/3.4/event_dispatcher.html)

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   ☆Folder contain all plugins inside
│       │      ├── [plugin code]                     ☆Root folder of specification plugin
│       │      │     ├── Event                       ★Folder contain all class about event
│       │      │     │    ├── [Name].php             ★Declare handle class
│       │      │     ├── [Name]Event.php             ★Will register listeners for events
```

```
class [Name]Event implements EventSubscriberInterface
{
    public static function getSubscribedEvents()
    {
        return [
            'admin.product.edit.initialize' => [['onAdminProductEditInitialize', 10]],
            'admin.product.edit.complete' => [['onAdminProductEditComplete', 10]],
            'Product/detail.twig' => [['onRenderProductDetail', 10]],
        ];
    }
    ...
    public function onRenderProductDetail(TemplateEvent $event)
    {
        // implement with $event
    }
    ...
}
```

* Some events kind were defined in EC-CUBE[will be confirmed]
    - System   : init & completed  
    - Route    : init  
    - Controler: init & completed
    - Form     : form extension
    - Template : render
    - Specify  :

* Service

* [Form Type](https://symfony.com/doc/3.4/forms.html) & [Form Extension](https://symfony.com/doc/3.4/form/create_form_type_extension.html)

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   ☆Folder contain all plugins inside
│       │      ├── [plugin code]                     ☆Root folder of specification plugin
│       │      │     ├── Form                        ★Folder contain all class about event
│       │      │     │    ├── [Type,Extension]       ★Folder contain form 
│       │      │     │    │    ├── [Name].php        ★Declare form
```

* [Template](https://symfony.com/doc/3.4/templating.html)

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                  ☆Folder contain all plugins inside
│       │      ├── [plugin code]                    ☆Root folder of specification plugin
│       │      │     ├── Resource                   ★Folder contain all class about event
│       │      │     │    ├── locale                ★Folder contain form 
│       │      │     │    │    ├── [Name].ja.php    ★Japanese    
│       │      │     │    │    ├── [Name].en.php    ★English  
│       │      │     │    ├── template              ★Folder contain form 
│       │      │     │    │    ├── admin            ★Folder contain template for admin 
│       │      │     │    │    │    ├── [Name].php      
│       │      │     │    │    ├── default          ★Folder contain template for front 
│       │      │     │    │    │    ├── [Name].php      
│       │      │     │    ├── asset                 ★Folder contain resource 
│       │      │     │    │    ├── [html,js,css,image..]
│       │      │     │    ├─- config
│       │      │     │    │    ├── services.yml     ★Contain any params, .....
```

* [Automation test- phpUnit](https://symfony.com/doc/3.4/components/phpunit_bridge.html)

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                     ☆Folder contain all plugins inside
│       │      ├── [plugin code]                       ☆Root folder of specification plugin
│       │      │     ├── Tests                         ★Folder contain all class about event
│       │      │     │    ├── [Web/...]                ★Folder contain testsuite
│       │      │     │    │    ├── [Name]Test.php      ★contain testcase    
│       │      │     │    ├── bootstrap.php            ★Setup information for phpUnit    
```

* Navigation

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                     ☆Folder contain all plugins inside
│       │      ├── [plugin code]                       ☆Root folder of specification plugin 
│       │      │     ├── Nav.php                       ★Specify position in menu 
```

```
class Nav implements EccubeNav
{
    public static function getNav()
    {
        return [
            'product' => [
                'id' => [unique name for menu],
                'name' => [Menu name],
                'url' => [route path]
            ]
        ];
    }
}
```

# E. How to migrate a Plugin 3.0.x to 3.n ?

## I. Apply new techniques
 - [Annotation](https://symfony.com/doc/current/bundles/SensioFrameworkExtraBundle/index.html)
 - [Injection](https://symfony.com/doc/3.4/components/dependency_injection.html)
 - [Doctrine 2.5](https://symfony.com/doc/3.4/doctrine.html)
 
## II. Event

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── Event                       
│       │      │     │    ├── [Name].php             
│       │      │     ├── [Name]Event.php             ☆3.0 : handle event 
                                                    ★3.n : will register listeners- handle for events
│       │      │     ├── event.yml                   ☆3.0 : will register listeners for events 
                                                    ★3.n : Removed
```

* Some events kind were defined in EC-CUBE[ will be confirm ]
    - System   : init & finish  
    - Route    : init  
    - Controler: request & response
    - Form     : form extension
    - Template : render
    - Specify  :
    
## III. ServiceProvider

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── ServiceProvider                       
│       │      │     │    ├── [plugin code]ServiceProvider.php       ☆3.0: Route & initialize components   
                                                                    ★3.n: removed ( instead : injection & annotation )    
```

## IV. Database
 - Schema
 
 ```
 EC-CUBE3n Root Directory
 ├──    app
 │       ├── Plugin                                   
 │       │      ├── [plugin code]                     
 │       │      │     ├── Resource                       
 │       │      │     │    ├── doctrine                       
 │       │      │     │    │    ├── Plugin.[plugin code].Entity.[Name].dcm.yml       ☆3.0: for install & delete
                                                                                    ★3.n: removed   
 ```

 - Migration
 
 ```
 EC-CUBE3n Root Directory
 ├──    app
 │       ├── Plugin                                   
 │       │      ├── [plugin code]                     
 │       │      │     ├── Resource                       
 │       │      │     │    ├── doctrine                       
 │       │      │     │    │    ├── migration                       
 │       │      │     │    │    │    ├── Version[date].php       ☆3.0: for install & update    
                                                                ★3.n: removed    
 ```

## V. Navigation

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── ServiceProvider                       
│       │      │     │    ├── [plugin code]ServiceProvider.php       ☆3.0: Define menu in function {register}
                                                                    ★3.n: removed ( instead : Nav.php )    
```