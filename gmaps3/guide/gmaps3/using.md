# Sử dụng cơ bản

## Hiển thị một bản đồ

Bạn sẽ cần tạo ra một khuôn mẫ với một tham chiếu cho Google Maps API bên ngoài và tham chiếu khác cho mã tự sinh.

Bạn có thể thêm URL của Google Maps API tĩnh nếu bạn muốn.

Ví dụ của khuôn mẫu:
	
	<html>
	<head>
		<meta charset=utf-8>
		<title>Mô-đun Kohana Google Maps 3</title>
		<style>
		#map_container {
			width: 500px;
			height: 500px;
			border: 1px solid #000;
		}
		</style>
	
		<!-- Tham chiếu tới Google Maps API bên ngoài--> 
		<?php echo $external_scripts?>
	
		<script type="text/javascript">
			window.onload = function() {
				// Tham chiếu tới mã tự sinh
				<?php echo $internal_scripts?>
			};
		</script>
	
	</head>
	<body>
		
		<!-- Lớp này sẽ chứa bản đồ -->
		<div id="map_container"></div>
	
	</body>
	</html>
	


Bây giờ chúng ta sẽ tạo bản đồ từ điều khiển của chúng ta:

	// Thể hiển của Gmaps3
	$map = Gmaps3::instance();
		
	// Lấy khuôn mẫu
	$template = View::factory('gmaps')
				->set('external_scripts', $map->get_apilink())
				->set('internal_scripts', $map->get_map('map_container'))
				->render();	
								
	$this->response->body($template);			


Phương thức
	
	$map->get_map([layer id])

sẽ hiển thị bản đồ vào trong [layer id]. 

Hãy nhớ thêm lớp bản đồ của bạn một kiểu css chiều rộng và chiều cao tối thiểu.
