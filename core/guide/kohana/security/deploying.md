Các thay đổi mà sẽ xảy ra khi bạn triển khai. (Chế độ production - thành phẩm)

Các thiết lập bảo mật từ: <http://kohanaframework.org/guide/using.configuration>

<http://kerkness.ca/wiki/doku.php?id=setting_up_production_environment>


## Thiết lập một môi trường thành phẩm

Có một vài điều bạn sẽ muốn làm với ứng dụng của trước khi chuyển tới chế độ thành phẩm.

1. Xem trang [Khởi động](bootstrap) trong tài liệu.
   Trang này bao gồm hầu hết các thiết lập toàn cục mà sẽ thay đổi giữa các môi trường.
   Như một quy tắc chung, bạn sẽ bật bộ nhớ đệm và vô hiệu lập hồ sơ (các thiết lập [Kohana::init]) cho các trang thành phẩm.
   [Route::cache] cũng có thể giúp nếu bạn có nhiều định tuyến.
2. Bật APC hoặc một số loại bộ nhớ đệm opcode.
   Đây là cách tăng hiệu suất duy nhất dễ nhất bạn có thể làm cho bản thân PHP.
   Ứng dụng của bạn phức tạp hơn, lớn hơn lợi ích của việc sử dụng bộ nhớ đệm opcode.

		/**
		 * Thiết lập chuỗi môi trường bằng bằng tên miền (mặc định là Kohana::DEVELOPMENT).
		 */
		Kohana::$environment = ($_SERVER['SERVER_NAME'] !== 'localhost') ? Kohana::PRODUCTION : Kohana::DEVELOPMENT;
		/**
		 * Khởi tạo Kohana dựa trên môi trường
		 */
		Kohana::init(array(
			'base_url'   => '/',
			'index_file' => FALSE,
			'profile'    => Kohana::$environment !== Kohana::PRODUCTION,
			'caching'    => Kohana::$environment === Kohana::PRODUCTION,
		));

`index.php`:

		/**
		 * Chạy yêu cầu chính bằng cách sử dụng PATH_INFO. Nếu không có nguồn URI được chỉ định,
		 * URI sẽ được tự động phát hiện.
		 */
		$request = Request::instance($_SERVER['PATH_INFO']);

		// Cố gắng để thực hiện đáp ứng
		$request->execute();

		if ($request->send_headers()->response)
		{
			// Lấy tổng số bộ nhớ và thời gian thực hiện
			$total = array(
			  '{memory_usage}' => number_format((memory_get_peak_usage() - KOHANA_START_MEMORY) / 1024, 2).'KB',
			  '{execution_time}' => number_format(microtime(TRUE) - KOHANA_START_TIME, 5).' seconds');

			// Chèn tổng số vào đối tượng phản hồi
			$request->response = str_replace(array_keys($total), $total, $request->response);
		}


		/**
		 * Hiển thị phản hồi của yêu cầu.
		 */
		echo $request->response;


