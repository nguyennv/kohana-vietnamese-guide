# Infowindows
	
		
	$map = Gmaps3::instance();			
 		
	// Đính kèm infowindow cho nhóm mặc định tới điểm đánh dấu này
	$map->add_mark(56.177668, 10.103302, 'Mark 1')
				->add_infowindow('Infowindow 1', FALSE);
		                
	// Đính kèm infowindow tự động mở cho nhóm mặc định tới điểm đánh dấu này
	$map->add_mark(55.928433, 8.595428, 'Mark 2')
				->add_infowindow('Infowindow 2', TRUE);	
		
	// Đính kèm infowindow cho nhóm tùy biến "MyGroup2" tới điểm đánh dấu này
	$map->add_mark(55.663257, 13.364182, 'Mark 3', FALSE,
				'http://maps.google.com/mapfiles/ms/micons/yellow-dot.png')
				->add_infowindow('Infowindow 3', FALSE, 'MyGroup2'); 
	
	// Đính kèm infowindow cho nhóm tùy biến "MyGroup3" tới hình chữ nhật này
	$map->add_rectangle(53.608803, // Begin Lat
						10.475464, // Begin Lon
						54.660916, // End Lat
						11.412292, // End Lon
						'#2080D0', // Stroke color
						2,		   // Stroke weight
						1,		   // Stroke opacity (0 = Hidden)
						0.5,	   // Fill opacity
						'#C0E0FF'  // Fill color
						)->add_infowindow('Infowindow 4', FALSE, 'MyGroup3');
	
	// Đính kèm infowindow tự động mở cho nhóm tùy biến "MyGroup3" tới vòng tròn này
	$map->add_circle(54.367759,	// Lat
					 12.722168, // Lon
					 50000,     // Radius
					 '#2080D0', // Stroke color
					 2,         // Stroke weight
					 1,         // Stroke opacity
					 0.5,       // Fill opacity
					 '#C0E0FF'  // Fill color
					 )->add_infowindow('Infowindow 5', TRUE, 'MyGroup3');
	
	// Đặt ở trung tâm và làm cho phù hợp vị trí bản đồ trong mối quan hệ với tất cả thành phần
	$map->center_all();
					
	$template = View::factory('gmaps')
					->set('external_scripts', $map->get_apilink())
					->set('internal_scripts', $map->get_map('map_container'))
					->render();	
							
	$this->response->body($template);					
			
