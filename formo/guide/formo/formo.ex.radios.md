# Ví dụ nhóm nút radio

## Thêm một nhóm nút radio

	$hobbies = array
	(
		'run' => 'Chạy',
		'swim' => 'Bơi',
		'bike' => 'Đi xe đạp',
		'hike' => 'Đi bộ đường dài',
	);
	
	$last_selection = 'swim';

	$form
		->add_group('hobby', 'radios', $hobbies, 'swim', array('label' => 'Sở thích'));