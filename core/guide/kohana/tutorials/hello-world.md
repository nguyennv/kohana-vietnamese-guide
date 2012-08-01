# Chào thế giới

Cũng như mọi khung làm việc đã từng được viết có vài loại ví dụ chào thế giới được bao gồm, vì vậy nó sẽ rất thô lỗ khi chúng ta phá vỡ truyền thống này!

Chúng ta sẽ bắt đầu bằng cách tạo một ví dụ chào thế giới rất rất cơ bản, và sau đó chúng ta sẽ mở rộng nó theo các nguyên tắc MVC.

## Xương sống - Bare bones

Trước hết chúng ta phải làm một điều khiển mà Kohana có thể sử dụng để xử lý một yêu cầu.

Tạo tập tin `application/classes/controller/hello.php `trong thư mục ứng dụng của bạn và điền nó như thế này:

    <?php defined('SYSPATH') OR die('No Direct Script Access');

	Class Controller_Hello extends Controller
	{
		public function action_index()
		{
			echo 'hello, world!';
		}
	}

Hãy xem những gì đang xảy ra ở đây:

`<?php defined('SYSPATH') OR die('No Direct Script Access');`
:	Bạn nên công nhận thẻ đầu tiên như một thẻ php mở (nếu không bạn nên [tìm hiểu php](http://php.net)). Cái mà theo sau là một kiểm tra nhỏ mà để chắc chắn rằng tập tin này đăng được bao gồm bởi Kohana. Nó ngăn người ta truy cập các tập tin trực tiếp từ url.

`Class Controller_Hello extends Controller`
:	Dòng này khai báo điều khiển của chúng ta, mỗi lớp điều khiển được bắt đàu với `Controller_` và một đường dẫn gạch dưới phân cách tới thư mục mà điều khiển ở trong (xem [Quy ước và phong cách](../conventions) để biết thêm thông tin). Mỗi điều khiển cũng nên mở rộng lớp `Controller` cơ sở cái mà cung cấp một cấu trúc chuẩn cho các điều khiển.


`public function action_index()`
:	Đoạn mã này định nghĩa hành động "index" của điều khiển của chúng ta. Kohana sẽ cố gắng gọi hành động này nếu người dùng không chỉ định một hành động. (Xem [Định tuyến, URL và Liên kết](../routing))

`echo 'hello, world!';`
:	Và đây là dòng mà kết xuất đầu ra cụm từ thông thường!

Bây giờ nếu bạn mở trình duyệt và truy cập vào http://localhost/index.php/hello bạn sẽ thấy vài thứ giống như:

![Hello, World!](hello_world_1.png "Hello, World!")

## Thật là tốt, nhưng chúng ta có thể làm tốt hơn

Nhứng gì chúng ta đã làm trong phần trước là một ví dụ tốt về cách dẽ dàng để tạo một ứng dụng Kohana *cực kỳ* cơ bản.
(Trong thực tế nó rất cơ bạn, rằng bạn sẽ không bao giờ làm lại nó!)

Nếu bạn chưa từng nghe nó bất kỳ điều gì về MVC có thể bạn sẽ nhận ra rằng việc echo ra nội dung trong một điều khiển là nghiêm khắc đối với nguyên tắc của MVC.

Cách thích hợp hơn để viết mã với một khung làm việc MVC là sử dụng _khung nhìn_ để xử lý việc trình bày ứng dụng của bạn, và cho phép điều khiển để làm những gì nó làm tốt nhất - điều khiển luồng yêu cầu!

Hãy thay đổi điều khiển ban đầu của chúng ta một chút:

    <?php defined('SYSPATH') OR die('No Direct Script Access');

	Class Controller_Hello extends Controller_Template
	{
		public $template = 'site';

		public function action_index()
		{
			$this->template->message = 'hello, world!';
		}
	}

`extends Controller_Template`
:	Chúng ta hiện thời đang mở rộng một điều khiển khuôn mẫu, nó làm cho thuận tiện hơn để sử dụng các khung nhìn trong điều kiển của chúng ta.

`public $template = 'site';`
:	Điều khiển khuôn mẫu cần biết khuôn mẫu nào mà bạn muốn sử dụng. Nó sẽ tự động tải khung nhìn đã định nghĩa trong biến này và gán đối tượng khung nhìn tới nó.

`$this->template->message = 'hello, world!';`
:	`$this->template` là một tham chiếu tới đối tượng khung nhìn cho khuôn mẫu trang của chúng ta. Những gì chúng ta đang làm ở đây là gán một biết được gọi là "message", với một giá trị "hello, world!" tới khung nhìn.

Bây giờ hãy thử chạy mã ...

![Hello, World!](hello_world_2_error.png "Hello, World!")

Đối với một vài lý do Kohana ném một thông điệp loạng choạng và không hiển thị thông điệp tuyệt vời của chúng ta.

Nếu chúng ta nhìn thấy thông báo lỗi chung ta có thể thấy rằng thư viện View không thể tìm thấy khuôn mẫu trang của chúng ta, có lẽ bởi vì chung ta chưa tạo nó – doh!

Chúng ta hay tới và tạo tập tin khung nhìn `application/views/site.php` cho thông điệp của chúng ta:

	<html>
		<head>
			<title>We've got a message for you!</title>
			<style type="text/css">
				body {font-family: Georgia;}
				h1 {font-style: italic;}

			</style>
		</head>
		<body>
			<h1><?php echo $message; ?></h1>
			<p>We just wanted to say it! :)</p>
		</body>
	</html>

Nếu chúng ta nạp lại trang thì chúng ta có thể thấy thành quả lao động của chúng ta:

![hello, world! We just wanted to say it!](hello_world_2.png "hello, world! We just wanted to say it!")

## Giai đoạn 3 - Lợi nhuận!

Trong hướng dẫn này bạn đã học cách tạo một điều khiển và sử dụng một khung nhìn để tách biệt logic của bạn từ màn hình của bạn.

Điều này rõ ràng là một giới thiệu rất cơ ban để làm việc với Kohana và thậm chí không cạo tiềm năng bạn có khi phát các triển ứng dụng với nó.
