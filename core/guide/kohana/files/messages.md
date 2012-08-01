# Thông điệp

Kohana có một hệ thống tra cứu dựa vào khoá mạnh mẽ để bạn có thể định nghĩa các thông điệp hệ thống.

## Nhận một thông điệp

Sử dụng phương thức Kohana::message() để nhận một khoá thông điệp:

	Kohana::message('forms', 'foobar');

Câu lệnh này sẽ tìm kiếm trong tập tin `messages/forms.php` cho khoá foobar:

	<?php
	
	return array(
		'foobar' => 'Hello, world!',
	);

Bạn cũng có thể tìm kiếm trong các thư mục con và các khoá con:

	Kohana::message('forms/contact', 'foobar.bar');

Câu lệnh này sẽ tìm trong `messages/forms/contact.php` cho khoá `[foobar][bar]`:

	<?php
	
	return array(
		'foobar' => array(
			'bar' => 'Hello, world!',
		),
	);

## Lưu ý

 * Không sử dụng __() trong các tập tin thông điệp của bạn, vì những tập tin này có thể được lưu bộ nhớ đệm và sẽ không làm việc đúng cách.
 * Các thông điệp được hợp nhất bởi hệ thống tập tin phân cấp, không ghi đè giống như các tập tin lớp và khung nhìn.
