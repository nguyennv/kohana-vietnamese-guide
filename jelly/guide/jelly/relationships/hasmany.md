#### `Jelly_Field_HasMany` (1:many)

Quan hệ này chủ yếu là đảo ngược của một quan hệ `belongs_to`.
Ở đây, một mô hình có nhiều mô hình khác.

Ví dụ `hasmany` sau đây: một tác giả có nhiều bài viết.
Trong cơ sở dữ liệu, bảng `posts` sẽ có một cột `author_id` mà chứa khoá chính của tác giả mà sở hữu bài viết.

	// Trong phương thức Model_Author::initialize()
	'posts' => Jelly::field('hasmany', array(
		'foreign' => 'post',
	)),

**Các thuộc tính**

**`foreign`** — Mô hình mà trường này có nhiều. Bạn cũng có thể truyền một trường để sử dụng khoá ngoại của mô hình ngoại.

	// Mặc định, sử dụng ví dụ ở trên
	'foreign' => 'post.author:foreign_key',

**`default`** — Thuộc tính này làm việc hơi khác các trường khác. Mặc định là giá trị đó sẽ được thiết lập trên cột của mô hình ngoại khi một quan hệ được loại bỏ. Trường này sẽ luôn luôn vẫn là 0.

**`convert_empty`** — Thuộc tính này mặc định là `TRUE`, không giống như hầu hết các trường khác. Các giá trị rỗng được chuyển đổi tới giá trị thiết lập cho `empty_value`, cái mà mặc định là `0`.

**`empty_value`** — Trường này giá trị mặc định mà các giá trị rỗng được chuyển đổi tới. Mặc định là `0`.

**`delete_dependent`** — Nếu giá trị này là `TRUE` các trường phụ thuộc tự động bị xóa khi xóa. Mặc định là `FALSE`.

**Sử dụng quan hệ này**

	$author = Jelly::query('author', 1)->select();

	// Truy cập tất cả bài viết
	foreach ($author->posts as $post)
	{
		echo $post->name;
	}

	// Truy lục chỉ các bài viết đã xuất bản
	$posts = $author->get('posts')->where('status', '=', 'published')->select();

	// Thay đổi tất cả bài viết
	$author->posts = array($post1, $post2, $post3);

	// Loại bỏ quan hệ
	$author->posts = NULL;
	$author->save();

**Truy lục quan hệ nhiều-nhiều**

Ví dụ, nếu một tác giả có nhiều bài viết có nhiều ý kiến​​, chúng ta có thể lấy tất cả các ý kiến ​​được gửi đến bài viết của tác giả.

Cách khuyến khích để làm điều này là mở rộng trình dựng truy vấn và tạo ra một phương thức để lấy ý kiến​​.

	<?php defined('SYSPATH') or die('No direct script access.');

	class Model_Builder_Comment extends Jelly_Builder {

		public function get_authors_comments($author_id)
		{
			return $this->with('post:author')->where(':post:author.id', '=', $author_id);
		}

	}

Từ bây giờ bạn có thể sử dụng phương thức này theo cách này:

	Jelly::query('comment')->get_authors_comments(1)->select();


[!!] Xem các [phương thức](../jelly/relationships#add-and-remove) `add()`, `remove()`, và `has()` cho các ví dụ khác.
