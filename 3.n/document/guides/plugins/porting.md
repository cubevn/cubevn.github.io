# How to migrate a Plugin 3.0.x to 3.n ?

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