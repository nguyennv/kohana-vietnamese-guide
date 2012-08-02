#### `Jelly_Field_Boolean`

Đại diện cho một boolean. Trong cơ sở dữ liệu, nó thường được đại diện bằng một `tinyint`.

 * **`true`** — Cái mà lưu `TRUE` như trong cơ sở dữ liệu. Cái này mặc định là 1, nhưng bạn có thể muốn có các giá trị `TRUE` được lưu như 'Yes', hoặc 'TRUE'.
 * **`false`** - Cái mà lưu `FALSE` như trong cơ sở dữ liệu.

[!!] Một ngoại lệ sẽ được ném nếu bạn cố gắng thiết lập `convert_empty` tới `TRUE` trên trường này.

[Tài liệu API](../api/Jelly_Field_Boolean)