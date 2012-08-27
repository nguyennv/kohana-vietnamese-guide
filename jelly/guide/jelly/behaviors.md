# Hành vi và sự kiện

Jelly làm cho nó có thể tạo ra các hành vi mà định nghĩa các hành động được thực hiện trên một sự kiện cụ thể, ví dụ như trước khi lưu một mô hình hoặc sau khi lưu một mô hình.
Nhưng chúng có nhiều khả năng hơn thì đây là **Ngân hàng** mô tả chúng:

> Hành vi cho phép bạn thêm chức năng cho _các thể hiện_ mô hình (cũng như các trình dựng mô hình) trong một cách mà nhiều thừa kế có thể đạt được. Ví dụ một mô hình có thể có trật tự MPTT VÀ là phiên bản điều khiển đơn giản bằng cách khai báo rằng nó sẽ hiển thị cả hai hành vi. Một mô hình khác chỉ có thể hỗ trợ MPTT không có cây thừa kế phức tạp hoặc trùng lặp mã.
>
> Thực tế rằng một hành vi có thể thêm các cải tiến tới trình dựng mô hình có thể làm mờ các dòng một chút nhưng điều này là đơn giản vì vậy nếu bạn khai báo rằng mô hình của bạn cư xử một cách nào đó, mà có thể bao hàm chức năng nhất định cho cả hai trường hợp của mô hình đó và danh sách mô hình đó.
>
> Ví dụ, Tôi có thể viết một số mô hình khác nhau mà tất cả các chia sẻ một số chức năng như duy trì một trạng thái  xuất bản / chưa xuất bản. Chứ không phải là bản sao tất cả các trường / mã để xử lý trạng thái này, Tôi chỉ có thể tạo ra một hành vi - gọi `Jelly_Behavior_Publishable` - mà có thêm chức năng ví dụ giống như `is_published()` hoặc `publish_now()`, nhưng cũng có thể thêm chức năng tới trình dựng như `published(TRUE)`. Vì vậy, Tôi có thể làm những việc như `Jelly::query('post')->published(TRUE)` và không có trùng lặp chức năng xuất bản liên quan trong nhiều trong mô hình.

## Tạo hành vi

Bởi các hành vi mặc định nên đi theo *APPATH/classes/jelly/behavior*, và chúng phải mở rộng `Jelly_Behavior`.

Một hành vi ví dụ:

	<?php defined('SYSPATH') or die('No direct access allowed.');

	class Jelly_Behavior_Post extends Jelly_Behavior {

		public function model_before_save($params, $event_data)
		{
			// Làm cái gì đó
		}

	} // End Jelly_Behavior_Post

#### Sự kiện trong hành vi

Trong ví dụ trên phương thức `model_before_save($params, $event_data)` được gọi chỉ ngay trước khi mô hình được lưu.
Các phương thức sự kiện nhận được các thông số và dữ liệu sự kiện khi được kích hoạt, những nội dung khác nhau cho mỗi sự kiện.

#### Dữ liệu sự kiện

Mỗi sự kiện nhận được dữ liệu sự kiện, có thể là hữu ích cho một số thứ.
Bạn có thể thiết lập giá trị trả lại của sự kiện hoặc quyết định nếu bạn muốn thực hiện sự kiện hơn nữa trong một tình huống nhất định.

Giá trị trả lại có thể làm thay đổi kết quả của một hành động, kiểm tra một ví dụ sau đây.

	<?php defined('SYSPATH') or die('No direct access allowed.');

	class Jelly_Behavior_Post extends Jelly_Behavior {

		public function model_before_save($params, $event_data)
		{
			if ($param->loaded())
			{
				// Thiết lập giá trị trả lại của sự kiện, trong trường hợp này trả về
				// FALSE điều này sẽ làm cho Jelly bỏ qua việc lưu của mô hình
				$event_data->return = FALSE;

				// Dừng thực hiện các sự kiện thêm nữa
				$event_data->stop = TRUE;
			}
		}

	} // End Jelly_Behavior_Post

#### Các sự kiện mặc định có sẵn

Jelly có một vài sự kiện dựng sẵn, như một trong những đề cập ở trên.
Kiểm tra danh sách đầy đủ các [sự kiện có sẵn](behaviors/events).

#### Tạo sự kiện tùy biến

Bạn tự do để tạo ra các sự kiện của riêng bạn, [đọc hướng dẫn](behaviors/custom-events) để làm quen với các API sự ​​kiện.


## Nạp hành vi trong mô hình

Nếu một hành vi được tạo ra với tên giống như mô hình, nó được tự động nạp.
Ví dụ `Model_Post` nạp hành vi `Jelly_Behavior_Post`.

Việc thêm hành vi có đặt tên khác nhau được thực hiện trong phương thức `initialize()`.

	<?php defined('SYSPATH') or die('No direct access allowed.');

	class Model_Post extends Jelly_Model
	{
		public static function initialize(Jelly_Meta $meta)
		{
			// Nạp các hành vi
			$meta->behaviors(array(
				'foo' => Jelly::behavior('Foo'),
				'bar' => Jelly::behavior('Bar'),
			));

			// Các trường được định nghĩa bởi mô hình
			$meta->fields(array(
				// Định nghĩa các trường
			));
		}

	} // End Model_Post