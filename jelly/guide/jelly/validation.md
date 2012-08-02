# Xác nhận hợp lệ

Xác nhận hợp lệ sẽ lo việc kiểm tra dữ liệu được gửi tới cơ sở dữ liệu.
Nó là một chuyển đổi trực tiếp gần của xác nhận hợp lệ của Kohana ORM với vài khác biệt nhỏ.
Để có dữ liệu của bạn được xác nhận bạn sẽ phải thêm các quy tắc tới các trường khi định nghĩa mô hình.

Có ba loại khác nhau của các quy tắc mà bạn có thể sử dụng khi xác nhận hợp lệ:

-   [quy tắc xác nhận Kohana](../kohana/security/validation#default-rules) dựng sẵn.
-   bất kỳ hàm PHP hiện có
-   callback tuỳ biến

## Thêm các quy tắc
Để thêm các quy tắc hãy tạo một mảng `rules` khi định nghĩa trường.

	// ...
	'name' => Jelly::field('string', array(
		// Mảng cho các quy tắc
		'rules' => array(),
	)),

### Định nghĩa các quy tắc dựng sẵn
Khi thêm các quy tắc dựng sẵn mà chỉ chấp nhận giá trị của trường tất cả bạn phải làm là thiết lập tên của quy tắc:
`array('name_of_the_rule')`

	// ...
	'name' => Jelly::field('string', array(
		'rules' => array(
			// Thêm quy tắc mà chỉ chấp nhận giá trị trường
			array('not_empty'),
		),
	)),

Điều này tương tự như việc gọi hàm giống như thế này: `Valid::not_empty('value_of_the_field')`

Việc thêm các quy tắc dựng sẵn mà chấp nhận các tham số đòi hỏi bạn định nghĩa các quy tắc giống như thế này:
`array('name_of_the_rule', array('value_of_the_field', 'parameter_for_the_rule'))`

	// ...
	'name' => Jelly::field('string', array(
		'rules' => array(
			// Thêm quy tắc mà chấp nhận các tham số
			array('max_length', array(':value', 32)),
		),
	)),

Điều này tương tự như việc gọi hàm giống như thế này: `Valid::max_length('value_of_the_field', 32)`

[!!] Lưu ý: ký tự đại diện `:value` sẽ được tự động thay thế với giá trị của trường, đó là cách bạn truyền nó tới các phương thức khác, trong trường hợp này là tới `Valid::max_length`.

### Định nghĩa các hàm PHP hiện có như các quy tắc
Bạn có thể sử dụng các hàm PHP hiện có để xác nhận dữ liệu của bạn.
Khi thêm một quy tắc giống như thế này chỉ thực hiện giống như ví dụ trên cho các hàm dựng sẵn.
Các hàm mà chỉ cần giá trị trường được định nghĩa giống như thế này:
`array('name_of_the_function')`

	// ...
	'name' => Jelly::field('string', array(
		'rules' => array(
			// Thêm quy tắc mà chỉ chấp nhận giá trị trường
			array('is_writable'),
		),
	))

Điều này tương tự như việc gọi hàm giống như thế này: `is_writable('value_of_the_field')`

Các hàm mà chấp nhận các tham số được định nghĩa giống như cách này:
`array('name_of_the_function', array('value_of_the_field', 'parameter_for_the_function'))`

	// ...
	'name' => Jelly::field('string', array(
		'rules' => array(
			// Thêm quy tắc mà chấp nhận các tham số
			array('mb-check-encoding', array(':value', 'UTF-8')),
		),
	)),

Điều này tương tự như việc gọi hàm giống như thế này: `is_writable('mb-check-encoding', 'UTF-8')`

### Định nghĩa các callback tuỳ biến như các quy tắc
Bạn có thể sử dụng bất kỳ loại callback tuỳ biến đã định nghĩa như một quy tắc.
Khi định nghĩa các callback tuỳ biến như các quy tắc hãy nghĩ về chúng như thế này:

`array('the_method_to_call', ('parameters_for_the_method'))`

__'the_method_to_call' có thể được thiết lập trong những cách sau::__

 -   `'Class::static_method'`
 -   `array('Class::static_method', 'dynamic_method_name')`
 -   `array(':model', 'method_name')`
 -   `array(':field', 'method_name')`


[!!] Việc sử dụng ký tự đại diện `:model` khi định nghĩa phần `'the_function_to_call'` của quy tắc có nghĩa là hàm được định nghĩa bên trong mô hình. Ký tự đại diện `:field` được thay thế với đối tượng trường, cái mà hữu ích khi bạn định nghĩa một trường tuỳ biến và định nghĩa callback của bạn bên trong nó.

__Trong phần 'parameters_for_the_method' bạn có thể truyền các giá trị này:__

 -   giá trị thực sự cho tham số
 -   `:value`
 -   `:field`
 -   `:model`
 -   `:validation`

[!!] Như bạn đã biết ký tự đại diện `:value` được thay thế với giá trị của trường. Ký tự đại diện :field được trao đổi với tên của trường, trong khi `:model` được thay thế với đối tượng mô hình, và ký tự đại diện `:validation` với đối tượng xác nhận hợp lệ.

Một ví dụ sẽ là một xác nhận của các tập tin tải lên.
`Jelly_Field_File` có một callback được đặt tên `_upload`:

	public function _upload(Validation $validation, $model, $field)
	{
		if ($validation->errors())
		{
			// Hiện đã có lỗi, không tiếp tục
			return FALSE;
		}

		// Nhận được hình ảnh từ đối tượng xác nhận
		$file = $validation[$field];

		if ( ! is_array($file) OR ! Upload::valid($file) OR ! Upload::not_empty($file))
		{
			return FALSE;
		}

		// Kiểm tra xem nếu nó là một kiểu hợp lệ
		if ($this->types AND ! Upload::type($file, $this->types))
		{
			// Thêm một lỗi với tên của trường
			$validation->error($field, 'Upload::type');
			return FALSE;
		}

		// V.v...
	}

Để có callback này làm việc trường hãy định nghĩa một quy tắc theo cách này:
`array(array(':field', '_upload'), array(':validation', ':model', ':field'))`

Điều này dịch thành: "tìm một phương thức trong đối tượng của trường này đã gọi '_upload' và truyền các đối tượng xác nhận và mô hình, và tên của trường".

[!!] Lưu ý đến phần mà thêm một lỗi khi chạy vào các vấn đề. Đây là cách bạn có thể thêm các lỗi của riêng bạn và điều này là cần thiết khi sử dụng các callback tuỳ biến.

## Kiểm tra nếu mô hình là hợp lệ

Việc xác nhận hợp lệ được thực hiện tự động khi gọi `save()`.
Bạn nên mong đợi một [Jelly_Validation_Exception](../api/Jelly_Validation_Exception) được ném ra:

	try
    {
        $user = Jelly::factory('user');
        $user->username = 'tên truy cập không hợp lệ';
        $user->save();
    }
    catch (Jelly_Validation_Exception $e)
    {
        // Nhận các thông báo lỗi
        $errors = $e->errors();
    }

### Thông báo lỗi

Khi bạn nhận các thông báo lỗi giống ví dụ ở trên (`$e->errors()`) bạn có thể truyền hai tham số như bạn có thể với `Validation::errors()`.
Sự khác biệt duy nhất là tham số thứ nhất không là tên tập tin, nó là tên một thư mục mà sẽ được nối thêm tới tên của mô hình cho việc tạo một đường dẫn thư mục cho các thông báo lỗi.

Vì vậy, sử dụng ví dụ ở trên bạn truyền `jelly-validation` như tên thư mục giống như thế này: `$e->errors('jelly-validation')`.
Điều đó có nghĩa bạn đã định nghĩa đường dẫn này: `application/messages/jelly-validation/user.php`, và bạn sẽ tạo tập tin đó để truy xuất các thông báo lỗi.

Tham số thứ hai bạn có thể truyền tới phương thức này giống hệt tham số thứ hai `Validation::errors()` và nó liên quan với bản dịch của các thông báo lỗi.

### Xác nhận hợp lệ bên ngoài

Một vài mẫu biểu chứa thông tin mà không được xác nhận bởi mô hình, nhưng xác nhận bằng điều khiển.
Thông tin như một dấu hiệu [CSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery), xác minh mật khẩu, hoặc một [CAPTCHA](http://en.wikipedia.org/wiki/CAPTCHA) sẽ không bao giờ được xác nhận bằng một mô hình.
Truy nhiên, thông tin xác nhận trong nhiều nơi và việc kết hợp các lỗi để cung cấp cho người dùng với với môt kinh nghiệm tốt thường khá buồn tẻ.
Ví lý do này, lớp [Jelly_Validation_Exception] được xây dựng để xử lý nhiều đối tượng Validation và không gian tên mảng các lỗi tự động cho bạn.
`Jelly::save()` tất cả có một tham số tuỳ chọn đầu tiên cái mà là một đối tượng [Validation] để xác nhận cùng với mô hình.

	public function action_create()
	{
		try
		{
			$user = Jelly::factory('user');
			$user->username = $_POST['username'];
			$user->password = $_POST['password'];

			$extra_rules = Validation::factory($_POST)
				->rule('password_confirm', 'matches', array(
					':validation', ':field', 'password'
				));

			// Truyền các quy tắc bổ xung để được xác nhận với mô hình
			$user->save($extra_rules);
		}
		catch (Jelly_Validation_Exception $e)
		{
			$errors = $e->errors('models');
		}
	}

Bởi vì đối tượng xác nhận được truyền như một tham số tới mô hình, bất kỳ lỗi nào được tìm thấy trong kiểm tra đó sẽ được được vào không gian tên trong một mảng con gọi là `_external`.
Mảng của lỗi sẽ trông giống như thế này:

	array(
		'username'  => 'Trường này không thể để trống.',
		'_external' => array(
			'password_confirm' => 'Các giá trị bạn đã nhập trong các trường mật khẩu không khớp.',
		),
	);

Điều này đảm bảo rằng các lỗi từ nhiều đối tượng xác nhận và mô hình sẽ không bao giờ ghi đè lên nhau.

### Bỏ qua xác nhận

Trong các tình huống nhất định - chẳng hạn như việc lưu một người dùng trong cơ sở dữ liệu với thông tin từng phần sau khi đăng kỳ thông qua một nhà cung cấp OAuth - bạn có thể cận bỏ qua các quy tắc xác nhận đã định nghĩa trong mô hình

Để đạt được điều này hãy truyền `FALSE` tới `Jelly::save()` cái mà sẽ bỏ qua quá trình xác nhận và lưu bất cứ dữ liệu mô hình nào được gửi.

Nếu bạn đã định nghĩa bất kỳ [bộ lọc](filters) nào trong mô hình chúng sẽ vẫn chạy.
Đừng quên xác nhận dữ liệu bằng cách sử dụng xác nhận của Kohana nếu bạn cần.

	public function action_create_twitter()
	{
		$user = Jelly::factory('user');
		$user->username = $_POST['twitter_id'];

		$user->save(FALSE);
	}