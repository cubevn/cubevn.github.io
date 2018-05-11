---
layout: default
title: システム要件
---

---

# EC-CUBE 3.0.15 features list

## Front Features List

| No | Category | Feature Name | Feature Detail | Code Status
|---|--------------------------|-----------------------|-------|--------------|
|1 |Product Intro| Product List | Hiển thị các sản phẩm đã đang ký theo từng category |　〇
|2 | | Product Thumbnail | Hiển thị sản phẩm trên trang danh sách sản phẩm dưới dạng thumbnail |　〇
|3 | | Product Sort | Sắp xếp sản phẩm theo giá hoặc thời gian đăng ký |　〇
|4 | | Product Detail | Hiển thị chi tiết sản phẩm(Hình ảnh, mô tả sản phẩm, logo ...) |　〇
|5 | | Product Status | Sản phẩm đã bán hết hay chưa thì tự động hiển thị |　〇
|6 | | Add Favourite | Thêm sản phẩm vào danh sách yêu thích|　〇
|7 |Product Order| Shopping Cart | Có tính năng cơ bản của giỏ hàng như là chọn được nhiều sản phẩm |　〇
|8 | | Guest Shopping | Dù không phải member của trang vẫn có thể mua hàng được|　〇
|9 | | Multi Shipping | Nếu là member trang thi người dùng có thể chọn nhiều địa chỉ ship|　〇
|10 | | Time Shipping | Có thể chỉ định thời gian giao hàng|　〇
|11 | | Select Payment | Có thể chọn phương thức thanh toán|　〇
|12 | | Shopping Estimate | Hiển thị tổng tiền đơn hàng để confirm trước khi order |　〇
|13 | | Order Handle | Đồng thời với việc xử lý order thì gửi mail cảm ơn đến khách hàng , cùng với mail thông báo đến admin |　〇
|14 | My Page | Add Member | Tính năng thêm member mới |　〇
|15 |  | Login | Màn hình login |　〇
|16 |  | Edit Member Info | Chỉnh sửa thông tin member |　〇
|17 |  | Order History | Lịch sử đặt hàng |　〇
|18 |  | Order Confirm | Confirm đơn hàng trước khi đặt mua |　〇
|19 |  | Multi Shipping Edit | Có thể thêm, sửa ,xóa  địa chỉ ship |　〇
|20 |  | Member Leave | Member có thể leave khỏi trang |　〇
|21 | Another | Password Reset | Trong trường hợp người dùng quên pass thì có thể lấy lại password mới bằng mail đã nhập khi đăng ký |　〇
|22 |  | Notification | Thông báo tin tức mới của trang |　〇
|23 |  | Product Category Search | Tìm kiếm theo category của sản phẩm |　〇
|24 |  | Product Keyword Search | Tìm kiếm theo keyword(tên, brand...) của sản phẩm |　〇
|25 |  | Contact Form | Gửi message cho admin |　〇


## Admin Features List

| No | Category |Feature Name |Feature Detail |Code Status
|---|-----------------------|-------------------|-------|------------|
|1 | Auth Feature | Admin Login | Màn hình đăng nhập admin |　〇
|2 | | Add Admin | Có thể thêm admin  |　〇
|3 | Top Page| Order Status | Tình trạng đơn hàng hiện tại sẽ hiển thị trên Top page  |　〇
|4 | | Revenue Status | Tình trạng doanh thu hiện tại, só member sẽ hiển thị trân Top page  |　〇
|5 | | Shop Status | Tình trạng kho hàng hiện tại, só member sẽ hiển thị trân Top page  |　〇
|6 | Product | Product Search・List | Tìm kiếm, hiển thị kết quả dưới dạng list  |　〇
|7 | | Add・Edit Product | Có thể thêm mới product, chỉnh sửa thông tin product  |　〇
|8 | | Product Images | Có thể upload ảnh của product  |　〇
|9 | | Product Stock  | Có thể add số lượng sản phẩm còn trong kho  |　〇
|10 | | Product CSV Export | Export products ra CSV  |　〇
|11 | | Product Class | Có thể nhập chủng loại, màu sắc ....của  sản phẩm  |　〇
|12 | | Category | Add, Edit category  |　〇
|13 | | Category CSV Export| Export Category CSV  |　〇
|14 | | Product CSV Import| Import product by CSV |　〇
|15 | | Category CSV Import| Import category by CSV |　〇
|17 | | Sale Limit| Giới hạn số lượng có thể mua trong một lần  của một sản phẩm |　〇
|18 | | Multi Category| Một sản phẩm có thể thuộc nhiều category |　〇
|19 | | Product Search Item Add| Thêm item vào trong  Product Keyword Search|　〇
|20 | Order | Order Search・List| Tìm kiếm, hiển thị kết quả dưới dạng list|　〇
|21 |  | Order CSV Export| Export order by CSV|　〇
|22 |  | Shipping CSV Export| Export shipping by CSV|　〇
|23 |  | Input New Order| Thêm một order mới|　〇
|24 |  | Edit Order| Chỉnh sửa thông tin order|　〇
|25 |  | Order Status| Thiết lập trạng thái đơn hàng|　〇
|26 |  | Email| Có thẻ gửi mail thông báo cho khách hàng(như là sản phẩm đã được xuất đi rồi)|　〇
|27 | Member | Search・List | Tìm kiếm, hiển thị kết quả dưới dạng list |　〇
|28 | | Member CSV Export | Export member info by CSV|　〇
|29 | | Add・Edit Member | Thêm sửa member|　〇
|30 | | Finish Member | Mail thông báo chuyển từ member tạm thời thành member chính thức|　〇
|31 | Context | Notification | Quản lý thông báo tới khách hàng của hệ thống|　〇
|32 | | File Handle | Tiến hành download, upload files.|　〇
|33 | | SEO | Có thể thiết lập metadata của từng trang |　〇
|34 | | Layout | Quản lý layout của Top Page, Product Page... |　〇
|35 | | Block | Có thể lưu block hiển thị  |　〇
|36 | | Header・Footer | Chỉnh sửa header, footer của toàn trang |　〇
|37 | Basic Info Setting| Shop Master | Chỉnh sửa thiết lập thông tin công ty cần thiết để vận hành trang |　〇
|38 | | Product law | Chỉnh sửa những quy định của pháp luật liên quan đến một sản phẩm đặc biệt nào đó|　〇
|39 | | Member Agreement Setting | Chỉnh sửa quy đinh trang dành cho member |　〇
|40 | | Payment Setting | Thiết lập  phương thức thanh toán, hoa hồng  |　〇
|41 | | Payment use condition | Thiết lập giới hạn cận trên dưới của số tiền thanh toán  |　〇
|42 | | Shipping Free Condition | Thiết lập điều kiện ship free  |　〇
|43 | | Shipping Setup | Thiết lập công ty vận chuyển, ngày giờ vận chuyển...  |　〇
|44 | | Tax Setup | Thiết lập thuế tiêu dùng |　〇
|45 | | Mail Template | Thiết lập mail template |　〇
|46 | | CSV Export | Thiết lập các mục sẽ export khi xuất CSV |　〇
|47 | | Site Setup| Thiết lập xem tính nắng có sử dụng hay không |　〇
|48 | Owner Store | Plugin List | Danh sách plugin đã cài đặt |　〇
|49 |  | Plugin Install | Cài đặt mới plugin |　〇
|50 |  | Plugin Handle | Thiết lập đối với những plugin đã cài |　〇
|51 |  | Template| Có thể sử dụng template đã chuẩn bị trước bằng nút bấm |　〇
|52 | System | System Info| Hiển thị cấu hình server, DB, OS... |　〇
|53 |  | Member Control| Quản lý member có thể login vào màn hình quản lý |　〇
|54 |  | Security Control| Giới hạn IP truy cập màn hình admin... |　〇
|54 | Another | Update Control| Có khả năng update phiên bản mới |　〇

























