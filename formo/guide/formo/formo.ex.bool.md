# Ví dụ bool

## Thêm một hộp chọn - checkbox boolean

	$form->add('agree', 'bool', $checked, array('label' => 'Tôi đồng ý với các điều khoản và điều kiện'));

## Tạo một hộp chọn đã chọn

	$form->add('agree', 'bool', 1, array('label' => 'Make a boolean checkbox pre-checked'));