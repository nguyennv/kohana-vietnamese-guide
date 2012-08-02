#### `Jelly_Field_Timestamp`

Đại diện cho một dấu thời gian - timestamp.
Trường này luôn trả về giá trị của nó như một dấu thời gian UNIX, tuy nhiên bạn có thể chọn để lưu nó như bất kỳ kiểu giá trị nào bạn muốn bằng cách thiết lập thuộc tính `format`.

 * **`format`** — Mặc định, trường này sẽ được lưu như một dấu thời gian UNIX, tuy nhiên bạn có thể thiết lập trường này để bất kỳ định dạng `date()` hợp lệ và nó sẽ được chuyển đổi tới định dạng đó khi lưu.
 * **`auto_now_create`** — Nếu là TRUE, giá trị sẽ lưu `now()` bất cứ khi nào INSERT.
 * **`auto_now_update`** — Nếu là TRUE, trường sẽ lưu `now()` bất cứ khi nào UPDATE.

[Tài liệu API](../api/Jelly_Field_Timestamp)