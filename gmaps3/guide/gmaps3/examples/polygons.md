# Đa giác

	$map = Gmaps3::instance();
		
	// Thiết lập lat, lon và zoom mặc định
	$map->set('default_lat', 24.886436490787712)
		->set('default_lon', -70.2685546875)
		->set('default_zoom', 4);				
	
	// Định nghĩa một nhóm đa giác
	$map->add_polygon_group('BermudasTriangle',	// Tên nhóm
							'#FF00FF',			// Màu Stroke 
							3,					// Trọng lượng Stroke 
							1,					// Độ trong suốt Stroke (Từ 0.0 tới 1.0)
							0.5,				// Độ trong suốt tô màu (Từ 0.0 tới 1.0) 
							'#FFF000'			// Màu tô
							);
	
	// Định nghĩa tọa độ cho nhóm BermudasTriangle
	$map->add_coord('BermudasTriangle', 25.774252, -80.190262);
	$map->add_coord('BermudasTriangle', 18.466465, -66.118292);
	$map->add_coord('BermudasTriangle', 32.321384, -64.75737);
	$map->add_coord('BermudasTriangle', 25.774252, -80.190262);
	
	// Bạn có thể sử dụng add_polygon_group và add_coord như phương thức xâu chuỗi
	$map->add_polygon_group('MyTriangle')
			->add_coord('MyTriangle', 17.999632, -78.815918)
			->add_coord('MyTriangle', 19.890723, -80.782471)
			->add_coord('MyTriangle', 17.067287, -81.095581);					
	
			
	$template = View::factory('gmaps')
							->set('external_scripts', $map->get_apilink())
							->set('internal_scripts', $map->get_map('map_container'))
							->render();	
							
	$this->response->body($template);									
			
