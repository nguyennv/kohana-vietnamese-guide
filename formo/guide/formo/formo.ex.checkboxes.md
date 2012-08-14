# Ví dụ về nút chọn

## Thêm một nút chọn

	$hobbies = array
	(
		'run' => 'Chạy',
		'swim' => 'Bơi',
		'bike' => 'Đi xe đạp',
		'hike' => 'Đi bộ đường dài',
	);
	
	$last_selection = 'swim';

	$form
		->add_group('hobby', 'checkboxes', $hobbies, array('swim', 'hike'), array('label' => 'Sở thích'));