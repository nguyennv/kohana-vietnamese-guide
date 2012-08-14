Các phương thức mà liên quan đến việc xây dựng một đối tượng mới theo một cú pháp tương tự: `param, param2, param3, v.v, options_array`.

Trong kịch bản này, nghĩ tới bất kỳ tham số trước khi mảng `$options` cuối cùng như các tham số tiện lợi.
Tại bất kỳ điểm nào bạn có thể nhập một mảng tùy chọn mà chứa tất cả tham số.

Ví dụ, hãy xem xét hàm `add()`.
Các tùy chọn của nó là sau đây: `add(alias, driver, value, options)`.

Lựa chọn sau đây là giống nhau

	$attr = array('height' => '23px');
	$form->add('username', 'input', $model->username, array($attr));
	
	$options = array('value' => $model->username, $attr);
	$form->add('username', 'text', $options);
	
	$options = array('driver' => 'input', 'value' => $model->username, $attr);
	$form->add('username', $options);
	
	$options = array('alias' => 'username', 'value' => $model->username, 'driver' => 'input', $attr);
	$form->add($options);
	
Đây là trường hợp với tất cả cách xây dựng của Formo.