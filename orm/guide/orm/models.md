# Tạo mô hình của bạn

Để tạo một mô hình cho bảng `members` trong cơ sở dữ liệu của bạn, hãy tạo tập tin `application/classes/model/member.php` với cú pháp sau:

	class Model_Member extends ORM
	{
		...
	}

(mục này nên cung cấp thêm ví dụ)

## Ghi đè tên bảng

Nếu bạn muốn thay đổi bảng cơ sở dữ liệu mà một mô hình sử dụng, chỉ cần ghi đè biến `$_table_name` như thế này:

	protected $_table_name = 'strange_tablename';

## Thay đổi khoá chính

ORM giả sử mỗi mô hình (và bảng cơ sở dữ liệu) có một cột `id` mà được đánh chỉ mục và là duy nhất.
Nếu cột khoá chính của bạn không được đặt tên `id`, điều đó tốt - chỉ cần ghi đè biến `$_primary_key` như thế này:

	protected $_primary_key = 'strange_pkey';

## Sử dụng cơ sở dữ liệu không mặc định

Đối với mỗi mô hình, bạn có thể định nghĩa cấu hình cơ sở dữ liệu nào mà ORM sẽ chạy các truy vấn trên nó.
Nếu bạn ghi đè biến `$_db_group`, ORM sẽ kết nối tới cơ sở dữ liệu đó. Ví dụ:

	protected $_db_group = 'alternate';
