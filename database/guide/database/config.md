# Cấu hình

Tập tin cấu hình mặc định nằm ở `MODPATH/database/config/database.php`.
Bạn nên sao chép tập tin này tới `APPPATH/config/database.php` và thực hiện các thay đổi ở đó, để phù hợp với [hệ thống tập tin phân cấp](../kohana/files).

Tập tin cấu hình cơ sở dữ liệu chứa một mảng của các nhóm câu hình.
Cấu trúc của mỗi nhóm cấu hình cơ sở dữ liệu, được gọi là một "instance", trong giống như thế này:

    string INSTANCE_NAME => array(
        'type'         => string DATABASE_TYPE,
        'connection'   => array CONNECTION_ARRAY,
        'table_prefix' => string TABLE_PREFIX,
        'charset'      => string CHARACTER_SET,
        'profiling'    => boolean QUERY_PROFILING,
    ),
	
Việc hiểu biết về mỗi thiết lập này là quan trọng.

INSTANCE_NAME
:  Các kết nối có thể được đặt tên bất kỳ thứ gì bạn muốn, nhưng bạn nên luôn có ít nhất một kết nối được gọi "default".

DATABASE_TYPE
:  Một trong các trình điều khiển cơ sở dữ liệu đã cài đặt. Kohana đi kèm với các trình điều khiển "mysql" và "pdo". Các trình điều khiển phải mở rộng từ lớp Database

CONNECTION_ARRAY
:  Các tuỳ chọn trình điều khiển cụ thể cho kết nối tới cơ sở dữ liệu của bạn. (Tuỳ chọn trình điều khiển được giải thích [bên dưới](#connection-settings).)

TABLE_PREFIX
:  Tiền tố mà sẽ được thêm vào tất cả tên bảng bằng [trình tạo truy vấn](#query_building). Câu lệnh được chuẩn bị sẽ **không** sử dụng tiền tố bảng.

QUERY_PROFILING
:  Kích hoạt [lập hồ sơ](../kohana/profiling) của các truy vấn cơ sở dữ liệu. Việc này rất hữu ích cho việc xem có bao nhiêu truy vấn ở mỗi trang đang sử dụng, và cái đang được dùng lâu nhất. Bạn phải bật trình tạo hộ sơ để xem các thống kê này.

## Ví dụ

Tập tin ví dụ dưới đây hiển thị hai kết nối MySQL, một cục bộ và một từ xa.

    return array
    (
        'default' => array
        (
            'type'       => 'mysql',
            'connection' => array(
                'hostname'   => 'localhost',
                'username'   => 'dbuser',
                'password'   => 'mypassword',
                'persistent' => FALSE,
                'database'   => 'my_db_name',
            ),
            'table_prefix' => '',
            'charset'      => 'utf8',
            'profiling'    => TRUE,
        ),
        'remote' => array(
            'type'       => 'mysql',
            'connection' => array(
                'hostname'   => '55.55.55.55',
                'username'   => 'remote_user',
                'password'   => 'mypassword',
                'persistent' => FALSE,
                'database'   => 'my_remote_db_name',
            ),
            'table_prefix' => '',
            'charset'      => 'utf8',
            'profiling'    => TRUE,
        ),
    );

## Kết nối và thể hiện

Mỗi nhóm cấu hình được tham chiếu tới một thể hiện cơ sở dữ liệu.
Mỗi thể hiện có thể được truy cập bằng cách gọi [Database::instance].
Nếu bạn không cung cấp một tham số, thể hiện mặc định sẽ được sử dụng.

	// Lệnh này sẽ kết nối tới cơ sử dữ liệu đã định nghĩa là 'default'
    $default = Database::instance();
	
	// Lệnh này sẽ kết nối tới cơ sử dữ liệu đã định nghĩa là 'remote'
    $remote  = Database::instance('remote');

Để ngắt kết nối cơ sở dữ liệu, chỉ cần huỷ bỏ đối tượng:

    unset($default)
	
	// Hoặc
	
	unset(Database::$instances['default']);

Nếu bạn muốn ngắt kết nối tất cả thể hiện cơ sở dữ liệu cùng một lúc:

    Database::$instances = array();

## Thiết lập kết nối

Mọi trình điều khiển cơ sở dữ liệu có các thiết lập kết nối khác nhau.

### MySQL

Một [cơ sở dữ liệu MySQL](http://www.php.net/manual/en/book.mysql.php) có thể chấp nhật các tuỳ chọn sau trong mảng `connection`:

Kiểu      | Tuỳ chọn   |  Mô tả                     | Giá trị mặc định
----------|------------|----------------------------| -------------------------
`string`  | hostname   | Tên máy cơ sở dữ liệu      | `localhost`
`integer` | port       | Số cổng                    | `NULL`
`string`  | socket     | Ổ cắm UNIX                 | `NULL`
`string`  | username   | Tên người dùng             | `NULL`
`string`  | password   | Mật khẩu                   | `NULL`
`boolean` | persistent | Kết nối liên tục           | `FALSE`
`string`  | database   | Tên cơ sở dữ liệu          | `kohana`

### PDO

Một [cơ sở dữ liệu PDO](http://php.net/manual/en/book.pdo.php) có thể chấp nhật các tuỳ chọn này trong mảng `connection`:

Kiểu      | Tuỳ chọn   |  Mô tả                      | Giá trị mặc định
----------|------------|-----------------------------| -------------------------
`string`  | dsn        | Định danh nguồn dữ liệu PDO | `localhost`
`string`  | username   | Tên người dùng              | `NULL`
`string`  | password   | Mật khẩu                    | `NULL`
`boolean` | persistent | Kết nối liên tục            | `FALSE`

[!!] Nếu bạn đang sử dụng PDO và không chắc chắn cái mà để sử dụng cho tuỳ chọn `dsn`, hãy xem lại [PDO::__construct](http://php.net/pdo.construct).
