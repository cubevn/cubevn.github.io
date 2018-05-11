---
layout: default
title: Try transferring variable to screen
---

---

# {{ page.title }}

## TTwig structure and View variable

- In the previous chapter, I conducted Controller and creating/rendering Twig

- In oder to build WEB Application,you need to transfer info from Controller to View. In this chapter,just use the basic parts.

### Menu of this chapter

- In this chapter, conduct the following part

    1. Explain about defining View variable for Controller

    1. Explain how to transfer variable when rendering Controller

    1. Explain how to display varible that received from Controller by Twig

### Mofication Controller

- Define variable in Controller,give variable that defined already in method [render] as parameter by associative array

- In this time,「key」 becomes the optional character string,but when call variable in Twig side, it becomes the name

- Modify **CrudController.php** as below quickly

    - /src/Eccube/Controller/Tutorial/CrudController.php

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_3/CrudController_add_var.php"></script>

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
        $viewname = 'このビューは「Tutorial/crud_top.twig」が表示されています。';★追記

        return $app->render(
            'Tutorial/crud_top.twig',
            array(
                'viewname' => $viewname,
            )
        );★連想配列を追記
    }
}
```
-->

- Eplain the modification

    1. If transfer associative array for the second paramter of **render** method, you will be able to operate variable that transferred in **twig**. In this time, based on key of associative array to conduct operation of variable

    1. Key of associative array is optional.

### Modify Twig

- Add the display of variable that transferred by associative array in Controller, into Twig 

- Modify as below

	- Tutorial/crud_top.twig

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_3/crud_top_add_var.twig"></script>

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
｛％ endblock ％｝

｛％ block main ％｝
    <div class="row">
       <div class="col-sm-12">
            <div class="main_wrap">
                <h1>CRUDチュートリアル</h1>
                <p>投稿を行なってください</p>
                <dl>★追記
                    <dt>コントローラーから取得した変数です</dt>
                    <dd>｛｛ viewname ｝｝</dd>★変数呼び出し部
                </dl>
            </div>
        </div>
    </div>
｛％ endblock ％｝
```
-->

- Explain simply about the addition contents of this time

    1. Display variable that set in ｛｛｝｝Twig by Controller

    2. There are 3 kinds of block(1.の｛｛｝｝) of Twig

        ---

        | ブロック種別 | 適用対象 | 凡例 |
        |------|------|
        | ｛｛｝｝ | 変数の中身を表示 | ｛｛ viewstr\|nl2b ｝｝ |
        | ｛％％｝ | ブロック内でロジックを記述する | ｛％ if myvar is ... ％｝ |
        | {##} | コメントアウト | {# DB取得内容を表示... #} |

        ---

    3. Use separately 3 kinds above in order to structure View

### Confirm the display contents

- Try accessing into Browser to confirm finally

    1. Please input「http://[ドメイン + インストールディレクトリ]/tutorial/crud」in URL of browser

    1. Contents of variable that defined in Controller, is displayed.

---

![View変数のレンダリング](/images/img-tutorial3-view-rendar.png)

---

### Collect this chapter

- In this chapter, I studied the following part 

    1. I explained how to add View variable to render method in Controller

    1. I conducted the display of varible of Twig

    1. I explained kind of Block of Twig


### Global variable of View

- The following variable has been intialized by 「Application.php」, storing, so you can call directly from all Twigs

| 変数名 | 詳細情報 |
|------ |-----|
| BaseInfo | 管理画面 > 設定 > 基本情報設定 > ショップマスターで保存した内容 |
| title | ページタイトル |

- About the call method will make call as below.

例. ｛｛ BaseInfo.カラム名 ｝｝

- About the detail Twig structure, I will explain in next chapter

### Reference

<a href="http://qiita.com/poego/items/81628dcd0f8e4d4a2d9d" target="_blank">EC-CUBE 3のTwigのタグ覚え書き（一部EC-CUBE2系のSmarty比較）<a/>
