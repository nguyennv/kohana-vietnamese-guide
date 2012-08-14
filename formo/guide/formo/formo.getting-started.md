Bắt đầu
===============

Tạo một form bằng cách sử dụng phương thức `form`:

	$form = Formo::form();

Nói chung bàn sẽ tạo một đối tượng form và các trường `add` tới nó:

	$form = Formo::form()
		->add('username')
		->add('email');

## Hàm dựng và mảng $options

Để hiểu rõ thêm về hàm dựng của Formo, xem [Tài liệu hàm dựng](formo.constructs).

## Phương thức add

Phương thức `add` có các tham số `alias, driver, value, options`.
Nếu trình điều khiển được bỏ qua, trình điều khiển mặc định là bất cứ gì mà bạn chỉ định trong tập tin cấu hình (mặc định là "text").
Ngoài ra, đối với hầu hết các phương thức Formo, bạn chỉ đơn giản truyền một mảng `options` như tham số đầu tiên hoặc thứ hai.
Như vậy, ví dụ sau đây là giống nhau:

	$form->add('notes', 'textarea', 'Ghi chú của tôi');
	$form->add('notes', array('driver' => 'textarea', 'value' => 'Ghi chú của tôi'));
	$form->add(array('alias' => 'notes', 'driver' => 'textarea', 'value' => 'Ghi chú của tôi'));

Đây là mẫu chung cho các phương thức mà có tham số `options` cuối cùng.

### Các trường truy cập và bí danh

Mỗi trường được *thêm* tới đối tượng `$form` mà được tham chiếu bởi một bí danh.
Các bí danh cho form được định nghĩa ở trên từ tham số `name`.
Tham số này cũng sẽ trở thành thuộc tính tên cho một trường.

Bạn sẽ cần truy cập tới các trường cụ thể.
Chúng sẽ được trả về bằng bí danh của trường hoặc thứ tự số nguyên của nó
Ba ví dụ này sẽ trả về cùng một trường:

	$form
		->add('subject', 'text')
		->add('notes', 'textarea');

	$form->notes;
	$form->{'notes'};
	$form->{1};

Đôi khi bạn có thể muốn sử dụng một số như là một bí danh của trường.
Để lấy trường bởi bí danh số của nó, gửi chuỗi phiên bản của số trong phương thức `__get`:

	$form->add(23, 'text', array(stuff));

	$form->{'23'};

[!!] Các bí danh form, subform và trường luôn luôn thay thế khoảng trắng bằng dấu gạch dưới. Như vậy, bí danh `my field` và `my_field` trỏ đến cùng một đối tượng trường.

Hai trường này là giống nhau:

	$form->add('my field', 'text');
	$form->add('my_field', 'text');

Chúng có thể được truy cập bởi:

	$form->{'my field'};	
	$form->{'my_field'};

Tóm lại, chuỗi trả về bởi bí danh, số nguyên trả về bởi khóa, `__get()` trả về một trường bởi bí danh và khoảng trắng của bí danh được chuyển đổi thành gạch dưới.

### Truy cập và thiết lập các biến

Để bảo vệ cú pháp đơn giản cho làm việc với các subform và các trường bên trong formo, các biến dựng sẵn đòi hỏi sử dụng các hàm để nhận và thiết lập.

#### Phương thức get

Phương thức này trả về một biết bên trong bấn kỳ đối tượng Container nào.

	$form->get('driver');

Bạn cũng có thể chỉ định một giá trị mặc định nếu biến không tồn tại.

	$form->get('my_variable', array());

#### Phương thức set

Để thiết lập các biến bên trong một đối tượng Container, hãy sử dụng phương thức `set`.

	$form->set('driver', 'group');

Bạn cũng có thể truyền một mảng khóa => giá trị để thiết lập.

	$settings = array('driver' => 'group', 'foo' => 'bar');

	$form->set($settings);

### Các trường Formo

Các trường là các trình chứa cho dữ liệu.
Mọi trường bạn thêm tới đối tượng form của bạn là một đối tượng `Formo_Field`.

### Subforms

Các subform được thêm tới một form giống như một trường.
Chúng là các đôi tượng Formo mà cũng chứa các trường bên trong chính chúng.
Nói riêng, các subform có chức năng giống hệt các form.

Các trường bên trong một subform khi được kết xuất dưới dạng HTML.

Để biết thêm về subform, xem [phần subform](formo.subforms).

### Phương thức find

Phương thức này tìm kiếm thông qua nhiều lớp form và trả về một trường hoặc subfield bởi bí danh của nó
Hoặc bạn có thể xác định một cách chính xác nơi mà trường này là bằng các sử dụng một mảng mà xác định chính xác nơi mà mục bạn đang tìm kiếm.

$subform2 = Formo::form()
	->add('bar', 'text');

$subform = Formo::form()
	->add('foo', 'text')
	->add('subform2', $subform2);

$form = Formo::form()
	->add('subform', $subform);

// Các lệnh này sẽ trả về cụng một trường 'bar'
$form->find(array('subform', 'subform2', 'bar'));
$form->find('bar');

### Kết xuất

Khi bạn kết xuất một đối tượng Formo, đối được sẽ được chuyển đổi về dữ liệu thuần túy cho một đối tượng có thể sử dụng.
Ví dụ, nếu bạn muốn kết xuất form của bạn như html bằng cách sử dụng các tập tin đã được định nghĩa, hãy làm:

	$form->render();

Ví dụ này sẽ chuyển đổi mọi trường vào trong một đối tượng HTML DOM và đối tượng đó sẽ được gửi tới các tập tin khung nhìn đã định nghĩa của chúng.

Xem thêm về kết xuất trong [phần kết xuất](formo.rendering)
