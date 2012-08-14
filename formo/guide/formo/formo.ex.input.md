# Ví dụ hộp nhập text

## Thêm một hộp nhập text

Các hộp nhập text là kiểu điều khiển mặc định. Kiểu 'text' không là bắt buộc.

	$form->add('username');

## Các hộp nhập text HTML 5

Việc này được xác định bởi thuộc tính `type`, giống như trong html.

	$form->add('website', array('type' => 'url'));