# Mở rộng Trình dựng truy vấn

Một trong những yếu tố chính của MVC là để giữ cho tất cả logic nghiệp vụ được gói gọn và trong cùng một nơi.
Giao diện liệt kê dựa trên truy vấn của Jelly là vô cùng tự nhiên và linh hoạt, tuy nhiên bạn không phải xây dựng logic liệt kê của bạn trong điều khiển.

Jelly cung cấp một giải pháp sạch sẽ cho điều này bằng cách cho phép bạn mở rộng trình dựng truy vấn Jelly trên cơ sở mỗi mô hình.
Ví dụ, nếu bạn cần định nghĩa một người dùng 'active' như một người mà đã đăng nhập trong 3 tháng qua, bạn có thể làm cái gì đó như thế này:

	$active_users = Jelly::query('user')->where('last_login', '>', strtotime('-3 month'))->execute();

Nhưng bạn không muốn đặt trực tiếp truy vấn này trong điều khiển của bạn vì nó là logic nghiệp vụ.
Hơn nữa, nếu bạn muốn thay đổi định nghĩa của bạn về 'active' sau này, bạn sẽ cần tìm kiếm qua mã của bạn và cập nhật nạp các truy vấn.

Thay vào đó bạn có thể đóng gói logic này bằng việc tạo một lớp có tên `Model_Builder_User` mà mở rộng `Jelly_Builder`.
Nếu Jelly tìm thấy một lớp có tên trong một cách như vậy, nó sẽ luôn trả lại mà thay vì `Jelly_Builder` mặc định cho bất kỳ truy vấn được xây dựng dựa trên mô hình `User`.

Bạn có thể thêm bất kỳ logic liệt kê nào bạn muốn tới việc xây dựng truy vấn của bạn.
Bạn thậm chí có thể thay đổi hành vi trình dựng truy vấn mặc định tới tài khoản để thoái thác trong một mô hình cụ thể.

Ví dụ những người dùng tích cực của chúng ta được giải quyết như sau:

	class Model_Builder_User extends Jelly_Builder
	{
		public function active()
		{
			return $this->where('last_login', '>', strtotime('-3 month'));
		}
	}

	// Giờ chúng ta có thể làm như thế này
	$active_users = Jelly::query('user')->active()->execute();

Chúng ta có thể gọi dây chuyền các phương thức đã định nghĩa tuỳ biến nếu cần thiết.

Bạn thậm chí có thể lấy hơn nữa: nói một vài mô hình của bạn yêu cầu một vài logic bổ xung tương tự.
Trong trường hợp này, bạn nên định nghĩa một lớp xây dựng trừu trượng mà mở rộng Jelly_Builder và sau đó bạn có thể mở rộng lớp đó lần lượt trong các lớp 'Model_Builder_ModelName`.

[!!] **Lưu ý**: Bất kỳ mô hình nào không có lớp xây dựng tương ứng chỉ đơn giản là sử dụng xây dựng truy vấn cơ bản của `Jelly_Builder`.
