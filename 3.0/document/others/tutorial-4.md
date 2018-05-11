---
layout: default
title: Try displaying Form
---

---

# {{ page.title }}


## Form and Form builder

- In previous chapter, I gave simple example about Routing, rendering 

- In this chapter, I will create Form for inputting screen

- In EC-CUBE 3, creating Form will use Form builder to conduct.

### Menu of this chapter

- In this chapter, I will conduct the following part

    1. Use Form builder from Controller

    1. Build Form element by Form Builder

    1. Explain summary of Form Builder

    1. Explain about how to create Form View from Form builder, and how to transfer for Twig

    1. Explain the display method of Form View that accepted in Twig

## Add Form definition into Controller file

- First, define Form in Controller

- Conduct the following modifications in Controller

    - CrudController.php

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_4/CrudController_add_form.php"></script>

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
        //$viewname = 'このビューは「Tutorial/crud_top.twig」が表示されています。';★コメントアウトします。

        $builder = $app['form.factory']->createBuilder('form', null, array())★ここからフォーム定義を追加

        $builder->add(
            'reason',
            'choice',
            array(
                'label' => '投稿種別',
                'choices' => array('1' => '質問', '2' => '提案'),
                'required' => false,
                'mapped' => false,
                'expanded' => false,
                'multiple' => false,
            )
        )
        ->add(
            'title',
            'text',
            array(
                'label' => '投稿のタイトル',
                'required' => false,
                'mapped' => false,
            )
        )
        ->add(
            'notes',
            'textarea',
            array(
                'label' => '内容',
                'required' => false,
                'mapped' => false,
                'empty_data' => null,
                'attr' => array(
                    'style' => 'height:100px;',
                ),
            )
        );

        $forms = $builder->getForm();

        return $app->render(
            'Tutorial/crud_top.twig',
            array(
                //'viewname' => $viewname,★コメントアウトします。
                'forms' => $forms->createView(),★追記
            )
        );
    }
}
```
-->

### Add Item that used Form builder

- In Silex(Symfony2),define Form Item by **フォームビルダー** as the previous description

#### Method of adding Form Item

-  [フォームビルダー]->**add([name属性], [type属性], [オプション])**

#### Explain parameter

- Explain parameter of add method

    1. [name属性]
        - This is Item name of Form that is identified on html

    1. [type属性]
        - This is kind(text, checkbox, textarea...) of Form Item 

    1. [オプション] will set the following contents by associative array accordingly 

        1. Mandatory setting
          - Setting of Mandatory input of Item

        1. Default value
          - Initial value of value value

        1. **バリデーション**(Validation) (explain in following chapter)
          - Examine th einputting value

        1. Setting of value property
          - Ths input value of User (exclude hidden)

        1. All attributes that can specify with form in html
          - Can specify alls in html such as id, class, placeholder...

#### Mapping Entity with Form Item

- I think that the following part is set in Option Item above

```

'mapped' => false,

```

- The part above will set whether linking Form Item with Entity that appears in next chapters or not?
- In case not using Entity, please set **false**
- In case setting is **true**, it will be error due to Builder conducts mapping Item with Entity.


#### Special Items

- About the following Form Item, when create the normal html, general Ideas will be different

    1. Selectbox

    1. Checkbox

    1. Radiobox

    - Things that expressed above, will set the chosen value based on setting Item of Form Builder

        - In case above,you can execute by specifying [choices] for **addメソッド第二引数の[type属性]**(type attribute of the second parameter
of add method), give associative arrays.

#### Get Builder

- Form Builder can get in below

  - CAll the following method from **$app['form.factory']**

    - **createBuilder([タイプ名称], [エンティティ(データーモデルオブジェクト)], [オプション])**

      1. [タイプ名称] : Specify name that defined by **getName** of method 「FormType」which explains in next chapter.
        - I don't recommend, but when generate just Builder without FormType, please specify 「form」

      1. [エンティティ(データーモデルオブジェクト)] : Normally, set Entity that is used in Internal
        - When do not use Entity, please specify [null]

      1. [オプション] : Set Option when generating Form
        - Because this is not used frequently, so I will separate in this Tutorial

### Get View object of Form from Form Builder

- When finished definition of Item in Form Builder, conduct generating [フォームのビューオブジェクト(View Object of Form)] by the following order.


    1. Get Form
        - [フォームビルダー]->getForm();

    1. Get View Object of Form
        - [フォームオブジェクト]->createView()

## Display contents of Form View Object by Twig file

- Implementation of Controller side has finished.

- Next,add the following modification into Twig, in order to be able to display View object of Form as html

- Modify **crud_test.twig** as below

    - crud_test.twig


<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_4/crud_top_add_form.twig"></script>


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
                <dl>
                    <dt>コントローラーから取得した変数です</dt>
                    <dd>
                        ｛＃｛｛ viewname ｝｝＃｝★コメントアウト
                    </dd>
                </dl>
            </div>
           <div id="form-wrapper">★追記
               ｛｛ form_widget(forms._token) ｝｝
               ｛｛ form_widget(forms) ｝｝
           </div>
        </div>
    </div>
｛％ endblock ％｝
```
-->

- Here, the important rows are as below

    - ｛｛ form_widget(forms._token) ｝｝
    - ｛｛ form_widget(forms)｝｝

- Based on setting above, Form Object will be written out for View

    - The truth is that you can display each 1 Item, but in this Tutorial, I am writing all Items once.
    - **forms._token** will judge the thing that Accessed from this screen to server, so must describe.
    - Conduct part above for security
    - About Judgement of server side, Form Object will conduct automatically

### Confirm the display contents

- Try accessing into Browser to confirm finally

    1. Please input 「http://[ドメイン + インストールディレクトリ]/tutorial/crud」 into URL of Browser.

    1. Form that structured in Form Builder,is displayed.
---

![フォームのレンダリング](/images/img-tutorial4-view-rendar.png)

---


## Collect this chapter

1. I defined Form by Controller
1. I builded Form Item in Form Builder
1. I got Form Object from Form Builder, and got View Object from there
1. I displayed View Oject of Form by Twig
