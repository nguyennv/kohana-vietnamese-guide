#### `Jelly_Field_Enum`

Đại diện cho một danh sách liệt kê.
Hãy nhớ rằng trường này chấp nhận bất kỳ giá trị nào đã truyền tới nó, và nó không là cho đến khi bạn `validate()` mô hình mà bạn sẽ biết có hoặc không giá trị là hợp lệ hay không.

Nếu bạn `allow_null` trên trường này, `NULL` sẽ được thêm vào mảng lựa chọn nếu nó hiện thời không có trong đó.
Tương tự, nếu `NULL` ở trong mảng lựa chọn `allow_null` sẽ được thiết lập là `TRUE`.

 * **`choices`** — Một mảng các lựa chọn hợp lệ.

[Tài liệu API](../api/Jelly_Field_Enum)