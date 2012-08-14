# Ví dụ hộp chọn

## Thêm một hộp chọn

	$hobbies = array
	(
		'run' => 'Chạy',
		'swim' => 'Bơi',
		'bike' => 'Đi xe đạp',
		'hike' => 'Đi bộ đường dài',
	);

	$form
		->add_group('hobby', 'select', $hobbies, 'swim', array('label' => 'Sở thích'));
		
## Với optgroups

	$activities = array
	(
		'Exercise' => array
		(
		'run' => 'Chạy',
		'swim' => 'Bơi',
		'bike' => 'Đi xe đạp',
		'hike' => 'Đi bộ đường dài',
		),
		'Dates' => array
		(
			'movie' => 'Phim ảnh',
			'rollerskating' => 'Trượt patin',
			'dinner' => 'Ăn tối',
			'bowling' => 'Chơi bowling',
		)
	);
	
	$form->add_group('activity', 'select', $activities, $initial_value);