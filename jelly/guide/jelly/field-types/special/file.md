#### `Jelly_Field_File`

Đại diện một tập tin tải lên.
Hãy truyền một tập tin tải lên hợp lệ tới trường này và nó sẽ lưu tự động trong vị trí bạn chỉ định.

Trong cơ sở dữ liệu, tên tập tin được lưu, cái mà bạn có thể sử dụng trong logic ứng dụng của bạn.

Để làm cho trường này là bắt buộc, hãy sử dụng quy tắc `Upload::not_empty` thay vì `not_empty` đơn giản.

Bạn phải cẩn thận để không truyền `NULL` hoặc vài giá trị khác tới trường này nếu bạn không muốn tên tập tin hiện thời bị ghi đè.

 * **`path`** — Thuộc tính này phải trỏ để một thư mục hợp lệ, có thể ghi được để lưu tập tin vào.
 * **`delete_file`** — Nếu giá trị là TRUE, tập tin sẽ được tự động bị xóa khi xóa. Mặc định là `FALSE`.
 * **`delete_old_file`** — Có hoặc không xoá tập tin cũ khi một tập tin mới được tải thành công. Mặc định là `FALSE`.
 * **`types`** — Các phần mở rộng tập tin hợp lệ mà tập tin có thể có.

[Tài liệu API](../api/Jelly_Field_File)