# Khởi động

Tập tin bootstrap được định vị tại `application/bootstrap.php`.
Nó có trách nhiệm thiết lập môi trường Kohana và chạy trình phản hồi chính.
Nó được bao gồm bởi index.php (Xem [Luồng yêu cầu](flow))

[!!] Bootstrap chịu trách nhiệm cho luồng ứng dụng của bạn.
Trong các phiên bản trước của Kohana bootstrap ở trong thư mục `system` và là một vài thứ bắt buộc không được sửa, không được thấy.
Trong Kohana 3 bootstrap mang một vai trò tích hợp và linh hoạt hơn.
Đừng sợ sửa và thay đổi việc khởi động của bạn khi bạn thấy phù hợp.

## Thiết lập môi trường

Đầu tiên bootstrap thiết lập múi giờ và miền địa phương, và thêm bộ nạp tự động của Kohana để [hệ thống tập tin phân cấp](files) làm việc.
Bạn có thể thêm bất kỳ thiết lập khác mà tất cả ứng dụng của bạn cần ở đây.

~~~
// Mẫu trích dẫn từ bootstrap.php với chú thích bị cắt giảm

// Thiết lập múi giờ mặc định.
date_default_timezone_set('Asia/Saigon');
 
// Thiết lập địa phương mặc định.
setlocale(LC_ALL, 'vi_VN.utf-8');
 
// Bật bộ tự động nạp của Kohana.
spl_autoload_register(array('Kohana', 'auto_load'));
 
// Bật bộ tự động nạp của Kohana cho việc bỏ nối tiếp hoá - unserialization.
ini_set('unserialize_callback_func', 'spl_autoload_call');
~~~

## Khởi tạo và cấu hình

Kohana sau đó được khởi tạo bằng việc gọi [Kohana::init], và trình đọc/ghi lưu vết và [cấu hình](files/config) được bật.

~~~
// Mẫu trích dẫn từ bootstrap.php với chú thích bị cắt giảm

Kohana::init(array('
    base_url' => '/kohana/',
	index_file => false,
));

// Đính kèm trình ghi tập tin vào việc lưu vết. Nhiều trình ghi được hỗ trợ.
Kohana::$log->attach(new Kohana_Log_File(APPPATH.'logs'));

// Đính kèm trình đọc tập tin vào cấu hình. Nhiều trình đọc được hỗ trợ.
Kohana::$config->attach(new Kohana_Config_File);
~~~

Bạn có thể thêm các câu lệnh điều kiện để làm bootstrap có các giá trị khác nhau dựa trên các thiết lập nhất định.
Ví dụ, phát hiện xem chúng ta đang sống bằng cách kiểm tra `$_SERVER['HTTP_HOST']` và thiết lập bộ nhớ đệm, hập hồ sơ, v.v. cho phù hợp.
Đây chỉ là một ví dụ, có rất nhiều cách khác nhau để thực hiện điều tương tự.

~~~
// Trích từ http://github.com/isaiahdw/kohanaphp.com/blob/f2afe8e28b/application/bootstrap.php
... [đã cắt tỉa]
 
/**
 * Thiết lập trạng thái môi trường bằng tên miền.
 */
if (strpos($_SERVER['HTTP_HOST'], 'kohanaphp.com') !== FALSE)
{
	// Chúng ta đang sống!
	Kohana::$environment = Kohana::PRODUCTION;
 
	// Tắt thông báo và lỗi chặt chẽ
	error_reporting(E_ALL ^ E_NOTICE ^ E_STRICT);
}
 
/**
 * Khởi tạo Kohana, thiết lập các tùy chọn mặc định.
 ... [đã cắt tỉa]
 */
Kohana::init(array(
	'base_url'   => Kohana::$environment === Kohana::PRODUCTION ? '/' : '/kohanaphp.com/',
	'caching'    => Kohana::$environment === Kohana::PRODUCTION,
	'profile'    => Kohana::$environment !== Kohana::PRODUCTION,
	'index_file' => FALSE,
));

... [đã cắt tỉa]

~~~

[!!] Lưu ý: Bootstrap mặc định sẽ thiết lập `Kohana::$environment = $_ENV['KOHANA_ENV']` nếu thiết lập. 
Tài liệu về làm thế nào để cung cấp biến này có sẵn trong tài liệu của máy chủ web của bạn (v.d. [Apache](http://httpd.apache.org/docs/2.4/mod/mod_env.html#setenv), [Lighttpd](http://redmine.lighttpd.net/wiki/1/Docs:ModSetEnv#Options)).
Việc này được xem là tốt hơn thực tế nhiều phương pháp thay thế để thiết lập `Kohana::$enviroment`, như bạn có thể thay đổi các thiết lập cho mỗi máy chủ, mà không cần phải dựa vào tuỳ chọn cấu hình hoặc tên máy.

## Mô-đun

**Đọc trang [Mô-đun](modules) để biết mô tả chi tiết hơn.**

[Mô-đun](modules) sau đó được nạp bằng cách sử dụng [Kohana::modules()]. Việc bao gồm các mô-đun là tuỳ chọn.

Mỗi một khoá trong mảng nên là tên của mô-đun, và giá trị là đường dẫn tới mô-đun, hoặc tương đối hoặc tuyệt đối.
~~~
// Ví dụ trích đoạn từ bootstrap.php

Kohana::modules(array(
	'database'   => MODPATH.'database',
	'orm'        => MODPATH.'orm',
	'userguide'  => MODPATH.'userguide',
));
~~~

## Định tuyến

**Đọc trang [Định tuyến](routing) để biết thêm mô tả chi tiết và các ví dụ.**

[Định tuyến](routing) được định nghĩa từ [Route::set()].

~~~
// Định tuyến mặc định mà đến từ Kohana 3
Route::set('default', '(<controller>(/<action>(/<id>)))')
	->defaults(array(
		'controller' => 'welcome',
		'action'     => 'index',
	));
~~~
