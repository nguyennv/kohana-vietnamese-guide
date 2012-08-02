#### `Jelly_Field_ManyToMany` (nhiều:nhiều)

Quan hệ này kết nối hai mô hình để mỗi mô hình có thể có nhiều mô hình khác.

Ví dụ, một bài viết blog có thể có nhiều thẻ, nhưng một thẻ cụ thể có thể thuộc về nhiều bài viết khác.

Quan hệ này yêu cầu một bảng `through` mà kết nối hai mô hình.

	// Trong phương thức Model_Post::initialize()
	'tags' => Jelly::field('manytomany', array(
		'foreign' => 'tag',
		'through' => 'posts_tags',
	)),

**Các thuộc tính**

**`foreign`** — Mô hình mà trường này có nhiều. Bạn cũng có thể truyền một trường mà sử dụng như một khoá chính của mô hình ngoại.

	// Mặc định, sử dụng ví dụ ở trên
	'foreign' => 'tag.tag:primary_key',

**`through`** — Bảng hoặc mô hình để sử dụng như bảng kết nối. Bạn cũng có thể truyền một mảng các trường để sử dụng cho việc kết nối hai khoá chính. Không giống `foreign`, mô hình có thể thực sự chỉ tới một bảng, và không cần trỏ tới một mô hình thực sự.

	// Mặc định, sử dụng ví dụ ở trên
	'through' => array(
		'model' => 'posts_tags',
		'fields' => array('post:foreign_key', 'tag:foreign_key'),
	),

**Sử dụng quan hệ này**

	$post = Jelly::query('post', 1)->select();

	// Truy cập tât cả thẻ
	foreach ($post->tags as $tag)
	{
		echo $tag->name;
	}

	// Xoá tất cả thẻ liên quan
	$post->get('tags')->delete();

	// Thay đổi thẻ
	$post->tags = Jelly::query('tag')->where('id', 'IN', $tags)->select();
	$post->save();

	// Loại bỏ các thẻ
	$post->tags = NULL;
	$post->save();

[!!] Xem các [phương thức](../jelly/relationships#add-and-remove) `add()`, `remove()`, và `has()` cho các ví dụ khác.
