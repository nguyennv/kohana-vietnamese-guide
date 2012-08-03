# Bắt đầu

Trước khi sử dụng GMaps3, chúng ta phải bật mô-đun

	Kohana::modules(array(
		...		
		'gmaps3' => MODPATH.'gmaps3',
		...
	));


# Cấu hình

Sao chép tập tin cấu hình từ `MODPATH/gmaps3/config/gmaps3.php` tới `APPPATH/config`

Tập tin cấu hình sẽ chứa các giá trị thiết lập và một số giá trị mặc định.

Bạn có thể thấy mô tả của mỗi khóa cấu hình trong tập tin cấu hình này.
