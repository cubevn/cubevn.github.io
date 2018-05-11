---
layout: default
title: Setting of URL
---

---

# {{ page.title }}


## Routing and controller Providers

- Firstly, I would like to explain about setting method of basic routing

### This is basic thingking method about each setting of EC-CUBE 3

- Setting of EC-CUBE is very simple. By describing the setting contents to setting file basically, we will build Application
- In setting file, [.php .yml] has been using.

### Menu of this chapter

- In this chapter, we will conduct the following part

    1. This is explanation about the setting file when linking URL and Controller

    1. Set routing
        - Explain linking method between Controller and URL.

### This is explanation about the setting file when linking URL and Controller

- File(routing)that sets linking URL and controller,has been setting in file of **ControllerProvider** 

#### Directory which saves Controller Provider

- Controller Provider is saving in the following Directory

```
/[インストールディレクトリ]/src/Eccube/ControllerProvider
```

#### Kind of Controller Provider file

- The following file has been saving in Directory

    1. AdminControllerProvider.php
        - Routing of 管理画面(management screen) is displayed.

    1. FrontControllerProvider.php
        - Routing of ユーザー画面(フロント画面)(User screen (Front screen) is displayed.

    1. InstallControllerProvider.php
        - Routing of インストール画面(Install screen) is displayed.
        - In customize, **本設定ファイルを使用することはありません。**(this setting file will not be used)

### Setting of Routing

#### Setting of FrontControllerProvider

- Setting of Routing of ユーザー画面(User screen) is executed in **FrontControllerProvider**, so please open the corresponding file from place where explained in the previous Item

#### Content of **FrontControllerProvider**

- After open file,set Item that is easy to understand,to be example,and first try searching [mypage]

-Extract setting of routing of mypage into the follwing part


<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_1/FrontControllerProvider_mypage.php"></script>

<!--
```
    // mypage
    $c->match('/mypage', '\Eccube\Controller\Mypage\MypageController::index')->bind('mypage');
    $c->match('/mypage/login', '\Eccube\Controller\Mypage\MypageController::login')->bind('mypage_login');
    $c->match('/mypage/change', '\Eccube\Controller\Mypage\ChangeController::index')->bind('mypage_change');
    $c->match('/mypage/change_complete', '\Eccube\Controller\Mypage\ChangeController::complete')->bind('mypage_change_complete');

```
-->

#### Explanation about method

- If describe parameter part,it will be as below

```
$c->match([ドキュメントルートからのurl], [紐付けるコントローラークラス・メソッド])->bind([ルーティング名称])
```

1. URL from Document route
    - Begin with /(slash), create the optional URL name.
        - Process in page from name, will name that easy to guess

1. the linking Controller class/method
    - Specify file name (class name)) and method name of Controller which created in /src/Eccube/Controller by full path from Install Directory
    - Between class name and method name will connect by 2 colon [::]

1. Routing name
    - Put 「名前」(name)]  in the setted routing name
    - Use 「名前」(name)] when re-direct

#### The actual description contents

1. Add source to **FrontControllerProvider**

- Add in before「return」of the lowest part of file as below

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_1/FrontControllerProvider_add_source.php"></script>


<!--
```
        // チュートリアル
        $c->match('/tutorial/crud', '\Eccube\Controller\Tutorial\CrudController::index')->bind('tutorial_crud');

        return $c;
    }
}
```
-->

- With operating above,setting of Routing has finished.

### Access by Browse

- Setting of Routing finshed already, so we try accessing by browser

    1.Please input「http://[ドメイン + インストールディレクトリ]/tutorial/crud」in URL of browser

    1. Display error message

- If the following message is displayed, you succeeded.
- Content of error is error that can not find Controller which is specifying by routing.
- Currently, because we have not creating Controller, so this is right action.
- Controler and method will create in next chapter.

---

![tutorial1-error1](/images/img-tutorial1-error1.png)

---
