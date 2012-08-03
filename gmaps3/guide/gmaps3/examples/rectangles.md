# Hình chữ nhật
	
	$map = Gmaps3::instance();
		
	$map->set('default_lat', 46.164614)
		->set('default_lon', 24.082031)
		->set('default_zoom', 6)
		->set('default_type', 'HYBRID');	// Thiết lập chế độ xem bản đồ lai
			
	// Transilvania 
	$map->add_rectangle(47.343136, // Vĩ độ bắt đầu
						20.994141, // Kinh độ bắt đầu
						45.15115,  // Vĩ độ kết thúc
						25.208374, // Kinh độ kết thúc
						'#000000', // Màu Stroke
						1,		   // Trọng lượng Stroke
						0,		   // Độ trong suốt Stroke (0 = Hidden)
						0.35,	   // Độ trong suốt tô màu
						'#FF00FF'  // Màu tô
						);
	
				
	$template = View::factory('gmaps')
					->set('external_scripts', $map->get_apilink())
					->set('internal_scripts', $map->get_map('map_container'))
					->render();	
							
	$this->response->body($template);				
			
