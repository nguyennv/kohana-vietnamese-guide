#### `Jelly_Field_BelongsTo` (1:1)

Trường này cho phép một mô hình thuộc về, hoặc là được *sở hữu bởi*, mô hình khác.

Ví dụ, một bài viết thuộc về một tác giả.
Trong cơ sở dữ liệu, bảng `posts` sẽ có một cột `author_id` mà chứa khoá chính của tác giả nó thuộc về.

	// Trong phương thức Model_Post::initialize()
	'author' => Jelly::field('belongsto', array(
		'column' => 'author_id',
		'foreign' => 'author',
	)),

**Các thuộc tính**

**`column`** — Thuộc tính này xác định tên của cột mà trường đại diện. `BelongsTo` là khác từ các quan hệ khác ở cho nó thực sự đại diện một cột trong cơ sở dữ liệu. Nói chung, thuộc tính này sẽ bằng khoá chính của mô hình mà nó thuộc về.

	// Mặc định, sử dụng ví dụ ở trên
	'column' => 'author_id'

**`foreign`** — Mô hình mà trường này thuộc về. Bạn cũng có thể truyền một trường để sử dụng như khoá chính của mô hình ngoại.

	// Mặc định, sử dụng ví dụ ở trên
	'foreign' => 'author.post:primary_key',

**`convert_empty`** — Thuộc tính này mặc định là `TRUE`, không giống như hầu hết các trường khác. Các giá trị rỗng được chuyển đổi thành giá trị thiết lập cho `empty_value`, cái mà mặc định là `0`.

**`empty_value`** — Đây là giá trị mà các giá trị rỗng được chuyển đổi tới. Mặc định là `0`.

**Sử dụng quan hệ này**

	// Xây dựng truy vấn
	$post = Jelly::query('post', 1);

	// Nhiều quan hệ 1:01 có thể được ghép nối (join) khi truy vấn bằng cách sử dụng phương thức 'with()',
	// trong trường hợp bài viết thuộc về tác giả và tác giả thuộc về vai trò
	$post->with('author:role');

	// Chọn bài viết chỉ nếu nó được viết bởi admin (chú ý dấu ':' trước mô hình đã ghép nối)
	$post->where(':author:role.type', '=', 'admin')->select();

	// Truy cập tên tác giả
	echo $post->author->name;

	// Thay đổi tác giả
	$post->author = Jelly::query('author', 1)->select();
	$post->save();

	// Loại bỏ quan hệ
	$post->author = NULL;
	$post->save();