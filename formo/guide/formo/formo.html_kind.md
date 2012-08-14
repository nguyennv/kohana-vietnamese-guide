Loại HTML
=============

Bạn có khả năng sẽ sử dụng khung nhìn HTML trong các form Formo.
Đây là một cái nhìn tổng thể của chức năng nó thêm tới các đối tượng Formo.
Đối tượng khung nhìn được truyền vào bên trong tập tin khung nhìn như biến `$this` và truy cập ở khắp mọi nơi nào với `view()`.

	$view_obj = $form->username->view();

### Mở

Bạn thường sẽ phải cần kết xuất thẻ mở của đối tượng HTML DOM chỉ hoàn thành với các thuộc tính và style.
Trong khung nhìn:

	echo $this->open();

### Đóng

Tương tự như vậy, bạn thường sẽ chỉ cần kết xuất thẻ đóng của đối tượng.
Trong khung nhìn:

	echo $this->close();

### Kết xuất

Để kết xuất đối tượng HTML DOM, sử dụng `html()`.
Trong khung nhìn:

	echo $this->html();

Thông tin thêm về việc kết xuất form, xem [phần kết xuất](formo.rendering);

### Kết xuất

Để kết xuất một tập tin khung nhìn bằng việc sử dụng một đối tượng form hoặc trường, hãy sử dụng `render()`:

	echo $field->render();

Thông tin thêm về kết xuất form, xem [phần kết xuất](formo.rendering);

### Thuộc tính

Các thuộc tính html như `style="height:25px" class="someclass" id="something"`, v.v. dễ dàng được truy cập với việc trang trí HTML thông qua các phương thức `attr()`, `css()`, `add_class()`, và `remove_class()`.

#### Các thuộc tính non-class, non-style

Để thiết lập một thuộc tính, hãy sử dụng `attr($attr_name, $attr_value)`:

	$field->view()->attr('method', 'post');

Để nhận một thuộc tính, hãy sử dụng `attr($attr_name)`:

	$method = $field->view()->attr('method');

Nếu bạn muốn loại bỏ một thuộc tính, hãy thiết lập nó thành `NULL`:

	$field->view()->attr('id', NULL);

Nếu bạn muốn thuộc tính vẫn còn nhưng trống rỗng, hãy sử dụng một chuỗi rỗng:

	$field->view()->attr('action', '');

Bạn có thể thiết lập nhiều thuộc tính tại một thời điểm với một mảng:

	$field->view()->attr(array(
		'method' => 'post',
		'action' => '',
		'id' => 'my-special-form',
	));

#### Thuộc tính style

Để thiết lập style cho một phần tử HTML DOM, sử dụng `css($style_name, $style_value)`:

	$form->email->view()->css('width', '300px');

Để nhật một style, sử dụng `css($style_name)`:

	$width = $form->username->view()->css('width');

Để loại bỏ một style, thiết lập nó thành `NULL`:

	$form->password->view()->css('font-weight', NULL);

Bạn có thể thiết lập nhiều style tại một thời điểm với một mảng:

	$form->email->view()->css(array(
		'width' => '200px',
		'background-color' => '#eee',
		'border' => '1px solid #999,
	));

#### Thuộc tính class

Bạn có thể thiết lập một class hoặc nhiều class bằng cách sử dụng `attr()`, nhưng nó sẽ ghi đè bất cứ class nào mà bạn đã thiết lập trước đó:

	$field->view()->attr('class', 'oneclass twoclass');

Sử dụng `add_class($classname)` để thêm một class:

	$form->email->view()->add_class('email');

Bạn có thể thiết lập nhiều class tại một thời điểm với hoặc một mảng hoặc các tên class phân tách bởi khoảng trống:

	$form->email->view()->add_class('email specialfield');
	$form->email->view()->add_class(array('email', 'specialfield'));

Sử dụng `remove_class($classname)` để loại bỏ một class:

	$form->username->view()->remove_class('sucks');

Bạn có thể loại bỏ nhiều class tại một thời điểm với hoặc một mảng hoặc các tên class phân tách bởi khoảng trống:

	$form->username->view()->remove_class('sucks specialfield');
	$form->username->view()->remove_class(array('sucks', 'specialfield'));

### Nhãn

Sử dụng `label()` để nhận nhãn của một trường.
Phương thức này sẽ trả về một biến `label` nhếu nó đã được thiết lập, và `alias` nếu `label` không được thiết lập.

	$label = $field->view()->label();

### Text

Bạn có thể thiết lập text bên trong một đối tượng HTML với `text($text)`:

	$form->comment->view()->text('Nhập chú thích của bạn ở đây');

Bạn có thể loại bỏ text bằng cách thiết lập nó thành `NULL` hoặc là một chuỗi trống:

	$form->comment->view()->text(NULL);

Nhận text của đối tượng với `text()`:

	$comment_text = $field->text();

#### Thao tác text cơ bản

Để thêm một chuỗi vào cuối của text bên trong đối tượng, hãy sử dụng `text('.=', $string)`:

	$field->view()->text('.=', ' - I meant what I said');

Để thêm một chuỗi vào đầu text bên trong một đối tượng, hãy sử dụng `text('=.', $string)`:

	$field->view()->text('=.', 'PAY ATTENTION TO THIS: ');

Để chạy một callback trên một text bên trong đối tượng, hãy sử dụng `text('callback', $callback)`:

	$field->view()->text('callback', 'trim');
