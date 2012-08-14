# Sử dụng các tập tin thông báo tùy biến

Theo mặc định, tập tin thông báo `validation.php` được sử dụng.
Nhưng bạn có thể xác định tập tin thông báo cụ thể để sử dụng cho bất kỳ form, subform nào hoặc thậm chí một trường cụ thể.

	$form->set('message_file', 'somefile');
	$form->{$some_field}->set('message_file', 'anotherfile');

	// Và như mọi khi, việc tạo mảng là tùy chọn
	$form = Formo::form(array('message_file' => $filename));