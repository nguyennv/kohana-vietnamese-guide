# Sự kiện tùy biến

Bạn có thể tạo ra hai loại sự kiện tùy biến cho các hành vi:

- sự kiện tự đông phát hiện (auto-discoverable event)
- sự kiện phải được ràng buộc bằng tay

Bạn có thể gọi các sự kiện này bất cứ nơi nào bạn muốn trong các mô hình hoặc trình xây dựng của bạn.

## Sự kiện tự đông phát hiện

Để tạo ra các sự kiện được tự động phát hiện và ràng buộc tới các hành vi, tất cả những gì bạn phải làm là gắn tiền tố vào các phương thức của bạn với một trong những điều sau đây:

- `model_`
- `builder_`
- `meta_`

Một hành vi ví dụ với một sự kiện tùy biến:

	<?php defined('SYSPATH') or die('No direct access allowed.');

	class Jelly_Behavior_Post extends Jelly_Behavior {

		public function model_publish_post($params, $event_data)
		{
			// Làm cái gì đó
		}

	} // End Jelly_Behavior_Post

## Sự kiện có thể ràng buộc bằng tay

Bạn có thể tạo ra các sự kiện với những cái tên hoàn toàn tùy biến, ví dụ sẽ là

	<?php defined('SYSPATH') or die('No direct access allowed.');

	class Jelly_Behavior_Post extends Jelly_Behavior {

		public function my_custom_event($params, $event_data)
		{
			// Làm cái gì đó
		}

	} // End Jelly_Behavior_Post

Những sự kiện này có được ràng buốc tới các hành vi bằng tay bằng cách

	$this->_meta->events()->bind($event, $callback);

- `$event`: tên của sự kiện, ví dụ: 'my.custom_event'. Lưu ý tiền tố!
- `$callback`: callback mà được gọi cho sự kiện cụ thể, ví dụ việc này này sẽ gọi phương thức `my_custom_event()` trong hành vi **Post**: `array(Jelly::behavior('Post'), 'my_custom_event')`

## Kích hoạt sự kiện

Các sự kiện tùy biến có thể được kích hoạt trong mô hình hoặc trình xây dựng của bạn như thế này:

	$this->_meta->events()->trigger($event, $sender, $params);

- `$event`: tên của sự kiện, ví dụ: 'model.publish_post'. Lưu ý tiền tố!
- `$sender`: đối tượng gửi của bộ kích hoạt, ví dụ bạn có thể thiết lập nó là `$this`
- `$params`: các tham số tùy biến cho sự kiện