# Ví dụ datalist

## Tạo một datalist

	$form->add('cars', 'datalist', array(
		'id' => 'cars',
		'options' => array('BMW', 'Mercedes', 'Honda', 'Ford'),
	));

[!!] Một `id` và `options` là bắt buộc cho datalist làm việc