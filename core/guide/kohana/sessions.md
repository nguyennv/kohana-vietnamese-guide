# Phiên làm việc

Kohana cung cấp các lớp mà làm cho nó dễ dàng làm việc với cả các cookie và phiên làm việc.
Ở mức cao cả hai phiên làm việc và cookie cung cấp các chức năng tương tự.
Chúng cho phép các nhà phát triển cho lưu trữ thông tin tạm thời hay kiên gan về một trình khách cụ thể cho việc lấy lại về sau, thường là làm cho một vài kiên gan giữa các yêu cầu.

Phiên làm việc nên được sử dụng để lưu trữ dữ liệu tạm thời hoặc riêng tư.
Dữ liệu rất nhảy cạm cần được lưu trữ bằng cách sử dụng lớp [Session] với bộ điều hợp "database" hoặc "native".
Khi sử dụng bộ điều hợp "cookie", phiên làm việc phải luôn luôn được mã hoá.

[!!] Để biết thêm thống tin về cách thực hành tốt nhất với các biến phiên làm việc xem [bảy tội lỗi chết người của phiên làm việc](http://lists.nyphp.org/pipermail/talk/2006-December/020358.html).

## Lưu trữ, truy lục, và xóa dữ liệu

[Cookie] và [Session] cung cấp một API rất giống để lưu trữ dữ liệu.
Sự khác biệt chính giữa chúng là các phiên làm việc được truy cập bằng cách sử dụng một đối tượng, và các cookie được truy cập bằng cách sử dụng một lớp tĩnh.

Việc truy cập thể hiện phiên làm việc được thực hiện bằng cách sử dụng phương thức [Session::instance]:

    // Lấy thể hiện phiên làm việc
    $session = Session::instance();

Khi sử dụng phiên làm việc, bạn có thể lấy tất cả dữ liệu phiên làm việc hiện thời bằng cách sử dụng phương thức [Session::as_array]:

    // Lấy tất cả dữ liệu phiên làm việc như một mảng
    $data = $session->as_array();

Bạn cũng có thể sử dụng cách này để quá tải biến toàn cục `$_SESSION` để lấy và thiết lập dữ liệu trong một cách tương tự như PHP chuẩn:

    // Quá tải $_SESSION với dữ liệu phiên làm việc
    $_SESSION =& $session->as_array();
    
    // Thiết lập dữ liệu phiên làm việc
    $_SESSION[$key] = $value;

### Lưu trữ dữ liệu

Việc lưu trữ dữ liệu phiên làm việc hoặc cookie được thực hiện bằng việc sử dụng phương thức `set`:

    // Thiết lập dữ liệu phiên làm việc
    $session->set($key, $value);
	// Hoặc
	Session::instance()->set($key, $value);

    // Lưu một id người dùng
    $session->set('user_id', 10);

### Truy lục dữ liệu

Getting session or cookie data is done using the `get` method:

    // Get session data
    $data = $session->get($key, $default_value);

    // Get the user id
    $user = $session->get('user_id');

### Xóa dữ liệu

Việc xoá dữ liệu phiên làm việc hoặc cookie được thực hiện bằng việc sử dụng phương thức `delete`:

    // Xoá dữ liệu phiên làm việc
    $session->delete($key);


    // Xoá id người dùng
    $session->delete('user_id');

## Cấu hình phiên làm việc

Luôn luôn kiểm tra các thiết lập này trước khi đưa ứng dụng của bạn sống, như nhiều trong số chúng có thể ảnh hưởng trực tiếp tới bảo mật của ứng dụng của bạn.

## Bộ điều hợp phiên làm việc

Khi tạo hoặc truy cập vào một thể hiện của lớp [Session] bạn có thể quyết định bộ điều hợp hoặc trình điều khiển phiên làm việc mà bạn muốn sử dụng.
Các bộ điều hợp phiên làm việc có sẵn cho bạn là:

Native
: Lưu trữ dữ liệu phiên làm việc trong vị trí mặc định của máy chủ web của bạn. Vị trí lưu trữ được định nghĩa bằng [session.save_path](http://php.net/manual/session.configuration.php#ini.session.save-path) trong `php.ini` hoặc được định nghĩa bởi [ini_set](http://php.net/ini_set).

Database
: Lưu trữ dữ liệu phiên làm việc trong một bảng cơ sở dữ liệu bằng cách sử dụng lớp [Session_Database]. Yêu cầu mô-đun [Database] được bật.

Cookie
: Lưu trữ dữ liệu phiên làm việc trong một cookie bằng cách sử dụng lớp [Cookie]. **Phiên làm việc sẽ có một giới hạn 4KB khi sử dụng bộ điều hợp này, và phải được mã hoá**.

Bộ điều hợp mặc định có thể được thiết lập bằng cách thay đổi giá trị của [Session::$default]. Bộ điều hợp mặc định là "native".

Để truy cập một Phiên làm việc sử dụng bộ điều hợp mặc định, đơn giản chỉ gọi [Session::instance()].
Để truy cập một Phiên làm việc sử dụng vài thứ khác so với mặc định, truyền tên bộ điều hợp tới `instance()`, ví dụ: Session::instance('cookie')


### Thiết lập bộ điều hợp phiên làm việc

Bạn có thể áp dụng thiết lập cấu hình tới mỗi bộ điều hợp phiên làm việc bằng cách tạo một tập tin cấu hình phiên làm việc tại `APPPATH/config/session.php`.
Tập tin cấu hình mẫu sau đây định nghĩa tất cả thiết lập cho mỗi bộ điều hợp:

[!!] Cũng như với các cookie, một thiết lập "lifetime" là "0" có nghĩa là phiên làm việc sẽ hết hạn khi trình duyệt bị đóng.

    return array(
        'native' => array(
            'name' => 'session_name',
            'lifetime' => 43200,
        ),
        'cookie' => array(
            'name' => 'cookie_name',
            'encrypted' => TRUE,
            'lifetime' => 43200,
        ),
        'database' => array(
            'name' => 'cookie_name',
            'encrypted' => TRUE,
            'lifetime' => 43200,
            'group' => 'default',
            'table' => 'table_name',
            'columns' => array(
                'session_id'  => 'session_id',
        		'last_active' => 'last_active',
        		'contents'    => 'contents'
            ),
            'gc' => 500,
        ),
    );

#### Bộ điều hợp native

Kiểu      | Thiết lập | Mô tả                                             | Mặc định
----------|-----------|---------------------------------------------------|-----------
`string`  | name      | name of the session                               | `"session"`
`integer` | lifetime  | number of seconds the session should live for     | `0`

#### Bộ điều hợp cookie

Kiểu      | Thiết lập | Mô tả                                             | Mặc định
----------|-----------|---------------------------------------------------|-----------
`string`  | name      | name of the cookie used to store the session data | `"session"`
`boolean` | encrypted | encrypt the session data using [Encrypt]?         | `FALSE`
`integer` | lifetime  | number of seconds the session should live for     | `0`

#### Bộ điều hợp cớ sở dữ liệu

Kiểu      | Thiết lập | Mô tả                                             | Mặc định
----------|-----------|---------------------------------------------------|-----------
`string`  | group     | [Database::instance] group name                   | `"default"`
`string`  | table     | table name to store sessions in                   | `"sessions"`
`array`   | columns   | associative array of column aliases               | `array`
`integer` | gc        | 1:x chance that garbage collection will be run    | `500`
`string`  | name      | name of the cookie used to store the session data | `"session"`
`boolean` | encrypted | encrypt the session data using [Encrypt]?         | `FALSE`
`integer` | lifetime  | number of seconds the session should live for     | `0`

##### Lược đồ bảng dữ liệu

Bạn sẽ cần phải tạo bảng lưu trữ phiên làm việc trong cơ sở dữ liệu. Đây là lược đồ mặc định:

    CREATE TABLE  `sessions` (
        `session_id` VARCHAR(24) NOT NULL,
        `last_active` INT UNSIGNED NOT NULL,
        `contents` TEXT NOT NULL,
        PRIMARY KEY (`session_id`),
        INDEX (`last_active`)
    ) ENGINE = MYISAM;

##### Các cột của bảng

Bạn có thể thay đổi các tên cột để phù hợp với lược đồ cơ sở dữ liệu hiện tại khi kết nối tới một bảng phiên làm việc cũ.
Giá trị mặc định giống như giá trị khoá.

session_id
: tên của cột "id"

last_active
: nhãn thời gian UNIX của thời điểm cuối khi phiên làm việc đã được cập nhật

contents
: dữ liệu phiên làm việc được lưu trữ như một chuỗi tuần tự, và tuỳ chọn mã hoá.