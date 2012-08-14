# Làm việc với tập tin

Các trường tập tin có chức năng giống như bất kỳ các trường Formo khác ngoại trừ giá trị của nó luôn được lấy từ mảng `$_FILES`.

Bạn cũng sẽ dùng các quy tắc Upload của Kohana thay vì các quy tắc của Validate.

[!!] Khi làm việc với các form HTML, form cha của trường được tự động thêm vào *enctype="multipart/form-data"*, nhưng chỉ cho form cha trực tiếp của trường

[!!] $_FILES không thể được đặt trong không gian tên, do đó không là các trường tập tin

### Ví dụ

Đây là một ví dụ của một việc thêm một trường input tập tin mà phải là một hình ảnh nhỏ hơn 1mb

	$form = Formo::form()
		->add('logo', 'file')
		->rules('logo', array(
			'Upload::not_empty' => NULL,
			'Upload::type'      => array(':value', 'PNG, PNG or GIF' => array('jpg', 'png', 'gif')),
			'Upload::size'      => array(':value', '1M'),
		));
		
	if ($form->load()->validate())
	{
		// Đây là dữ liệu thô từ mảng $_FILES[$filename]
		$file_data = $form->logo->val();

		// Làm điều gì đó với $file_data
	}