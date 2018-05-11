---
layout: default
title: テンプレートの探索順序
---

---

# Template searching order   

## Overview

In EC-CUBE, when searching file Design template, we search file passed `render()` in several folders one by one and after finding out a suitable template file we use it.
However, though intervention to Design template file by Plugin is allowed, there is still not completed overwriting.

### Searching order

* Front
  * The default template_code is `default`

```
  1. app/template/[template_code]
  2. src/Eccube/Resource/template/default
  3. app/Plugin/[plugin_code]/Resource/template/[template_code]
```

* Management screen
  * template_code of management screen is `admin`

```
1. app/template/admin
2. src/Eccube/Resource/template/admin
3. app/plugin/[plugin_code]/Resource/template/admin/[template_admin]
```

### Searching example

* Example for Front

Use Design template names [MyDesign] and it becomes `$app['view']->render('TemplateDir/template_name.twig');` by Controller.

```
 1. app/template/MyDesign/TemplateDir/template_name.twig
 2. src/Eccube/Resource/template/default/TemplateDir/template_name.twig
 3. app/Plugin/[plugin_code]/Resource/template/TemplateDir/template_name.twig
```

* Example for Management screen

Make it become  `$app['view']->render('Product/index.twig');`, customize template of Product master and put into app/ .

```
 1. app/template/admin/product/index.twig
 2. src/Eccube/Resource/template/admin/product/index.twig
 3. app/Plugin/[plugin_code]/Resource/template/admin/product/index.twig
```

## Behavior of editing on Management screen (block editing and page details)

Loading the template being used at present. If it doesn't have, bring it (file under src / Eccube / Resource / template / default /) and newly save it under app / template / default / .

### Image of behavior when loading

```
if (app/template/[template_code]/block/category.tpl) {
    read(app/template/[template_code]/block/category.tpl);
} else {
    read(src/Eccube/Resource/template/default/block/category.tpl)
}
```

### Image of behavior when saving

save (app/template/[template_code]/block/category.tpl)

