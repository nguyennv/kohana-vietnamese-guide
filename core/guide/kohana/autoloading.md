# Nạp các lớp

Kohana tận dụng khả năng [nạp tự động](http://php.net/manual/language.oop5.autoload.php) của PHP.
Việc này loại bỏ sự cần thiết gọi hàm [include](http://php.net/include) hoặc [require](http://php.net/require) trước khi sử dụng một lớp.
Khi bạn sử dụng một lớp Kohana sẽ tìm và bao gồm tập tin lớp cho bạn.
Ví dụ, khi bạn muốn sử dụng phương thức Cookie::set, bạn đơn giản chỉ gọi:

    Cookie::set('mycookie', 'giá trị chuỗi bất kỳ');

Hoặc để nạp một thể hiện của Encrypt, chỉ cần gọi Encrypt::instance:

    $encrypt = Encrypt::instance();

Các lớp được nạp từ phướng thức Kohana::auto_load, cái mà thực hiện một chuyển đổi đơn giản từ tên lớp tới tên tập tin:

1. Các lớp được đặt trong thư mục `classes/` của ơhệ thống tập tinư(files)
2. Bất kỳ dấu gạch dưới nào trong tên lớp sẽ được chuyển đổi thành dấu gạch chéo
2. Tên tập tin là chữ thường

Khi gọi một lớp mà đã được nạp (vd: `Session_Cookie`),
Kohana sẽ tìm kiếm trong hệ thống tập tin bằng cách sử dụng [Kohana::find_file] cho một tập tin có tên `classes/session/cookie.php`.

Nếu các lớp của bạn không theo quy ước này, chúng có thể không được nạp tự động bởi Kohana.
Bạn sẽ phải bao gồm bằng tay các tập tin của bạn, hoặc thêm [hàm autoload](http://us3.php.net/manual/en/function.spl-autoload-register.php) của chính bạn.

## Tuỳ biến bộ nạp tự động

Bộ nạp tự động mặc định của Kohana được kích hoạt trong `application/bootstrap.php` bằng cách sử dụng [spl_autoload_register](http://php.net/spl_autoload_register):

    spl_autoload_register(array('Kohana', 'auto_load'));

Điều này cho phép [Kohana::auto_load] cố gắng tìm và bao gồm bất kỳ lớp nào mà chưa tồn tại khi lớp được sử dụng lần đầu.

### Ví dụ: Zend

Bạn có thể dễ dàng được truy cập vào thư viện khác nếu chúng bao gồm một bộ nạp tự động.
Ví dụ, ở đây là cách để bật bộ nạp tự động của Zend để bạn có thể sử dụng các thư viện Zend trong ứng dụng Kohana của bạn.

#### Tải về và cài đặt các tập tin của Zend Framework

- [Tải về các tập tin Zend Framework mới nhất](http://framework.zend.com/download/latest).
- Tạo một thư mục vendor tại `application/vendor`. Việc này giữ cho phần mềm hãng thứ ba tách riếng từ các lớp ứng dụng của bạn.
- Chuyển thư mục Zend đã giải nén chứa Zend Framework tới `application/vendor/Zend`.


#### Bao gồm bộ nạp tự động của Zend trong tập tin khởi động của bạn

Ở một nơi nào đó trong `application/bootstrap.php`, sao chép mã sau:

	/**
	 * Bật nạp tự động Zend Framework
	 */
	if ($path = Kohana::find_file('vendor', 'Zend/Loader'))
	{
	    ini_set('include_path',
	    ini_get('include_path').PATH_SEPARATOR.dirname(dirname($path)));
	
	    require_once 'Zend/Loader/Autoloader.php';
	    Zend_Loader_Autoloader::getInstance();
	}
	
#### Ví dụ sử dụng

Bây giờ bạn có thể nạp tự động bất kỳ lớp Zend Framework từ bên trong ứng dụng Kohana của bạn.

	if ($validate($_POST))
	{
		$mailer = new Zend_Mail;
		
		$mailer->setBodyHtml($view)
			->setFrom(Kohana::$config->load('site')->email_from)
			->addTo($email)
			->setSubject($message)
			->send();
	}