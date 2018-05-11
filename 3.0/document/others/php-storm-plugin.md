---
layout: default
title: PHP Storm Plugin
---

---

Nếu cài đặt [silex-idea-plugin](https://github.com/Sorien/silex-idea-plugin) thi có thể truy cập đến các biến dạng như là `$app['orm.em']`


# Môi trường
PhpStorm ver 9.0

# silex-idea-plugin install
Settings＞Plugins＞「Browse repositories」, tìm kiếm「silex plugin」
Sau đó chọn và cài đặt nó

Nếu mà không kiếm thấy thì download theo link dưới đây

https://plugins.jetbrains.com/plugin/7809?pr=idea

Trong trường hợp đó thì dùng Settings＞Plugins＞「Install Plugin from disk

# Silex Pimple Dumper install
silex-idea-plugin dùng [silex-pimple-dumper](https://github.com/Sorien/silex-pimple-dumper) để sinh ra file pimple.json

Cài đặt bằng lệnh sau

`composer require sorien/silex-pimple-dumper:1.0.* --dev`

Sau đó chèn câu lênh sau vào `\html\index_dev.php` ngay sau dòng 53 `$app['debug'] = true;` sau khi chèn xong code nhìn
sẽ như sau

```
$app['debug'] = true;
$app->register(new \Sorien\Provider\PimpleDumpProvider());
```

sau đó access vào trang `http://localhost/eccube/html/index_dev.php/` sẽ thấy điều kì diệu xảy ra.

# Tham khảo

https://youtrack.jetbrains.com/issue/WI-17116

https://github.com/Sorien/silex-idea-plugin

https://github.com/Sorien/silex-pimple-dumper






