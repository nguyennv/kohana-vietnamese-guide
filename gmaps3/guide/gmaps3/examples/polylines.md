# Đường nhiều nét
	
	$map = Gmaps3::instance();
		
	// Thiết lập lat, lon và zoom mặc định
	$map->set('default_lat', 48.014689)
			->set('default_lon', 1.828888)
			->set('default_zoom', 4);				
	
	// Định ngĩa một nhóm đường nhiều nét
	$map->add_polyline_group('Finisterre2Aarhus',	// Tên nhóm
							 '#FF0000',				// Màu Stroke
							 3						// Trọng lượng Stroke
							 );						// ... Độ trong suốt tô mà, màu tô
													
	// Thêm các tọa độ
	$map->add_coord('Finisterre2Aarhus', 42.907115, -9.261206);
	$map->add_coord('Finisterre2Aarhus', 43.29976, -8.374586);
	$map->add_coord('Finisterre2Aarhus', 40.417678, -3.694153);
	$map->add_coord('Finisterre2Aarhus', 56.177668, 10.176086);
	
	$template = View::factory('gmaps')
							->set('external_scripts', $map->get_apilink())
							->set('internal_scripts', $map->get_map('map_container'))
							->render();	
							
	$this->response->body($template);											
			
