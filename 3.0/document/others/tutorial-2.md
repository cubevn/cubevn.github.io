---
layout: default
title: Try displaying View from Controller
---

---

# {{ page.title }}


## Rendering of View

- Setting of Routing has finished in previous chapter

- In this chapter,try displaying View with the created Routing

### Menu of this chapter

- In this chapter,conduct the following part

    1. Explanation about creating Controller and Rending method of View

    1. Conduct explanation about creation and role of View (Twig)

### Create Controller

#### Create Folder

- First, please create the following folder

1. /src/Eccube/Controller/Tutorial
    - Relevant Controller will gather into one folder.
    - Creation method will be different in each
    - Please create Directory as below

---

![フォルダの作成](/images/img-tutorial2-make-dir.png)

---

#### Create file

- Next,create **CrudController.php**

- Copy TopController, re-name

- **CrudController.php**( contents will copy TopController.php)


<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_2/CrudController_before.php"></script>

<!--
```
<?php
/*
 * This file is part of EC-CUBE
 *
 * Copyright(c) 2000-2015 LOCKON CO.,LTD. All Rights Reserved.
 *
 * http://www.lockon.co.jp/
 *
 * This program is free software; you can redistribute it and/or
 * modify it under the terms of the GNU General Public License
 * as published by the Free Software Foundation; either version 2
 * of the License, or (at your option) any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with this program; if not, write to the Free Software
 * Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.
 */


namespace Eccube\Controller;

use Eccube\Application;

class TopController
{

    public function index(Application $app)
    {
        return $app->render('index.twig');
    }
}
```
-->

- Modify as below

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_2/CrudController_after.php"></script>

<!--
```
<?php
/*
 * This file is part of EC-CUBE
 *
 * Copyright(c) 2000-2015 LOCKON CO.,LTD. All Rights Reserved.
 *
 * http://www.lockon.co.jp/
 *
 * This program is free software; you can redistribute it and/or
 * modify it under the terms of the GNU General Public License
 * as published by the Free Software Foundation; either version 2
 * of the License, or (at your option) any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with this program; if not, write to the Free Software
 * Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.
 */


namespace Eccube\Controller\Tutorial; ★フォルダのパスを追加

use Eccube\Application;
use Eccube\Controller\AbstractController; ★親コントローラーのパスを追加

class CrudController extends AbstractController ★クラス名を修正 + 親コントローラーを継承
{

    public function index(Application $app)
    {
        echo 'First Tutorial';★追記
        exit();★追記
        //return $app->render('index.twig');★一旦コメントアウト
    }
}
```
-->

#### Confirm routing

- Try accessing into browser in order to check again

    1. Please input「http://[ドメイン + インストールディレクトリ]/tutorial/crud」

    1. Perhaps,next is not error, the following part will be displayed.

---

![エコーで文字表示](/images/img-tutorial2-echo-str.png)

---

- Perhaps, there is no problem in setting of routing and Controller. 

### Create view

- Add Twig file into the following folder

    1. /src/Eccube/Resource/template/default/Tutorial

        - Gather View of the relating Controller for each Folder.
        - The creating method will be different in each environment, so I will separate.
        - Please create Directory as below.

---

![ビューフォルダの作成](/images/img-tutorial2-make-dir.png)

---

#### Create file

- Next, create **crud_top.twig**

- Copy index.twig, and rename

- **crud_top.twig**( contents will copy index.twig)

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_2/crud_top_before.twig"></script>

<!--
```
｛＃
This file is part of EC-CUBE

Copyright(c) 2000-2015 LOCKON CO.,LTD. All Rights Reserved.

http://www.lockon.co.jp/

This program is free software; you can redistribute it and/or
modify it under the terms of the GNU General Public License
as published by the Free Software Foundation; either version 2
of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program; if not, write to the Free Software
Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.
＃｝

｛％ extends 'default_frame.twig' ％｝

｛％ set body_class = 'front_page' ％｝

｛％ block javascript ％｝
<script>
$(function(){
    $('.main_visual').slick({
        dots: true,
        arrows: false,
        autoplay: true,
        speed: 300
    });
});
</script>
｛％ endblock ％｝

｛％ block main ％｝
    <div class="row">
       <div class="col-sm-12">
            <div class="main_visual">
                <div class="item">
                  <img src="{{ app.config.front_urlpath }}/img/top/mv01.jpg">
                </div>
                <div class="item">
                  <img src="{{ app.config.front_urlpath }}/img/top/mv02.jpg">
                </div>
                <div class="item">
                  <img src="{{ app.config.front_urlpath }}/img/top/mv03.jpg">
                </div>
            </div>
        </div>
    </div>
｛％ endblock ％｝
```
-->

- Conduct modification as below

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_2/crud_top_after.twig"></script>

<!--
```
｛＃
This file is part of EC-CUBE

Copyright(c) 2000-2015 LOCKON CO.,LTD. All Rights Reserved.

http://www.lockon.co.jp/

This program is free software; you can redistribute it and/or
modify it under the terms of the GNU General Public License
as published by the Free Software Foundation; either version 2
of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program; if not, write to the Free Software
Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.
＃｝
｛％ extends 'default_frame.twig' ％｝

｛％ set body_class = 'front_page' ％｝

｛％ block javascript ％｝★<sctipt> ～ </script>を削除
｛％ endblock ％」

｛％ block main ％｝
    <div class="row">
       <div class="col-sm-12">
            <div class="main_wrap">★ID名称を変更「main_visual」→「main_wrap」、「main_visual」内を削除し新しく内容を追記
                <h1>CRUDチュートリアル</h1> ★追記
                <p>投稿を行なってください</p> ★追記
            </div>
        </div>
    </div>
｛％ endblock ％｝
```
-->

#### Modify Controller

- Modify the place where did 「echo」by controller in the following content

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_2/CrudController_modified.php"></script>

<!--
```
<?php
/*
 * This file is part of EC-CUBE
 *
 * Copyright(c) 2000-2015 LOCKON CO.,LTD. All Rights Reserved.
 *
 * http://www.lockon.co.jp/
 *
 * This program is free software; you can redistribute it and/or
 * modify it under the terms of the GNU General Public License
 * as published by the Free Software Foundation; either version 2
 * of the License, or (at your option) any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with this program; if not, write to the Free Software
 * Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.
 */


namespace Eccube\Controller\Tutorial;

use Eccube\Application;
use Eccube\Controller\AbstractController;

class CrudController extends AbstractController
{

    public function index(Application $app)
    {
        return $app->render('Tutorial/crud_top.twig');★修正箇所(コメント部と、echo、exitを削除)
    }
}
```
-->

-  I will explain simply about Controller and Method  

    1. Paramter : $app
        - In $app,class that is used in EC-CUBE, has been storing.
        - Correction is that content which is set in Application.php/ServiceProvider become structure, which is instanted, used when implementing.
        - Here,I don't explain in detail, but remember that you just call many functions from [$app] to structure Application
    1. Name space : use Eccube\Application;
        - In order to use [$app] that explained in 1., you have to specify name space out of scope of class
        - If say simply, I will tell you place for storing class that use for controller.
        - Path that specify in Name space will be different based on class which use, but in case use class that exist in [/src/Eccube], please specify relative path from [Eccube] (At the beginning, no need [\])
    1. Name space : use Eccube\Controller\AbstractControler;
        - I will set parent class of controller based on reason same above
    1. $app->render([表示したいTwigのパス])
        - If transfer path of Twig into [render] as parameter, Twig of target will be analyzed, and converted into html.
        - Normally,as return value of method of controller, if let return value of render as like that and [return],html that was converted, will be returned, screen is displayed.
        - About path that specify as [引数(parameter)], [/src/Eccube/Resource/template/] is setting as route path
        - Route path is setting when finished initialization of Application.php
        - In case of Controller of Admin side,[/admin/] of folder above becomes target, 
           In case of User screen, [/default/] becomes route folder.

#### Confirm the display content

- Try accessing in browser i

    1. Please input「http://[ドメイン + インストールディレクトリ] into URL of browser 

    1. Contents which was described in

        - Header and Footer has not displayed yet, but current status is exactly.
        - The display setting of Header and footer will conduct later.

---

![twigで文字表示](/images/img-tutorial2-view-rendar.png)

---

### Gather this chapter

- Contents volume had also increased, so I will gather content of chapter
- I conducted the following part in this chapter

1. Copy the existing Controller to create new controller
1. Copy the existing Twig to create new Twig
1. I explained the thing that based on Controller /Twig to gather into the relating folder.
1. I explained that $app is transfered as parameter of method of controller,many functions have been stored.
1. I explained that by converting Twig into html by render,set to be returned value of method, screen will be drawn.
