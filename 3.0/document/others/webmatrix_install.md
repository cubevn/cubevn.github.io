---
layout: default
title: WebMatrix+Window
---

---

# Cài đặt EC-CUBE trên môi trường WebMatrix+Window

## Chuẩn bị ban đầu

- Download và cài đặt WebMatrix `https://www.microsoft.com/web/downloads/platform.aspx`
- Cài đặt PHP

 Chọn phiên bản PHP mà bạn muốn cài đặt (khong chọn bản  For IIS Express)

 ![PHP Version](https://raw.githubusercontent.com/lammn/markdown-images/master/WebInstall.PNG)

Kiểm tra cài đặt PHP bằng lệnh

 ```
php --version
 ```

## Thiết lập PHP

Tiến hành chỉnh sửa file php.ini

- Thêm dòng này vào
 ```
extension = php_fileinfo.dll
 ```
Lưu lại các thay đổi của file php.ini khi hoàn thành chỉnh sửa.

## Khởi chạy EC-CUBE

Giải nén EC-CUBE đổi tên thư mục thành eccube và di chuyển đến thư mục sau giải nén ví dụ `C:\temp\eccube`
và thực hiện lệnh sau
 ```
php -S 127.0.0.1:8080 –t html
 ```

## Cài đặt EC-CUBE

- Cài đặt EC-CUBE
Truy cập vào link bên dưới

```
http://127.0.0.1:8080 /
```

nếu mà màn hình install EC-CUBE hiện ra thì là OK.Tiến hành cài đặt theo các bước.

- Chỉ định DB SQLite

Khi đến màn hình chọn DB thì chọn SQlite



