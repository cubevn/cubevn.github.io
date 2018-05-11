---
layout: default
title: PHP Storm Debug
---

---


## Môi trường

Windows7

PHPStorm 10.0.3

Xampp

## Mục tiêu
Có thể chạy debug từ PHPStorm chỉ bằng một nút bấm

## PHPStorm Debug

1.  Enable Xdebug
    Để enable Xdebug thì làm theo hướng dẫn như link sau  [enable xdebug](http://www.wikihow.com/Configure-XDebug-in-XAMPP-(1.7.2/Later)-on-Windows)

2.  PHPStorm debug

    PHP Storm `Run > EditConfigurations`

    ![run config](https://raw.githubusercontent.com/lammn/markdown-images/master/phpstorm_xdebug.PNG)

    Click vào 2

    ![xdebug config](https://raw.githubusercontent.com/lammn/markdown-images/master/xdebug.PNG)

    Sau khi đã config xong xdebug thì bắt đầu debug thử

    `Run > Debug` chọn eccube , sau đó đánh dấu `breakpoint` rồi debug thôi(dễ mà)

    Các Step debug

     ![xdebug step](https://raw.githubusercontent.com/lammn/markdown-images/master/run_xdebug.PNG)

     F8 : Step Over

     F7 : Step Into (đi vào trong hàm gọi nó)

     F9 : Resume Program

     Hay dùng nhiều F7 và F9.