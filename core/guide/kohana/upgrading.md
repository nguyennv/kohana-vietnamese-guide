# Chuyển từ 3.1.x

## Cấu hình

Hệ thống cấu hình đã được viết lại để làm cho nó linh hoạt hơn.
Sự thay đổi đáng kể nhất là nhóm cấu hình được sáp nhập trên tất cả các bộ đọc, tương tự như cách các tập tin phân tầng trong hệ thống tập tin phân cấp.
Vì vậy, bạn có thể ghi đè lên một giá trị cấu hình duy nhất (có lẽ là một kết nối cơ sở dữ liệu chuỗi, hoặc một địa chỉ email 'gửi từ') và lưu trữ nó trong một nguồn cấu hình riêng biệt (tập tin cấu hình môi trường / cơ sở dữ liệu / tùy chỉnh), trong khi kế thừa các thiết lập còn lại từ nhóm đó từ các tập tin cấu hình ứng dụng của bạn.

Trong khía cạnh khác, đa số các API công cộng vẫn hoạt động trong cùng một cách, tuy nhiên một thay đổi lớn là quá trình chuyển đổi từ sử dụng `Kohana::config()` thành `Kohana::$config->load()`), nơi `Kohana::$config` là một thể hiện của lớp `Config`.

`Config::load()` hoạt động gần như giống hệt với `Kohana::config()`, ví dụ:

	Kohana::$config->load('dot.notation')
	Kohana::$config->load('dot')->notation

Đơn giản là tìm/thay thế cho `Kohana::config`/`Kohana::$config->load` trong phạm vi dự án của bạn nên sửa lỗi này.

Thuật ngữ cho nguồn cấu hình cũng đã thay đổi.
Bản tiền 3.2 cấu hình được nạp từ "Config Readers" cái mà cả đọc và ghi cấu hình.
Trong 3.2 có **Config Readers** và **Config Writers**, cả hai đều là một kiểu của **Config Source**.

Một **Config Reader** được thực hiện bằng cách thực hiện các giao diện `Kohana_Config_Reader`;
tương tự một **Config Writer** được thực hiện bằng cách thực hiện các giao diện `Kohana_Config_Writer`.

ví dụ: cho Database:

	class Kohana_Config_Database_Reader implements Kohana_Config_Reader
	class Kohana_Config_Database_Writer extends Kohana_Config_Database_Reader implements Kohana_Config_Writer

Mặc dù không bị ép buộc, quy ước là bộ ghi mở rộng bộ đọc cấu hình.

Để giúp duy trì khả năng tương thích ngược khi tải các nguồn cấu hình các lớp rỗng được cung cấp cho các nguồn db/tập tin mà mở rộng bộ đọc/bộ ghi của nguồn.

ví dụ:

	class Kohana_Config_File extends Kohana_Config_File_Reader
	class Kohana_Config_Database extends Kohana_Config_Database_Writer

## Yêu cầu bên ngoài

Trong Kohana 3.2, `Request_Client_External` bây giờ có ba trình điều khiển riêng biệt để xử lý các yêu cầu bên ngoài;

 - `Request_Client_Curl` là trình điều khiển mặc định, bằng cách sử dụng mở rộng Curl của PHP
 - `Request_Client_HTTP` sử dụng mở rộng PECL HTTP
 - 	Request_Client_Stream` sử dụng luồng gốc - streams native PHP và không yêu cầu mở rộng. Tuy nhiên phương pháp này là chậm hơn so với các lựa chọn thay thế.

Trừ khi có quy định khác, `Request_Client_Curl` sẽ được sử dụng cho tất cả các yêu cầu bên ngoài.
Điều này có thể được thay đổi cho tất cả các yêu cầu bên ngoài, hoặc cho các yêu cầu riêng lẻ.

Để thiết lập một trình điều khiển bên ngoài trên tất cả các yêu cầu, thêm dòng sau vào tập tin `application/bootstrap.php`;

    // Thiết lập tất cả các yêu cầu bên ngoài sử dụng PECL HTTP
    Request_Client_External::$client = 'Request_Client_HTTP';

Ngoài ra, nó có thể thiết lập một trình khách cụ thể cho một yêu cầu riêng lẻ.

    // Thiết lập trình khách Stream tới yêu cậu riêng lẻ và
    // Thực thi
    $response = Request::factory('http://kohanaframework.org')
        ->client(new Request_Client_Stream)
        ->execute();

## Điều khiển bộ nhớ đệm HTTP

Kohana 3,1 giới thiệu điều khiển bộ nhớ đệm HTTP, cung cấp bộ nhớ đệm trong suốt hoàn toàn tuân thủ chuẩn RFC 2616 của các phản hồi.
Kohana 3,2 xây dựng điều này băng cách chuyển tất cả logic bộ nhớ đệm ra khỏi `Request_Client` và đưa vào `HTTP_Cache`.

[!!] HTTP Cache yêu cầu các mô-đun Cache được bật trong tất cả các phiên bản của Kohana!

Trong Kohana 3.1, bộ nhớ đệm HTTP được kích hoạt bằng cách làm như sau;

    // Áp dụng bộ nhớ đệm tới một yêu cầu
    $request = Request::factory('foo/bar', Cache::instance('memcache'));

    // Trong điều khiển, đảm bảo phản hồi được thiết lập điều khiển bộ nhớ đệm,
    // việc này sẽ lưu vào bộ nhớ đệm phản hồi trong một giờ
    $this->response->headers('cache-control', 
        'public, max-age=3600');

Trong Kohana 3.2, bộ nhớ đệm HTTP được kích hoạt hơi khác;

    // Áp dụng bộ nhớ đệm tới một yêu cầu
    $request = Request::factory('foo/bar',
        HTTP_Cache::factory('memcache'));

    // Trong điều khiển, đảm bảo phản hồi được thiết lập điều khiển bộ nhớ đệm,
    // việc này sẽ lưu vào bộ nhớ đệm phản hồi trong một giờ
    $this->response->headers('cache-control', 
        'public, max-age=3600');

## Các tham số hành động của điều khiển

Trong 3.1, các tham số hành động điều khiển không tán thành.
Trong 3.2, hành vi này đã được gỡ bỏ.
Nếu bạn có bất kỳ mã như:

	public function action_index($id)
	{
		// ... mã
	}

Bạn sẽ cần phải thay đổi nó thành:

	public function action_index()
	{
		$id = $this->request->param('id');

		// ... mã
	}

## Lớp Form

Nếu bạn sử dụng Form::open(), hành vi mặc định đã thay đổi.
Nó sử dụng mặc định "/" (trang chủ), nhưng bây giờ một tham số trống sẽ mặc định là URI hiện tại.
