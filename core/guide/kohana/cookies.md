# Cookie

Kohana cung cấp các lớp mà làm cho nó dễ dàng làm việc với cả các cookie và phiên làm việc.
Ở mức cao cả hai phiên làm việc và cookie cung cấp các chức năng tương tự.
Chúng cho phép các nhà phát triển cho lưu trữ thông tin tạm thời hay kiên gan về một trình khách cụ thể cho việc lấy lại về sau, thường là làm cho một vài kiên gan tục giữa các yêu cầu.

[Cookies](http://en.wikipedia.org/wiki/HTTP_cookie) nên được sử dụng để lưu trữ dữ liệu không riêng tư mà được kiên gan cho một khoảng thời gian dài.
Ví dụ việc lưu trữ một tham chiếu người dùng hoặc một thiết lập ngôn ngữ.
Sử dụng lớp [Cookie] cho việc nhận và thiết lập cookies.

[!!] Kohana sử dụng các cookie "được ký". Mọi cookie mà được lưu trữ được kết hợp với một hàm băm bảo mật để ngăn ngừa thay đổi của cookie. Nếu một cookie được sử đổi bên ngoài Kohana việc băm sẽ không đúng và cookie sẽ bị xoá. Băm này được sinh bằng cách sử dụng [Cookie::salt()], cái mà sử dụng thuộc tính [Cookie::$salt]. Bạn phải định nghĩa thiết lập này trong tập tin bootstrap.php của bạn:

	Cookie::$salt = 'foobar';

Hoặc định nghĩa một lớp cookie mở rộng trong ứng dụng của bạn:

	class Cookie extends Kohana_Cookie
	{
		public static $salt = 'foobar';
	}

Bạn nên thiết lập biến salt tới một giá trị bảo mật. Ví dụ trên chỉ nhằm mục đích trình bày.

Không có gì ngăn bạn sử dụng `$_COOKIE` như bình thường, nhưng bạn không thể kết hợp sử dụng lớp Cookie và biến toàn cục `$_COOKIE` bình thường, vì việc băm mà Kohana sử dụng để ký cookies sẽ không có, và Kohana sẽ xoá cookie.

## Lưu trữ, truy lục, và xóa dữ liệu

[Cookie] và [Session] cung cấp một API rất giống để lưu trữ dữ liệu.
Sự khác biệt chính giữa chúng là các phiên làm việc được truy cập bằng cách sử dụng một đối tượng, và các cookie được truy cập bằng cách sử dụng một lớp tĩnh.

### Lưu trữ dữ liệu

Việc lưu trữ dữ liệu phiên làm việc hoặc cookie được thực hiện bằng việc sử dụng phương thức [Cookie::set]:

    // Thiết lập dữ liệu cookie
    Cookie::set($key, $value);

    // Lưu trữ một id người dùng
    Cookie::set('user_id', 10);

### Truy lục dữ liệu

Việc nhận dữ liệu phiên làm việc hoặc cookie được thực hiện bằng việc sử dụng phương thức [Cookie::get]:

    // Nhận dữ liệu cookie
    $data = Cookie::get($key, $default_value);

    // Nhận id người dùng
    $user = Cookie::get('user_id');

### Xoá dữ liệu

Việc xoá dữ liệu phiên làm việc hoặc cookie được thực hiện bằng việc sử dụng phương thức [Cookie::delete]:
    
    // Xoá dữ liệu cookie
    Cookie::delete($key);

    // Xoá dữ liệu người dùng
    Cookie::delete('user_id');

## Thiết lập cookie

Tất cả thiết lập cookie được thay đổi bằng cách sử dụng thuộc tính tĩnh.
Bạn có thể thay đổi các thiết lập trong `bootstrap.php` hoặc bằng cách sử dụng [mở rộng trong suốt](extension).
Luôn kiểm tra các thiết lập này trước khi đưa ra ứng dụng của bạn sống, như nhiều thiết lập của chúng sẽ có một ảnh hưởng trực tiếp đến bảo mật của ứng dụng của bạn.

Thiết lập quan trọng nhất là [Cookie::$salt], cái mà được sử dụng cho việc ký bảo mật. Giá trị này nên được thay đổi và được giữ bí mật:

    Cookie::$salt = 'bí mật của bạn là an toàn với tôi';

[!!] Việc thay đổi giá trị này sẽ làm cho tất cả các cookie đã được thiết lập trước không hợp lệ.

Mặc định, các cookie được lưu trữ tới khi trình duyệt bị đóng.
Để sử dụng một vòng đợi cụ thể, thay đổi thiết lập [Cookie::$expiration]:

    // Thiết lập cookie hết hạn sau 1 tuần
    Cookie::$expiration = 604800;

    // Thay thế cho sử dụng số nguyên, cho rõ ràng hơn
    Cookie::$expiration = Date::WEEK;

Đường dẫn mà cookie có thể được truy cập từ đó có thể được hạn chế bằng cách sử dụng thiết lập [Cookie::$path].

    // Chỉ cho phép cookie khi đi vào /public/*
    Cookie::$path = '/public/';

Tên miền mà cookie có thể được truy cập mà từ đó cũng có thể được hạn chế, sử dụng thiết lập [Cookie::$domain].

    // Chỉ cho phép cookie trên miền www.example.com
    Cookie::$domain = 'www.example.com';

Nếu bạn muốn làm các cookie có thể truy cập trên tất cả tên miên con, sử dụng một dấu chấm ở đầu tên miền.

    // Cho phép các cookie được truy cập trên example.com và *.example.com
    Cookie::$domain = '.example.com';

Để chi cho phép cookie được truy cập qua một kết nối bảo mật (HTTPS), sử dụng thiết lập [Cookie::$secure] .

    // Cho phép các cookie được truy cập chỉ trên một kết nối bảo mật
    Cookie::$secure = TRUE;
    
    // Cho phép các cookie được truy cập trên kết nối bất kỳ
    Cookie::$secure = FALSE;

Để ngăn chặn các cookie được truy cập từ việc sử dụng Javascript, bạn có thể thay đổi thiết lập [Cookie::$httponly].

    // Làm cho các cookie không thể truy cập bởi Javascript
    Cookie::$httponly = TRUE;