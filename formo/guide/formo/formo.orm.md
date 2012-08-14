Làm việc với mô-đun ORM chính thức của Kohana
=============================================

Bạn có thể là việc trực tiếp với các trường mô hình ORM bên trong Formo thông qua giao tiếp `orm()`.
Giao tiếp này chạy một phương thức trên một đối tượng orm.

### Nạp

Để nạp các trường từ một mô hình, hãy dùng `orm('load',  $model)`.
Ví dụ, nếu bạn có một đối tượng `$user` và muốn chuyển đổi bản ghi thành một form Formo, bạn có thể chỉ đơn giản là làm như thế này:

	$user = ORM::factory('user', 20);
	
	$form = Formo::form()
		->orm('load', $user);
		
Phương thức `load` sẽ nhận biết tất cả các quy tắc bên trong định nghĩa mô hình ORM  và chũng cũng sẽ mang theo vào bên trong form như các quy tắc xác nhận hợp lệ.

### Các quy tắc trong mô hình

Bạn chỉ nên xác định các quy tắc áp dụng tới mô hình bên trong mô hình, và bất kỳ quy tắc nào khác có thể được liên kết với một trường form bên trong Formo.
Để xác định các quy tắc bên trong mô hình, hãy sử dụng phương thức `rules()` như bình thường.

Bất kỳ quy tắc nào bên trong mô hình sẽ mang theo vào form.

### Định nghĩa Formo trong mô hình

Bởi vì nó là thực hành tốt nhất để làm việc một lần, các định nghĩa trường cụ thể cho mô hình đã chuyển đổi tới các form Formo, rất có thể bạn sẽ muốn khai báo các định nghĩa Formo cụ thể bên trong mô hình.

Phương thức để thực hiện các định nghĩa này trong mô hình là `formo()`.
Bạn có thể trả về một mảng của bất kỳ thiết lập nào bên trong phương thức này.

	// Bên trong mô hình
	// Chú ý khai báo phải là public
	public function formo()
	{
		return array
		(
			'user_tokens' => array
			(
				'render' => FALSE,
			),
			'notes' => array
			(
				'driver' => 'textarea'
			),
		);
	}
	
	
### Luồng sử dụng trình điều khiển với các bản ghi

Các luồn nói chung thường như thể này
Trước tiên bạn tạo bản ghi và nạp các giá trị và các quy tắc của nó vào form.
Bản ghi này cũng có thể là một bản ghi trống, chưa được lưu.

	$user = ORM::factory('user');
	
	$form = Formo::form()
		->orm('load', $user);

Bất kỳ định nghĩa form cụ thể nào được thêm:

	$form
		->add('confirm', 'password', array('rules' => array(array('matches', array(':validation', 'confirm', 'password')))))
		->add('save', 'submit');

Form được xác nhận hợp lệ và bản ghi được lưu lại
	
	if ($form->load($_POST)->validate())
	{
		$user->save();
	}

[!!] Lưu ý rằng các giá trị được nạp tới mô hình trong phương thức `load()`.

### Quan hệ has many through và has one

Hiện thời, ORM của Kohana tự đong thêm và loại bỏ các quan hệ, phương thức moment `add()` và `remove()` được chạy và do đó các bản ghi trống ngắt khi làm việc này.

Bởi vì nhược điểm này của ORM, Form sẽ giấu các thay đổi quan hệ bên trong trình điều khiển ORM của nó, và bạn phải chạy phương thức `'save_rel'` sau khi lưu mô hình.

	if ($form->load($_POST)->validate())
	{
		$user->save();
		$form->orm('save_rel', $user);
	}

### Chỉ lấy một số trường từ mô hình vào trong form

Có hai tham số tùy chọn  trong phương thức `Formo::orm('load')`. Đó là:

1. Một mảng các bí danh trường
2. Một cờ boolean cho dù các bí danh đó là các trường để bỏ qua (mặc định là `FALSE`)

Nếu bạn liệt kê một mảng của các trường trong tham số tùy chọn đầu tiên, chỉ có các trường đó sẽ được lấy từ mô hình vào trong form.
Nhưng nếu bạn thiết lập cờ `skip_fields` thành `TRUE`, các trường đó xác định trong tham số tùy chọn đầu tiên trở thành một danh sách các trường để bỏ qua.

#### Ví dụ của việc lấy một số trường

Cách để lấy các trường `username` và `password`:

	$user = ORM::factory('user', 20);
	
	$form = Formo::form()
		->orm('load', $user, array('username', 'password'));

Cách để lấy mọi trường trừ `password` và `email`:

	$user = ORM::factory('user', 20);
	
	$form = Formo::form()
		->orm('load', $user, array('password', 'email'), TRUE);