# Xử lý lỗi/ngoại lệ

Kohana cung cấp cả hai một bộ sử lý ngoại lệ và một bộ xử lý lỗi mà biến đổi các lỗi thành các ngoại lệ bằng cách sử dụng lớp [ErrorException](http://php.net/errorexception) của PHP.
Nhiều chi tiết của lỗi và tình trạng nội bộ của ứng dụng được hiển thị bởi bộ sử lý:

1. Lớp ngoại lệ
2. Mức lỗi
3. Thông điệp lỗi
4. Nguồn của lỗi, với dòng lỗi được làm tô sáng
5. Một [gỡ rối truy ngược](http://php.net/debug_backtrace) của luồng thực hiện
6. Các tập tin được bao gồm, các phần mở rộng được nạp, và các biến toàn cục

## Ví dụ

Nhấp vào bất kỳ liên kết để chuyển đổi việc hiển thị thông tin bổ sung:

<div>{{userguide/examples/error}}</div>

## Vô hiệu xử lý lỗi/ngoại lệ

Nếu bạn không muốn sử dụng việc xử lý lỗi nội bộ, bạn có thể vô hiệu nó (rất khuyến khích) khi gọi [Kohana::init]:

    Kohana::init(array('errors' => FALSE));

## Thông báo lỗi

Mặc định, Kohana hiển thị tất cả các lỗi, bao gồm các cảnh báo chế độ nghiêm ngặt.
Việc này được thiết lập bằng các sử dụng hàm [error_reporting](http://php.net/error_reporting):

    error_reporting(E_ALL | E_STRICT);

Khi ứng dụng của bạn sống và trong chế độ production, một thiết lập thận trọng hơn được khuyến cáo, chẳng hạn như bỏ qua các thông báo:

    error_reporting(E_ALL & ~E_NOTICE);

Nếu bạn nhật một màn hình trắng khi một lỗi được kích hoạt, máy chủ của bạn có thể đã vô hiệu việc hiển thị lỗi.
Bạn có thể bật nó trở lại bằng việc thêm dòng này chỉ ngay sau lời gọi `error_reporting`:

    ini_set('display_errors', TRUE);

Các lỗi nên **luôn luôn** được hiển thị, thậm chí trong chế độ production, bỏi vì nó cho phép bạn sử dụng [sử lý ngoại lệ và lỗi](errors) để phục vụ một trang báo lỗi đẹp hơn là một màn hình trống màu trắng khi một lỗi xảy ra.

## Xử lý ngoại lệ HTTP

Kohana đi kèm với một hệ thống mạnh mẽ cho việc xử lý các lỗi http.
Nó bao gồm các lớp ngoại lệ cho mỗi mã trạng thái http.
Để kích hoạt một trạng thái 404 trong ứng dụng của bạn (kịch bản phổ biến nhất):

	throw new HTTP_Exception_404('Tập tin không tìm thấy!');

Không có phương thức mặc định để xử lý các lỗi này trong Kohana.
Nó được khuyến cáo rằng bạn thiết lập một bộ xử lý ngoại lệ (và đăng ký nó) để xử lý các loại lỗi này.
Ở đây là một ví dụ đơn giản mà có thể ở trong */application/classes/foobar/exception/handler.php*:

	class Foobar_Exception_Handler
	{
		public static function handle(Exception $e)
		{
			switch (get_class($e))
			{
				case 'Http_Exception_404':
					$response = new Response;
					$response->status(404);
					$view = new View('error_404');
					$view->message = $e->getMessage();
					$view->title = 'Tập tin không tìm thấy';
					echo $response->body($view)->send_headers()->body();
					return TRUE;
					break;
				default:
					return Kohana_Exception::handler($e);
					break;
			}
		}
	}

Và đặt vài cái gì đó giống như thế này trong tập tin bootstrap của bạn để đăng ký bộ xử lý.

	set_exception_handler(array('Foobar_Exception_Handler', 'handle'));

 > *Lưu ý:* Chắc chắn đặt `set_exception_handler()` **sau** `Kohana::init()` trong bootstrap của bạn, hoặc nó sẽ không làm việc.
 
 > Nếu bạn nhận *Fatal error: Exception thrown without a stack frame in Unknown on line 0*, nó có nghĩa rằng có một lỗi bên trong bộ xử lý ngoại lệ của bạn. Nếu sử dụng ví dụ ở trên, chắc chắn *404.php* tồn tại dưới thư mục */application/views/error/*.
