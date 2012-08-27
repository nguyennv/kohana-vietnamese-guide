# Cách Userguide làm việc

Userguide sử dụng [Markdown](markdown) cho tài liệu.
Cả các trang hướng dẫn sử dụng, và trong chú tích mã cho bộ duyệt API được viết ở định dạng markdown.

## Các trang Userguide

Các trang Userguide trong các mô-đun chúng được áp dụng tới, trong `guide/<module>`.
Ví dụ, tài liệu cho Kohana trong `system/guide/kohana` và tài liệu cho orm trong `modules/orm/guide/orm`, cơ sở dữ liệu trong `modules/database/guide/database`, v.v...

Mỗi mô-đun có một trang index tại `guide/<module>/index.md`.

Mỗi trình đơn của mô-đun tại `guide/<module>/menu.md`.

Tất cả các trang khác ở trong `guide/<module>` và có thể được tổ chức trong các thư mục con và đặt tên mà bạn muốn.

Để biết thêm thông tin về cách làm cho mô-đun của bạn có các trang userguide, xem [Thêm mô-đun của bạn](adding).

### Hình ảnh

Bất kì hành ảnh nào được sử dụng trong các trang userguide phải được đặt trong `media/guide/<module>/`.
Ví dụ, nếu một trang có `![Image Title](hello-world.jpg)` hình ảnh có thể được đặt tại `media/guide/<module>/hello-world.jpg`.
Những hình ảnh cho mô-đun ORM được ở trong `modules/orm/media/guide/orm`, và những hình ảnh cho tài liệu Kohana ở trong `system/media/guide/kohana`.

### Bộ duyệt API

Bộ duyệt API được sinh từ mã nguồn thực tế.
Các mô tả cho các lớp, các hằng, các thuộc tính, và các phương thức được chiết xuất từ chú thích và được phân tích trong Markdown.
Ví dụ nếu bạn nhìn vào một chú thích cho [Kohana_Core::init](http://github.com/kohana/core/blob/c443c44922ef13421f4a/classes/kohana/core.php#L5) bạn sẽ thấy một danh sách và bảng markdown.
Chúng được phân tích và hiện thị đúng trong bộ duyệt API.
`@param`, `@uses`, `@throws`, `@returns` và các thẻ khác được phân tích tốt.

TODO: cho biết thêm chi tiết cụ thể cách chú thích các lớp, các hằng, các phương thức của bạn, v.v. bao gồm việc đóng gói và cách nó quan hệ tới api của mô-đun.
