---
layout: default
title: PHP Storm Unittest
---

---


## Môi trường
Windows7

PHPStorm 10.0.3

## Mục tiêu
Có thể chạy PHPUnit từ PHPStorm chỉ bằng một nút bấm

![2016-03-15_16h46_06.png](https://qiita-image-store.s3.amazonaws.com/0/113212/14f0c4ae-ce78-e264-0ed8-0f0ef055112d.png)

## PHPStrom Unittest

### Mở EC-CUBE bằng PHPStorm

![2016-03-15_16h48_11.png](https://qiita-image-store.s3.amazonaws.com/0/113212/cd7abc46-159d-1854-fbe7-6057fdebaa87.png)

### Tiến hành setup PHP interpreter

![2016-03-15_16h49_35.png](https://qiita-image-store.s3.amazonaws.com/0/113212/8df58ca4-54b2-13b4-77c4-bfd70a937f0e.png)

### Thiết lập để có thể load PHPUnit từ autoload.php

Chỉ định đường dẫn `autoload.php`

![2016-03-15_16h51_22.png](https://qiita-image-store.s3.amazonaws.com/0/113212/7a421c7c-bccd-530f-392b-822aec590e8c.png)


##Tạo thiết lập PHPUnit

Mở thiết lập bằng cách truy cập Run>EditConfigurations... 。

![image](https://qiita-image-store.s3.amazonaws.com/0/113212/7671c441-eea2-8dc2-9d7f-6d5def530321.png)


[+] Bấm vào nút + và chọn `PHPUnit`

![image](https://qiita-image-store.s3.amazonaws.com/0/113212/2a53424b-ebdd-426e-2c87-1c26263d6719.png)


TestScope thì chọn là Directory sau đó nhập đường dẫn test vào.
※Bời vì nếu mà nhập toàn bộ thư mục Test của EC-CUBE vào thì sẽ rất mất thời gian nên hãy giới hạn phần mà bạn muốn
Test qua việc chỉ định đường dẫn đến thư mục Test tương ứng

![2016-03-15_17h01_42.png](https://qiita-image-store.s3.amazonaws.com/0/113212/64d453de-a0b4-f994-3d40-9465b37e7af6.png)

## Chạy Test

Tiến hành chạy unittest vừa thiết lập ở trên bằng cách click nút như hình
(Debug cũng chạy ok)

![2016-03-15_17h05_29.png](https://qiita-image-store.s3.amazonaws.com/0/113212/1775764d-8667-8cae-5e0e-39d0889b77f3.png)

## Console Test

Thực thi unittest trên console thì chạy câu lệnh sau
```
    vendor/bin/phpunit path_to_test_folder
```