#### `Jelly_Field_HasOne` (1:1)

Quan hệ này chính xác giống như `Jelly_Field_HasMany` với ngoại lệ Jelly đảm bảo rằng mô hình có thể chỉ *có* một mô hình khác, thay vì có nhiều.

	// Trong phương thức Model_Author::initialize()
	'posts' => Jelly::field('hasone', array(
		'foreign' => 'post.author:foreign_key',
	)),

**Các thuộc tính**

**`foreign`** — Mô hình mà trường này có nhiều. Bạn cũng có thể truyền một trường mà để sử dụng như khoá ngoại của mô hình ngoại.

	// Mặc định, sử dụng ví dụ ở trên
	'foreign' => 'post.author:foreign_key',

**`convert_empty`** — Thuộc tính này mặc định là TRUE, không giống như các trường khác. Các giá trị rỗng được chuyển đối tới giá trị thiết lập cho `empty_value`, cái mà mặc định là `0`.

**`empty_value`** — Đây là giá trị mặc định mà các giá trị rỗng được chuyển đổi tới. Mặc định là array().

**`delete_dependent`** — Nếu giá trị này là `TRUE` các trường phụ thuộc tự động bị xóa khi xóa. Mặc định là `FALSE`.

**Sử dụng quan hệ này**

	// Xây dựng truy vấn
	$author = Jelly::query('author', 1);

	// Các quan hệ một một có thể được ghép nối khi truy vấn bằng cách sử dụng phương thức 'with()',
	// trong trường hợp này tác giả có một bài viết
	$post->with('post');

	// Chọn tác giả chỉ nếu bài viết được chấp thuận (chú ý dấu ':' trước các mô hình đã ghép nối)
	$post->where(':post.approved_by', '!=', '0')->select();

	// Truy cập tên bài viết
	$author->post->name;

	// Truy lục bài viết nếu nó được xuất bản
	$author->get('post')->where('status', '=', 'published')->select();

	// Xóa bài viết
	$author->get('post')->delete();

	// Loại bỏ quan hệ
	$author->post = NULL;
	$author->save();