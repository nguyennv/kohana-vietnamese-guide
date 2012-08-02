# ORM

Kohana 3.x bao gồm một mô-đun Ánh xạ Đối tượng Quan hệ - Object Relational Mapping (ORM) mà sử dụng mẫu bản ghi tích cực và sự tự xem xét cơ sở dữ liệu để xác định một thông tin cột của mô hình.
ORM được tích hợp chặt chẽ với thư viện [Validation].

ORM cho phép thao tác và điều khiển dữ liệu bên trong một cơ sở dữ liệu như thể nó là một đối tượng PHP.
Một khi bạn định nghĩa các quan hệ ORM cho phép bạn kéo dữ liệu từ cơ sở dữ liệu của bạn, thao tác dữ liệu trong bất kỳ cách nào bạn muốn, và sau đó lưu kết quả trở lại cơ sở dữ liệu mà không sử dụng SQL.
Bằng việc tạo các quan hệ giữa các mô hình mà theo quy ước qua việc cấu hình, nhiều sự lặp lại của của việc viết các truy vấn để tạo, đọc, cập nhật, và xoá thông tin từ cơ sở dữ liệu có thể được giảm thiểu hoặc được loại bỏ hoàn toàn.
Tất cả các quan hệ có thể được xử lý tự động bằng thư viện ORM và bạn có thể truy cập dữ liệu liên quan như các thuộc tính đối tượng chuẩn.

ORM được bao gồm với cài đặt Kohana 3.x nhưng cần phải được kích hoạt trước khi bạn có thể sử dụng nó.
Trong tập tin `application/bootstrap.php` của bạn sửa đổi lời gọi `Kohana::modules` và bao gồm các mô-đun ORM.

## Bắt đầu

Trước khi chúng ta sử dụng ORM, chúng ta phải kích hoạt các mô-đun yêu cầu

	Kohana::modules(array(
		...
		'database' => MODPATH.'database',
		'orm' => MODPATH.'orm',
		...
	));

[!!] Mô-đun cơ sở dữ liệu là bắt buộc để mô-đun ORM làm việc. Tất nhiên mô-đun cơ sở dữ liệu phải được cấu hình để sử dụng một cơ sở dữ liệu đang có.

Bây giờ bạn có thể tạo [các mô hình](models) của bạn và [sử dụng ORM](using).
