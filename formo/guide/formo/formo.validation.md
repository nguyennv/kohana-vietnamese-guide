# Nạp các giá trị $_POST

Các giá trị trường mới được thêm vào với phương thức `load`.
Phương thức này chấp nhận một mảng các cặp khóa => giá trị mà tương ứng với các bí danh trường.
Nếu một mảng không được truyền trong phương thức, Form nạp sử dụng mảng $_POST.

Phương thức này cũng thiết lập cho dù form được coi là "sent".
Nó được coi là đã gửi nếu bất kỳ trường nào đã có mặt trong mảng từ phương thức `load()`.

	// Câu lệnh này nạp mảng $_POST
	$form->load();

	// Câu lệnh này cũng nạp các giá trị $_POST
	$form->load($_POST);

	// Câu lệnh này nạp các giá trị $_GET
	$form->load($_GET);

	// Câu lệnh này nạp các giá trị của mảng khác
	$form->load($vals);

# Phương thức sent

Mặc dù phương thức này được sử dụng nội bộ, nó là một phương thức công cộng mà bạn có thể tìm thấy sự cần thiết.
Truyền một mảng đầu vào như tham số và nó trả về một giá trị *boolean* cho dù form đã gửi hay chưa.

Nếu một trường từ mảng đầu vào tồn tại trong form/subform, `sent()` trả về `TRUE`.

# Phương thức validate

Phương thức `validate` kiểm tra mỗi trường đối với bất kỳ quy tắc xác nhận hợp lệ mà được gán cho nó, đính kèm các lỗi tới bất kỳ trường hoặc subform mà không vượt qua, và trả về một đại diện **boolean** cho dù form đã vượt qua (`TRUE`) hoặc thất bại (`FALSE`) xác nhận hợp lệ.

Nếu form không gửi (có nghĩa là, không có bất kỳ giá trị trường nào trong mảng `load()`), `validate()` cũng sẽ trả về `FALSE`.
Nếu bạn muốn chạy xác nhận hợp lệ trên form thậm chí nếu nó không được gửi bẳng cách truyền `TRUE` như là một tham số.

Vì bạn thường sẽ kiểm tra các quy tắc xác nhận hợp lệ sau khi thêm một mảng đầu vào, `validate()` có thể sẽ được cặp với `load()`.

Dưới đây là một số ví dụ:

	// Đính kèm các thông báo lỗi và kiểm tra nếu form vượt qua xác nhận hợp lệ, và yêu cầu form được gửi.
	if ($form->load()->validate())

	// Đính kèm các thông báo lỗi và kiểm tra nếu form vượt qua, ngay cả nếu nó không được gửi
	if ($form->validate(FALSE))

# Xác nhậ hợp lệ Formo

Formo cung cấp hệ thống xác nhận hợp lệ của riêng nó thay vì thư viện Validate đã đóng gói của Kohana ngoại trừ việc nó sử dụng các phương thức hữu ích của trình trợ giúp Validate.

Xác nhận hợp lệ được dựa trên các bộ lọc, các quy tắc, các bộ kích hoạt và post_filters đã đính kèm tới forms, subforms và các trường riêng biệt.

## Ngữ cảnh xác nhận hợp lệ

Formo sử dụng xác nhận hợp lệ dựng sẵn của Kohana và dó đó tuân theo cùng các quy tắc cho các định nghĩa của nó.

Ngoài các tham số `:field`, `:value` và `:validation` thường xuyên, Formo liên kết các giá trị sau tới đối tượng xác nhận hợp lệ:

Chuỗi tham số		|	Cái được truyền
--------------------|-----------------------
`:form`				|	Đối tượng form cha
`:model`			|	Đối tượng mô hình (chỉ áp dụng khi sử dụng ORM)

## 'required' và quy tắc 'not_empty'

Lớp Validation của Kohana sử dụng một quy tắc đặc biệt để làm cho một trường là bắt buộc.
Quy tắc là `not_empty`.
Bạn có thể thêm quy tắc này giống như bất kỳ quy tắc khác tới form và các trường của bạn, nhưng bạn cũng có thể thiết lập quy tắc tương tự bằng việc sử dụng tham số tắt `required`:

	$form = Formo::form()
		->add('email', array('type' => 'email', 'required' => TRUE))

## Các quy tắc mức form/subform

Các quy tắc mà tồn tại tại mức form hoặc subform có sự khác biệt đối với các quy tắc ở mức trường như sau:

- Chúng được thêm tới form/subform bằng bí danh của chúng.
- `:value` truyền một mảng của tất cả các giá trị bên trong subform.

Một ví dụ của một quy tắc tại mức form có thể là một form đăng nhập nơi mà `username` và `password` có các quy tắc mà chúng không thể là trống, và form có một quy tắc mà các hai trường là thông tin đăng nhập chính xác với nhau.

	$form = Formo::form(array('rules' => array(array('Model_User::can_login', array(':values', 'username', 'password'))))
		->add('username', array('rules' => array(array('not_empty'))))
		->add('password', 'password', array('rules' => array(array('not_empty'))))
		->add('submit', 'submit');

	if ($form->load($_POST)->validate())
	{

	}

Trong ví dụ này, các trường `username` và `password` trước tiên yêu cầu không được rỗng, sau đó quy tắc form đăng nhập sẽ chạy sau khi hai trường khác đã vượt qua.

Ví dụ quy tắc xác nhận hợp lệ trong `Model_User`:

	public static function can_login($values, $username, $password)
	{
		if (Auth::instance()->login($values[$username], $values[$password]))
			return TRUE;

		return FALSE;
	}