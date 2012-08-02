# Xác nhận hợp lệ

Các mô hình ORM được tích hợp chặt chẽ với thư viện [Validation] và mô-đun đi kèm với một lớp [ORM_Validation_Exception] rất linh hoạt mà giúp bạn nhanh chóng xử lý các lỗi xác nhận hợp lệ từ các hoạt động CRUD cơ bản.

## Định nghĩa quy tắc

Các quy tắc xác nhận hợp lệ được định nghĩa trong phương thức `ORM::rules()`.
Phương thức này trả về một mảng các quy tắc được thêm tới đối tượng [Validation] giống như:

	public function rules()
	{
		return array(
			'username' => array(
				// Sử dụng Valid::not_empty($value);
				array('not_empty'),
				// Gọi Some_Class::some_method('param1', 'param2');
				array('Some_Class::some_method', array('param1', 'param2')),
				// Gọi A_Class::a_method($value);
				array(array('A_Class', 'a_method')),
				// Gọi hàm lambda và truyền giá trị trường và đối tượng xác nhận hợp lệ
				array(function($value, Validation $object)
				{
					$object->error('some_field', 'some_error');
				}, array(':value', ':validation')),
			),
		);
	}

### Các giá trị ràng buộc

ORM sẽ tự động ràng buộc các giá trị sau với phương thức `Validation::bind()`:

- **:field** - Tên của trường mà quy tắc đang được áp dụng tới.
- **:value** - Giá trị của trường mà quy tắc đang áp dụng tới.
- **:model** - Thể hiện của mô hình mà đang được xác nhận hợp lệ.

## Xác nhận hợp lệ tự động

Tất cả mô hình tự động xác nhận dữ liệu của riêng chúng khi `ORM::save()`, `ORM::update()`, hoặc `ORM::create()` được gọi.
Bởi vì điều này, bạn sẽ luôn kỳ vọng các phương thức này ném một [ORM_Validation_Exception] khi dữ liệu của mô hình không hợp lệ.

	public function action_create()
	{
		try
		{
			$user = ORM::factory('user');
			$user->username = 'invalid username';
			$user->save();
		}
		catch (ORM_Validation_Exception $e)
		{
			$errors = $e->errors();
		}
	}

## Xử lý ngoại lệ xác nhận hợp lệ

Lớp [ORM_Validation_Exception] sẽ cung cấp cho bạn truy cập tới các lỗi xác nhận hợp lệ mà đã gặp phải trong khi cố gắng lưu một thông tin của mô hình.
Phương thức `ORM_Validation_Exception::errors()` làm việc rất giống với `Validation::errors()`.
Không truyền một tham số đầu tiên sẽ trả tên của quy tắc mà thất bại.
Nhưng không giống `Validate::errors()`, tham số đầu tiên của `ORM_Validation_Exception::errors()` là một đường dẫn thư mục.
Thuộc tính `ORM::$_object_name` của mô hình sẽ được nối vào thư mục để tạo thành tập tin thông báo cho `Validation::errors()` sử dụng.
Tham số thứ hai giống như tham số của `Validation::errors()`.

Trong ví dụ bên dưới, các thông báo lỗi sẽ được định nghĩa trong `application/messages/models/user.php`

	public function action_create()
	{
		try
		{
			$user = ORM::factory('user');
			$user->username = 'invalid username';
			$user->save();
		}
		catch (ORM_Validation_Exception $e)
		{
			$errors = $e->errors('models');
		}
	}

## Xác nhận hợp lệ bên ngoài

Một số biểu mẫu chứa thông tin mà sẽ không được xác nhận hợp lệ bằng mô hình, nhưng bằng trình điều khiển.
Thông tin như một dấu hiệu [CSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery), xác minh mật khẩu, hoặc một [CAPTCHA](http://en.wikipedia.org/wiki/CAPTCHA) sẽ không bao giờ được xác nhận bằng một mô hình.
Tuy nhiên, thông tin xác nhận trong nhiều nơi và việc kết hợp các lỗi để cung cấp cho người dùng với với môt kinh nghiệm tốt thường khá tẻ nhạt.
Ví lý do này, lớp [ORM_Validation_Exception] được xây dựng để xử lý nhiều đối tượng Validation và không gian tên mảng các lỗi tự động cho bạn.
`ORM::save()`, `ORM::update()`, và `ORM::create()` tất cả có một tham số tuỳ chọn đầu tiên cái mà là một đối tượng [Validation] để xác nhận cùng với mô hình.

	public function action_create()
	{
		try
		{
			$user = ORM::factory('user');
			$user->username = $_POST['username'];
			$user->password = $_POST['password'];

			$extra_rules = Validation::factory($_POST)
				->rule('password_confirm', 'matches', array(
					':validation', ':field', 'password'
				));

			// Truyền các quy tắc để được xác nhận với mô hình
			$user->save($extra_rules);
		}
		catch (ORM_Validation_Exception $e)
		{
			$errors = $e->errors('models');
		}
	}

Bởi vì đối tượng xác nhận được truyền như một tham số tới mô hình, bất kỳ nỗi nào được tìm thấy trong kiểm tra đó sẽ được được vào không gian tên trong một mảng con gọi là `_external`.
Mảng của lỗi sẽ trông giống như thế này:

	array(
		'username'  => 'Trường này không thể để trống.',
		'_external' => array(
			'password_confirm' => 'Các giá trị bạn đã nhập trong các trường mật khẩu không khớp.',
		),
	);

Điều này đảm bảo rằng các lỗi từ nhiều đối tượng xác nhận và mô hình sẽ không bao giờ ghi đè lên nhau.

[!!] Sức mạnh của [ORM_Validation_Exception] có thể được tận dụng trong nhiều cách khác nhau để hợp nhất các lỗi từ các mô hình liên quan. Hãy xem một danh sách [Ví dụ](examples) cho một số lớn trường hợp sử dụng.
