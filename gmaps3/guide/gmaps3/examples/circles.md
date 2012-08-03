# Vòng tròn
	
	$map = Gmaps3::instance();
		
	$map->set('default_lat', 48.014689)
		->set('default_lon', 1.828888)
		->set('default_zoom', 4);				
			
	// Uh la la
	$map->add_circle(48.859068,	 // Vĩ độ 
					  2.346954,	 // Kinh độ
					  200000, 	 // Bán kính
					  '#2080D0', // Màu Stroke
					  2, 		 // Trọng lượng Stroke
					  1, 		 // Độ trong suốt Stroke
					  0.5, 		 // Độ trong suốt tô màu
					  '#C0E0FF'  // Màu tô
					);	 
	
	// Điểm cuối của trái đất
	$map->add_circle(42.877788, -9.265423, 50000, '#00FF00');
	
	
	$template = View::factory('gmaps')
				->set('external_scripts', $map->get_apilink())
				->set('internal_scripts', $map->get_map('map_container'))
				->render();	
							
	$this->response->body($template);
			
