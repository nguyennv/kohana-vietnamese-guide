# Tạo, cập nhật và xoá bản ghi

Để minh hoạ các phương thức khác nhau được sử dụng để thao tác các bản ghi, chúng ta sẽ tạo, lưu và xoá một bản ghi.

### Tạo và cập nhật

Cả hai việc tạo và cập nhật được thực hiện với phương thức `save()`.
Hãy nhớ rằng `save()` có thể ném một [Jelly_Validation_Exception](../api/Jelly_Validation_Exception) nếu mô hình của bạn không xác nhận theo các quy tắc bạn chỉ định, do đó bạn sẽ luôn luôn kiểm thử cho việc này.
Có nói rằng, chúng ta sẽ không ở đây để chỉ rõ ràng.

##### Ví dụ - Tạo một bản ghi mới

Bạn có thể truyền một mảng giá trị tới `set()` hoặc bạn có thể thiết lập các thành viên đối tượng một cách trực tiếp.

	Jelly::factory('post')
		 ->set(array(
			 'name'      => 'A new post',
			 'published' => TRUE,
			 'body'      => $body,
			 'author'    => $author,
			 'tags'      => $some_tags,
		 ))->save();

##### Ví dụ - Cập nhật một bản ghi

Bởi vì mô hình được nạp, Jelly biết rằng bạn muốn cập nhật, thay vì chèn.

	$post = Jelly::query('post', 1)->select();
	$post->name = 'Một têm mới!';
	$post->save();

##### Ví dụ - Lưu một bản ghi từ dữ liệu $_POST

Một khi phải quan tâm để chỉ chèn các khoá mà bạn muốn, mặt khác có các ý nghĩa bảo mật quan trọng.
Ví dụ, việc chèn dữ liệu POST một cách trực tiếp sẽ cho phép một kẻ tấn công cập nhật các trường mà không nhất thiết ở trong mẫu biểu.

	// Tạo một mô hình trống
	$model = Jelly::factory('post');

	// Trích xuất các khóa hữu ích
	$model->set(Arr::extract($_POST, array('keys', 'to', 'use')));

	// Lưu mô hình
	$model->save();

### Xoá

Việc xoá bỏ cũng khá đơn giản với Jelly.
Bạn chỉ cần gọi phương thức `delete()` trên một mô hình.
Số các hàng ảnh hưởng (1 hoặc 0) sẽ được trả về.

##### Ví dụ - Xoá bản ghi được nạp hiện thời

	$post = Jelly::query('post', 1)->select();
	$post->delete();