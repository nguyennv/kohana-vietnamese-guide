#### `Jelly_Field_Image`

Đại diện một hình ảnh tải lên.
Trường này cư sử gần chính xác giống như `Jelly_Field_File` ngoại trừ nó cho phép chuyển đổi hình ảnh gốc và tạo một số không giới hạn các hình ảnh thu nhỏ khác nhau.

Để thực hiện trường này là bắt buộc, hãy sử dụng quy tắc `Upload::not_empty` thay cho `not_empty` đơn giản.

Dưới đây là một ví dụ mà bạn thay đổi kích cỡ hình ảnh ban đầu và tạo ra một hình thu nhỏ.


	Jelly::field('image', array(
		// nơi để lưu hình ảnh ban đầu
		'path'			  => 'upload/images/',
		// các phép biến đổi cho hình ảnh ban đầu, tham khảo các mô-đun Hình ảnh về các phương thức có sẵn
		'transformations' => array(
			'resize' => array(1600, 1600, Image::AUTO),  // chiều rộng, chiều cao, chiều tổng thể
		),
		// mong muốn chất lượng của hình ảnh lưu lại, mặc định 100
		'quality'		  => 75,
		// định nghĩa hình thu nhỏ của bạn ở đây, nếu việc lưu hình thu nhỏ vào thư mục gốc đừng quên đặt một tiền tố
		'thumbnails'      => array (
			// hình thu nhỏ đầu tiênl
			array(
				// nơi lưu hình ảnh thu nhỏ
				'path'   => 'upload/images/thumbs/',
				// tiền tố của tên tập tin hình ảnh thu nhỏ
				'prefix' => 'thumb_',
				// phép biến đổi cho hình thu nhỏ, tham khảo các mô-đun Hình ảnh về các phương thức có sẵn
				'transformations' => array(
					'resize' => array(500, 500, Image::AUTO),  // chiều rộng, chiều cao, chiều tổng thể
					'crop'   => array(100, 100, NULL, NULL),   // chiều rộng, chiều cao, offset_x, offset_y
				),
				// mong muốn chất lượng của hình ảnh lưu lại, mặc định 100
				'quality' => 50,
			),
			// hình thu nhỏ thứ hai
			array(
				// ...
			),
		)
	));

[!!] Các bước chuyển đổi sẽ được thực hiện theo thứ tự mà bạn chỉ định chúng trong mảng.

 * **`path`** — Thuộc tính này phải trỏ để một thư mục hợp lệ, có thể ghi được để lưu hình ảnh vào.
 * **`transformations`** — Các phép biến đổi cho hình ảnh, tham khảo các mô-đun Hình ảnh về các phương thức có sẵn
 * **`quality`** — mong muốn chất lượng của hình ảnh được lưu giữa 0 và 100, mặc định là 100.
 * **`driver`** — trình điều khiển hình ảnh để sử dụng, mặc định là `NULL` kết quả là sử dụng [Image::$default_driver](../api/Image#property:default_driver)
 * **`delete_old_file`** — Có hoặc không xoá các tập tin cũ khi một hình ảnh mới được tải thành công. Mặc định là `FALSE`.
 * **`delete_file`** — Nếu giá trị là TRUE, tập tin sẽ được tự động bị xóa khi xóa. Mặc định là `FALSE`.
 * **`types`** — Các phần mở rộng tập tin hợp lệ mà tập tin có thể có. Mặc định cho phép là JPEG, GIF, và PNG.

 [Tài liệu API](../api/Jelly_Field_Image)