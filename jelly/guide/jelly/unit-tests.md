# Kiểm thử đơn vị

Các kiểm thử đơn vị cho Jelly được bao gồm và chúng sử dụng mô-đun Kiểm thử đơn vị của Kohana.
Hãy chạy các kiểm thử này nếu bạn đóng góp bất kỳ mã để giảm thiểu nguy cơ của việc tạo ra các lỗi mới.

Nếu bạn có thêm tính năng mới cho Jelly (hoặc thay đổi một số hành vi), rất khuyến khích viết hoặc sửa đổi các kiểm thử cho nó.

Các kiểm thử có thể được chạy trên cơ sở dữ liệu MySQL, SQLite và PostgreSQL.

## Yêu cầu để chạy kiểm thử
Nếu bạn không tìm thấy các tập tin cấu hình đề cập dưới đây sao chép chúng từ thư mục tương ứng với các mô-đun của thư mục cấu hình ứng dụng của bạn.

### Thiết lập các hồ sơ cơ sở dữ liệu cho kiểm thử đơn vị
Tìm tập tin cấu hình kiểm thử đơn vị: *APPATH/config/unittest.php*.
Thiết lập `db_connection` tới hồ sơ cơ sở dữ liệu (tên cấu hình), bạn sẽ sử dụng cho kiểm thử đơn vị.

### Luôn luôn thiết lập tên cơ sở dữ liệu
Tìm hồ sơ cho kiểm thử đơn vị trong tập tin cấu hình CSDL của bạn: *APPATH/config/database.php*.
Bạn có để thiết lập tên cơ sở dữ liệu (`database`) trong mảng `connection` ngay cả khi sử dụng trình điều khiển PDO.

### Sử dụng tên chính xác cho loại cơ sở dữ liệu
Tìm hồ sơ cho kiểm thử đơn vị trong tập tin cấu hình CSDL của bạn: *APPATH/config/database.php*.
Loại cơ sở dữ liệu (`type`) là một trong những loại sau đây: **mysql**, **sqlite**, **postgresql**.

Nếu bạn có một trình điều khiển PDO tùy chỉnh với một tên khác nhau, ví dụ **pdo_sqlite** và bạn không muốn thay đổi nó, bạn sẽ phải đổi tên các tập tin lược đồ cơ sở dữ liệu cho cơ sở dữ liệu tương ứng (điều này được sử dụng để thiết lập các bảng cho kiểm thử).

Thực hiện tìm tập tin *MODPATH/jelly/tests/test_data/jelly/test-schema-**YOUR DATABASE TYPE**.sql* và đổi tên kiểu CSDL mặc định mà bạn muốn.

Sử dụng ví dụ, chúng ta cần đổi tên **test-schema-sqlite.sql** thành **test-schema-pdo_sqlite.sql**.

### Thiết lập nhận dạng cho SQLite {#sqlite_ident}

Khi sử dụng SQLite thông qua các trình điều khiển PDO nó là cần thiết để thiết lập các định danh trích dẫn trong tập tin cấu hình cơ sở dữ liệu của bạn.
Không làm điều này sẽ gây ra lỗi cú pháp SQL khác nhau.

	'unittest' => array
	(
		'type'       => 'pdo',
		'connection' => array(
			...
		),
		...
		'identifier' => '`',
	),

## Quy trình kiểm thử

Để có được một sự hiểu biết tốt hơn về cách kiểm thử làm việc đọc những điểm sau đây về những gì xảy ra trước khi mỗi nhóm kiểm thử được chạy:

1. các bảng được tạo ra bằng cách sử dụng các tập tin lược đồ cơ sở dữ liệu từ *MODPATH/jelly/tests/test_data/jelly/*
2. kết nối được thiết lập tới cơ sở dữ liệu
3. dữ liệu được đưa vào cơ sở dữ liệu từ *MODPATH/jelly/tests/test_data/jelly/test.xml*
4. các kiểm thử được chạy

## Sử lý sự cố

### Các lỗi cú pháp SQL

Nếu bạn đang sử dụng PDO và SQLite, xin vui lòng kiểm tra xem bạn đã [thiết lập các định danh trích dẫn](#sqlite_ident).
