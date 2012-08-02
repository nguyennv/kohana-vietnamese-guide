# Quan hệ Jelly

Jelly hỗ trợ các quan hệ chuẩn 1:1, 1:nhiều và nhiều:nhiều thông qua các trường đặc biệt.

### Hiểu các quan hệ khác nhau

Các kiểu quan hệ phổ biến nhất và các trường mà đại diện cho chúng được trình bày bên dưới, cùng với một ví dụ đơn giản.
Chọn các quan hệ từ trình đơn bên trái.

[!!] Trong các ví dụ, các thuộc tính (như `foreign` và `through` ) được quy định trên trường hoàn toàn tuỳ chọn nhưng đã được hiển thị cho rõ ràng.

### Một lưu ý chung về nhận và thiết lập quan hệ

#### Nhận

Bạn có thể đã nhận thấy trong các ví dụ mà đôi khi chúng ta sử dụng cú pháp thuộc tính đối tượng `($model->field)` cho việc truy lục một quan hệ và các lần khác chúng ta sử dụng `$model->get('field')`.

Sự khác nhau là cú pháp thuộc tính đối tượng đó trả về quan hệ mà đã `select()`, trong khi `get()` trả về một trình dựng truy vấn mà bạn có thể làm việc với:

	// Ở đây, một Jelly_Collection của các mô hình được trả về
	$author->posts;
	
	// Ở đây, một Jelly_Builder được trả về
	$author->get('posts');
	
Với `get()` bạn có thể thêm các điều kiện bổ xung tới truy vấn trước khi bạn `select()` nó, hoặc bạn có thể thực sự thực hiện một `delete()` hoặc `update()` trên toàn bộ, nếu cần thiết.

________________

#### Thiết lập

Quan hệ rất linh hoạt trong các kiểu dữ liệu bạn có thể truyền tới nó. Đối với các quan hệ `n:1`, bạn có thể truyền những thứ sau:

 * **Một khoá chính**: e.g. `$post->author = 1;`
 * **Mộ mô hình đã nạp - loaded()**: e.g. `$post->author = Jelly::query('author', 1)->select();`
	
Đối với quan hệ `n:many`, bạn có thể truyền những thứ sau:

 * **Một mảng các khoá chính**: e.g. `$author->posts = array(1, 3, 5);`
 * **Một mảng các mô hình**: e.g. `$author->posts = array($model1, $model2, $model3);`
 * **Một kết quả truy vấn**: e.g. `$author->posts = Jelly::query('post')->select();`

### `add()` và `remove()`

Hai phương thức này đưa ra điều khiển mịn hơn trong các quan hệ `n:many`. Bạn có thể sử dụng chúng để thêm hoặc loại bỏ các mô hình cụ thể từ một quan hệ.

	// Giả sử tác giả này có các bài viết 1, 5, và 7
	$author = Jelly::query('author', 1)->select();
	
	// Loại bỏ bài viết 1
	$author->remove('posts', 1);
	
	// Loại bỏ bài viết 5
	$author->remove('posts', Jelly::query('post', 5)->select());
	
	// Làm các thay đổi vĩnh viễn
	$author->save();
	
	// Thêm trở lại bài 1 và 5
	$author->add('posts', Jelly::query('post')->where('id', 'IN', array(1, 5))->select());

Như bạn có thể thấy, `add()` và `remove()` hỗ trợ việc truyền tất cả các kiểu khác nhau của dữ liệu đã nêu ở trên.

### `has()`

Has hữu ích để xác định nếu một mô hình có môt quan hệ tới một mô hình cụ thể hoặc một tập các mô hình.
Giống như `add()` và `remove()`, phương thức này chỉ làm việc trên các quan hệ `n:many`.

	// Giả sử tác giả này có các bài viết 1, 5, và 7
	$author = Jelly::query('author', 1)->select();

	// Trả về TRUE
	$author->has('posts', 1);

	// Trả về FALSE, vì tác giả không có bài viết 8
	$author->has('posts', array(1, 5, 7, 8));
	
	// Trả về TRUE
	$author->has('posts', Jelly::query('post', 1)->select());

	// Trả về TRUE
	$author->has('posts', Jelly::query('post')->where('id', 'IN', array(1, 5))->select());
	
Một lần nữa, `has()` hỗ trợ việc truyền tất cả các kiểu khác nhau của dữ liệu đã nêu ở trên.
