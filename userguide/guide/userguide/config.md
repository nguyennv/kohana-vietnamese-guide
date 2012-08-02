# Cấu hình

userguide có những tuỳ chọn cấu hình sau, có sẵn trong `config/userguide.php`.

	return array
	(
		// Cho phép duyệt API.  TRUE hoặc FALSE
		'api_browser'  => TRUE,
		
		// Cho phép các gói này trong trình duyệt API.  TRUE cho tât cả các gói, hoặc một chuỗi dấu phẩy phân cách các gói, Sử dụng 'None' cho một lớp với không có @package
		// Ví dụ: 'api_packages' => 'Kohana,Kohana/Database,Kohana/ORM,None',
		'api_packages' => TRUE,
		
	);

Bạn có thể cho phép hoặc vô hiệu toàn bộ trình duyệt api, hoặc giới hạn nó chỉ hiện thị các gói nào đó.
Để vô hiệu một mô-đun từ việc hiển thị các trang trong userguide, một cách đơn giản thay đổi tuỳ chọn `enabled` các mô-đun bằng việc sử dụng hệ thống tập tin phân cấp.
Ví dụ:

	`application/config/userguide.php`
	
	return array
	(
		'modules' => array
		(
			'kohana' => array
			(
				'enabled' => FALSE,
			),
			'database' => array
			(
				'enabled' => FALSE,
			)
		)
	)
	
Việc sử dụng này bạn có thể làm userguide chỉ hiển thị các mô-đun của bạn và các lớp của bạn trong trình duyệt api, nếu bạn muốn lưu tài liệu của riêng bạn trên trang web của bạn.
Cảm thấy tự do để thay đổi phong cách và hiển thị là tốt, nhưng chắc chắn rằng để cung cấp tín dụng nơi mà tín dụng đến kỳ hạn!
