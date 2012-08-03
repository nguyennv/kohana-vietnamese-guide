# Điểm đánh dấu

	$map = Gmaps3::instance();
		
	/* Tạo một điểm đánh dấu đơn giản */
	// Chúng ta có thể sử dụng các phương thức xâu chuỗi
	$map->add_mark(56.177668, 10.103302)// Tọa độ	
		->set('view_shadow', TRUE)		// Hiển thị các biểu tượng đổ bóng
		->center(FALSE);				// Đặt trung tập bản đồ liên quan tới điểm đánh dấu này nhưng không tự động phóng to cho phù hợp
		
	
	
	/* Tạo một điểm đánh dấu màu vàng có thể kéo thả được */
	$map->add_mark(56.005628, 10.02305, 'My draggable mark', TRUE, 'http://maps.google.com/mapfiles/ms/micons/yellow-dot.png');
	
	$template = View::factory('gmaps')
				->set('external_scripts', $map->get_apilink())
				->set('internal_scripts', $map->get_map('map_container'))
				->render();	
							
	$this->response->body($template);
			
