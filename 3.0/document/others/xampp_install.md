---
layout: default
title: Xampp+Window
---

---

# Cài đặt EC-CUBE trên môi trường Xampp+Window

## Chuẩn bị ban đầu
- Download và cài đặt Xampp [apache download link](https://www.apachefriends.org/download.html)
- Giải nén EC-CUBE đơi tên thành eccube và bỏ vào `xampp\htdocs`. Sau khi bỏ vào ta sẽ có cấu trúc thư mục như dưới đây
```
C:\xampp\htdocs\eccube
```

## Thiết lập PHP

Tiến hành chỉnh sửa file php.ini(`C:\xampp\php\php.ini`)

- Timezone(Asia/Tokyo)

```
;date.timezone = Europe/Berlin
date.timezone = Asia/Tokyo
```

- Thiết lập module mở rộng (uncomment các dòng này)

```
extension=php_fileinfo.dll
extension=php_pdo_pgsql.dll
extension=php_pgsql.dll
extension=php_openssl.dll
```

Lưu lại các thay đổi của file php.ini khi hoàn thành chỉnh sửa.

## Khởi chạy xampp

Chạy xampp-control , bấm vào nút apache và mysql. Truy cập vào đường dẫn `http://localhost/xampp/`
nếu thấy homepage của xampp hiện ra là cài đặt thành công.

## Cài đặt EC-CUBE

- Setup Database

Do cần thiết phải tạo DB trước nên tiến hành tạo DB cho EC-CUBE bằng phpMyAdmin.
Truy cập vào đường dẫn `http://localhost/phpmyadmin/ `
Tiến hành tạo nội dung như sau

```
DB Name：eccube3_db
User Name：eccube3_db_user
Password:password
```

Tạo DB

```
DB Name：eccube3_db
Encode：utf8_general_ci
```

Tạo User

```
Thông tin user
  User name：eccube3_db_user
  Host:127.0.0.1
  Password:password
Quyền hạn
  Full quyền(Check toàn bộ)
```

- Cài đặt EC-CUBE
Truy cập vào link bên dưới

```
http://localhost/eccube-3.0.0/html/admin/install.php
```

 nhập các thông tin cần thiết vào và tiến hành theo các bước.

