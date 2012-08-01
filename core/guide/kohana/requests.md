# Yêu cầu

Kohana bao gồm một hệ thống yêu cầu HMVC linh hoạt.
Nó hỗ trợ không gò bó, hỗ trợ cho các yêu cầu nội bộ và yêu cầu bên ngoài.

HMVC là viết tắt của `Hierarchical Model View Controller` và về cơ bản có nghĩa là các yêu cầu có thể có bộ ba MVC được gọi bên trong mỗi yêu cầu khác.

Đối tượng Yêu cầu trong Kohana tương thích với chuẩn HTTP/1.1

## Tạo các Yêu cầu

Việc tạo một yêu cầu là rất dễ dàng:

### Yêu cầu nội bộ

Một yêu cầu nội bộ là một yêu cầu gọi tới ứng dụng nội bộ.
Nó sử dụng các [định tuyến](routing) để hướng đến ứng dụng dựa trên URI được truyền tới nó.
Một yêu cầu cơ bản nội bộ có thể trông giống như:

	$request = Request::factory('welcome');

Trong ví dụ này, URI là 'welcome'.

#### Yêu cầu ban đầu

Kể từ khi Kohana sử dụng HMVC, bạn có thể gọi rất nhiều yêu cầu bên trong mỗi yêu cầu khác.
Yêu cầu đầu tiên (thường được gọi là từ `index.php`) được gọi là "yêu cầu ban đầu".
Bạn có thể truy cập yêu cầu này thông qua:

	Request::initial();

Bạn chỉ nên sử dụng phương thức này nếu bạn hoàn toàn chắc chắn rằng bạn muốn yêu cầu ban đầu.
Nếu không, bạn nên sử dụng phương thức `Request::current()`.

#### Yêu cầu con

Bạn có thể gọi một yêu cầu bất cứ lúc nào trong ứng dụng của bạn bằng cách sử dụng cú pháp `Request::factory()`.
Tất cả những yêu cầu này sẽ được coi là yêu cầu con.

Ngoài sự khác biệt này, chúng chính xác là như nhau.
Bạn có thể phát hiện nếu yêu cầu là một yêu cầu con trong điều khiển của bạn với phương thức is_initial():

	$sub_request = ! $this->request->is_initial()

### Yêu cầu bên ngoài

Một yêu cầu bên ngoài gọi ra một trang web bên thứ ba.

Bạn có thể sử dụng để hút HTML từ một trang web từ xa, hoặc thực hiện một lời gọi REST tới một API của bên thứ ba:

	// Sử dung phương thức GET
	$request = Request::factory('http://www.google.com/');

	// Sử dung phương thức PUT
	$request = Request::factory('http://example.com/put_api')->method(Request::PUT)->body(json_encode('the body'))->headers('Content-Type', 'application/json');

	// Sử dung phương thức POST
	$request = Request::factory('http://example.com/post_api')->method(Request::POST)->post(array('foo' => 'bar', 'bar' => 'baz'));

## Thực thi yêu cầu

Để thực thi một yêu cầu, sử dụng phương thức `execute()` trên đó.
Việc này sẽ cung cấp cho bạn một đối tượng [phản hồi](responses).

	$request = Request::factory('welcome');
	$response = $request->execute();

## Yêu cầu điều khiển cache

Bạn có thể lưu vào bộ nhớ đệm các yêu cầu cho việc thực thi nhanh chóng bằng cách truyền một thể hiện của bộ nhớ đệm vào tham số thứ hai của phương thức factory:

	$request = Request::factory('welcome', Cache::instance());

TODO
