# Các trang lỗi thân thiện

Theo mặc định Kohana 3 không có một phương thức để hiển thị các trang lỗi thân thiện như đã thấy trong Kohana 2;
Trong chỉ dẫn gắn gọn này bạn sẽ tìm hiểu cách để nó được thực hiện.

## Điều kiện tiên quyết

Bạn sẽ cần `'errors' => TRUE` được truyền tới `Kohana::init`.
Điều này sẽ chuyển đổi các lỗi PHP thành các ngoại lệ mà dễ dàng xử lý.

## 1. Một trình xử lý ngoại lệ cải thiện

Trình xử lý ngoại lệ tuỳ biến của chúng ta là tự giải thích.

	class Kohana_Exception extends Kohana_Kohana_Exception {

		public static function handler(Exception $e)
		{
			if (Kohana::DEVELOPMENT === Kohana::$environment)
			{
				parent::handler($e);
			}
			else
			{
				try
				{
					Kohana::$log->add(Log::ERROR, parent::text($e));

					$attributes = array
					(
						'action'  => 500,
						'message' => rawurlencode($e->getMessage())
					);

					if ($e instanceof HTTP_Exception)
					{
						$attributes['action'] = $e->getCode();
					}

					// Error sub-request.
					echo Request::factory(Route::get('error')->uri($attributes))
					->execute()
					->send_headers()
					->body();
				}
				catch (Exception $e)
				{
					// Xoá bộ đệm đầu ra nếu tồn tại
					ob_get_level() and ob_clean();

					// Hiển thị văn bản ngoại lệ
					echo parent::text($e);

					// Kết thúc với một trạng thái lỗi
					exit(1);
				}
			}
		}
	}

Nếu chúng ta đang ở trong môi trường phát triển thì truyền nó ra khỏi Kohana, nếu không:

* Lưu vết lỗi
* Thiết lập hành động định tuyến và các thuộc tính của thông báo.
* Nếu một ngoại lệ `HTTP_Exception` được ném, thì ghi đè hành động với mã lỗi.
* Bắn ra một yêu cầu con nội bộ.

Hành động này sẽ được sử dụng như một mã đáp ứng HTTP.
Theo mặc định là: 500 (lỗi bên trong máy chủ) trừ khi một `HTTP_Response_Exception` được ném ra.

Vì vậy câu lệnh này:

	throw new HTTP_Exception_404(':file does not exist', array(':file' => 'Gaia'));

Sẽ hiển thị một trang lỗi 404 đẹp, ở đó:

	throw new Kohana_Exception('Directory :dir must be writable',
				array(':dir' => Debug::path(Kohana::$cache_dir)));

sẽ hiển thị một trang lỗi 500.

**Định tuyến**

	Route::set('error', 'error/<action>(/<message>)', array('action' => '[0-9]++', 'message' => '.+'))
	->defaults(array(
		'controller' => 'error_handler'
	));

## 2. Điều khiển trang lỗi

	public function before()
	{
		parent::before();

		$this->template->page = URL::site(rawurldecode(Request::$initial->uri()));

		// Internal request only!
		if (Request::$initial !== Request::$current)
		{
			if ($message = rawurldecode($this->request->param('message')))
			{
				$this->template->message = $message;
			}
		}
		else
		{
			$this->request->action(404);
		}

		$this->response->status((int) $this->request->action());
	}

1. Thiết lập biến khuôn mẫu "page" để người dùng có thể thấy cái mà họ đã yêu cầu.
   Việc này chỉ là cho mục đích hiển thị.
2. Nếu một yêu cầu bên trong, thì thiết lập biến khuôn mẫu "message" để được hiện thị tới người dùng.
3. Nếu không sử dụng hành động 404. Người dùng nếu không có thể làm thủ công thông báo lỗi của riêng họ, vd:
   `error/404/email%20your%20login%20information%20to%20hacker%40google.com`


		public function action_404()
		{
			$this->template->title = '404 Không tìm thấy';

			// Ở đây chúng ta kiểm tra xem nếu một trang 404 tới từ trang web của chúng ta. Điều này cho phép
			// quản trị trang tìm liên kết hỏng và cập nhật chúng trong một khoảng thời gian ngắn hơn.
			if (isset ($_SERVER['HTTP_REFERER']) AND strstr($_SERVER['HTTP_REFERER'], $_SERVER['SERVER_NAME']) !== FALSE)
			{
				// Thiết lập một cờ cục bộ để chúng ta có thể hiển thị các thông điệp khác nhau trong khuôn mẫu của chúng ta.
				$this->template->local = TRUE;
			}

			// Mã trạng thái HTTP
			$this->response->status(404);
		}

		public function action_503()
		{
			$this->template->title = 'Chế độ bảo trì';
		}

		public function action_500()
		{
			$this->template->title = 'Lỗi bên trong máy chủ';
		}

Bạn sẽ nhận thấy rằng mỗi phương thức ví dụ được đặt tên theo mã đáp ứng HTTP và thiết lập mã đáp ứng yêu cầu.

## 3. Kết luận

Do đó là nó. Giờ hiển thị một trang lỗi đẹp dễ dàng như:

	throw new HTTP_Exception_503('Trang web bị tắt');
