Loại form
==========

Formo sử dụng một hệ thống trang trí để cung cấp các chức năng mở rộng tới các form dựa trên các loại dữ liệu mà mó sẽ xử lý.

Một ví dụ của điều này, một *loại form* là **html**.
Một form **html** cần để kết xuất các trường nhập liệu của nó như các thẻ input html, các hộp textearea, các nút, v.v.

### Xác định loại form

Loại form mặc định được định nghĩa trong **config/formo.php** như là `type`.

Nếu bạn cần tạo form với một form có loại cụ thể đầu tiên bên cạnh một loại mặc định, bạn có thể thiết lập nó với `type`:

	$form = Formo::factory(array('type' => 'json'));
	
Nếu bạn cần thay đổi tại bất kỳ điểm nào sau đó, hãy sử dụng phương thức `type()`:

	$form->type('json');