# Thông tin địa lý
	
	$map = Gmaps3::instance();
		
	// Nhận thông tin địa lý từ địa chỉ
	$info = $map->get_from_address('Finisterre, Spain');
		
	echo "<pre><code>";
	print_r($info);						
	echo "</code></pre>";


				
