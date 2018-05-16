# How to porting a Plugin 3.0.x to 3.n ?

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

## VI. Technical Problems

1. Template : Confuse 
 - New way to adding or changing content in template: 

```
   "short block" : like this '{{ eccube_block_hello({'name':'EC-CUBE'}) }}'.
```

 - Listen event of Admin template:
 
```
    Admin/Product/product.twig  -> Admin/@admin/Product/product.twig
```

1. Event : 
 - Removed:
    - event: admin.order.delete.complete

1. Problems
    - Unable to retrieve the total order payment amount on the shopping index page.
    - The system still charges taxes on discounts.
    - The block.twig can not get the {http_cache} (esi) parameter.
    - Difficult to inject template (  edit inline, dont use id for element - xpath ).
    