# Loại trường Jelly

Jelly đi kèm với nhiều loại trường phổ biến được định nghĩa như các đối tượng với logic phù hợp để truy lục và định dạng chúng cho cơ sở dữ liệu.

### Các thuộc tính trường

Mỗi trường cho phép bạn truyền một mảng tới hàm dựng của nó để dễ dàng cấu hình nó. Tất cả tham số là tuỳ chọn.

#### Thuôc tính toàn cục

Những thuộc tính sau áp dụng cho gần như tất các các trường.

**`in_db`** — Có hoặc không trường đại diện cho một cột thực sự trong bảng.

**`default`** — Một giá trị mặc định cho trường.

**`allow_null`** — Có hoặc không các giá trị `NULL` có thể được thiết lập ở trên trường. Các giá trị mặc định là `TRUE` cho hầu hết các trường, ngoại trừ các trường dựa vào chuỗi và các trường quan hệ, trong trường hợp này nó mặc định là `FALSE`.

 * Nếu thuộc tính này là `FALSE`, hầu hết các trường sẽ được chuyển đổi `NULL` tới giá trị mặc định của trường.
 * Nếu thuộc tính này là `TRUE` giá trị default của trường sẽ được thay đổi là `NULL` (trừ khi bạn thiết lập giá trị mặc định cho mình).

**`convert_empty`** — Nếu thiết lập là `TRUE` bất kỳ giá trị `empty()` nào được truyền tới trường sẽ được chuyển đổi tới bất cứ cái gì được thiết lập cho `empty_value`. Thuộc tính này cũng thiết lập `allow_null` tới `TRUE` nếu `empty_value` là `NULL`.

**`empty_value`** — Đây là giá trị mà các giá trị `empty()` được chuyển đổi tới nếu `convert_empty` là TRUE. Mặc định cho thuộc tính này là `NULL`.

______________

#### các thuộc tính trường in_db

Các thuộc tính sau đây chủ yếu áp dụng tới các trượng mà thực sự đại diện cho một cột trong bảng.

**`column`** — Tên của cột cơ sở dữ liệu để sử dụng cho trường này. Nếu thuộc tính này không được đưa ra, tên trường sẽ được sử dụng.

**`primary`** — Có hoặc không trường là một khoá chính. Trường duy nhất mà có thiết lập này tới `TRUE` là `Jelly_Field_Primary`. Một mô hình có thể chỉ có một trường chính.

______________

#### Các thuộc tính xác nhận hợp lệ

Các thuộc tính sau đây có sẵn cho tất cả các loại trường và chủ yếu liên quan tới xác nhận hợp lệ. Có một cuộc thảo luận sâu sắc hơn của các thuộc tính này ở [tài liệu xác nhận hợp lệ](validation).

**`unique`** — Một thuộc tính lối tắt cho việc xác nhận mà dữ liệu của trường là duy nhất trong cơ sở dữ liệu.

**`label`** — Nhãn để sử dụng cho trường khi xác nhận.

**`filters`** — Các bộ lọc được áp dụng tới dữ liệu trước khi xác nhận nó.

**`rules`** — Các quy tắc được sử dụng để xác nhận dữ liệu.