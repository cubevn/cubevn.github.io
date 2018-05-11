---
layout: default
title: Sample Plugin Tutorial
---

---

# Sample Plugin Tutorial

Bài viết này dành cho ai đã phát triển xong plugin GiftWrapping.
Tạo một plugin tên là LowStockAlert(dự kiến 1-2 ngày)

## Plugin Design

1.  Nếu số lượng sản phẩm trong kho bé hơn so với số lượng sản phẩm thiết lập thì hiện cảnh báo 「残りあとわずか！」
2.  Thiết lập số lượng sản phẩm thì trên màn hình admin/plugin setting. Số lượng sản phẩm thì có thể thay đổi tùy ý.
3.  Việc cài đặt số lượng sản phẩm thì áp dụng cho toàn hệ thống (không cần thiết đối với từng sản phẩm)

Màn hình hiển thị sản phẩm.

![PHP Version](https://raw.githubusercontent.com/lammn/markdown-images/master/plugin_sample.PNG)

Màn hinh plugin setting.

![PHP Version](https://raw.githubusercontent.com/lammn/markdown-images/master/plugin_sample1.PNG)

