# Mẹo và sai lầm thường gặp

Đây là một tập hợp các mẹo và sai lầm thường gặp hoặc lỗi bạn có thể gặp phải.

## Không chỉnh sửa thư mục `system`!

Bạn nên (hầu như) không bao giờ chỉnh sửa thư mục hệ thống.
Bất kỳ thay đổi nào bạn muốn thực hiện cho các tập tin trong hệ thống và các mô-đun có thể được thực hiện thông qua [hệ thống tập tin phân cấp](files) và [mở rộng trong suốt](extension) và sẽ không phá vỡ khi bạn cố gắng nâng cập phiên bạn Kohana của bạn.

## Không dùng một định tuyến cho mọi thứ

[Định tuyến](routing) Kohana 3 rất mạnh mẽ và linh hoạt, đừng ngại sử dụng nhiều như bạn cần để thực hiện các chức năng ứng dụng của bạn theo cách bạn muốn!

## Xử lý nhiều định tuyến

Đôi khi ứng dụng của bạn là đủ phức tạp mà bạn có nhiều định tuyến và nó sẽ trở thành không thể quản lý để đặt chúng trong bootstrap.php.
Nếu đây là trường hợp, chỉ đơn giản là tạo tập tin `routes.php` ở APPPATH và yêu cầu nó trong bootstrap của bạn: `require_once APPPATH.'routes'.EXT;`

## Reflection_Exception

Nếu bạn nhận một Reflection_Exception khi đang thiết lập trang của bạn, nó hầu như chắc chắn là do thiết lập Kohana::init 'base_url' của bạn là sai.
Nếu url cơ sở của bạn là đúng thì vài thứ có thể sai với [các định tuyến](routing) của bạn

	ReflectionException [ -1 ]: Class controller_<something> does not exist
	// <something> là một phần của url mà bạn đã nhập trong trình duyệt của bạn

### Giải pháp  {#reflection-exception-solution}

Thiết lập Kohana::init 'base_url' của bạn tới thiết lập đúng.
Url cơ bản phải là đường dẫn tới tập tin index.php của bạn liên quan tới tài liệu gốc của máy chủ web.

## Lỗi ORM/Session __sleep()

Có một lõi trong php mà có thể là hỏng phiên của bạn sau một lỗi nghiêm trọng.
Một máy chủ ở chế độ production không nên có các lỗi nghiêm trọng chưa bị bắt, dó đó lỗi này chỉ nên xảy ra trong quá trình phát triển, khi bạn làm điều gì ngu ngốc và gây ra một lỗi nghiêm trọng.
Trên lần tải trang tiếp theo bạn sẽ nhận một lỗi kết nối cơ sở dữ liệu, sau đó tất các các lần tải trang tiếp theo sẽ hiển thị lỗi sau:

	ErrorException [ Notice ]: Undefined index: id
	MODPATH/orm/classes/kohana/orm.php [ 1308 ]

### Giải pháp   {#orm-session-sleep-solution}

Để khắc phục điều này, hãy dọn dẹp các cookie của bạn cho tên miền đó để thiết lập lại phiên làm việc của bạn.
Điểu này không bao giờ nên xẩy ra trên một máy chủ ở chế độ production, vì vậy bạn sẽ không giải thích cho khách hàng của bạn cách dọn dẹp các cookie của họ.
Bạn có thể xem [thảo luận về vấn đề này](http://dev.kohanaframework.org/issues/3242) để biết thêm chi tiết.
