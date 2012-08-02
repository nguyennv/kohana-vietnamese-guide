# Quan hệ

Kohana ORM hỗ trợ bốn kiểu quan hệ đối tượng: `belongs_to`, `has_many`, `has_many "through"` và `has_one`.
Quan hệ `has_many "through"` có thể được sử dụng tới chức năng giống kiểu quan hệ `has_many_and_belongs_to` của bản ghi tích cực - Active Record.

## belongs_to

Một quan hệ `belongs_to` nên được sử dụng khi bạn có một mô hình mà thuộc về mô hình khác.
Ví dụ, một mô hình `Child` `belongs_to` một `Parent` hoặc một mô hình `Flag` `belongs_to` một `Country`.

Đây là cơ sở quan hệ `belongs_to`:

	protected $_belongs_to = array(
		'[alias name]' => array(
			'model'       => '[model name]',
			'foreign_key' => '[column]',
		),
	);

Bạn có thể bỏ qua bất kỳ hoặc tất cả khoá/giá trị trong mảng bên phải, trong trường hợp đó các giá trị mặc định được sử dụng:

	protected $_belongs_to = array('[alias name]' => array());

Tên **alias name** là cái mà được sử dụng để truy cập đến mô hình liên quan trong mã của bạn.
Nếu bạn đã có một mô hình `Post` mà thuộc về một mô hình `User` mà muốn sử dụng các giá trị mặc định của cấu hình `belongs_to` thì mã của bạn sẽ trông giống như thế này:

	protected $_belongs_to = array('user' => array());

Để truy cập mô hình người dùng, bạn sẽ sử dụng `$post->user`.
Vì chúng ta đang sử dụng các giá trị mặc định ở trên, tên bí danh sẽ được sử dụng cho tên mô hình, và khoá ngoại trong bảng bài viết sẽ là tên bí danh được theo sau bởi `_id`, trong trường hợp này nó sẽ là `user_id`.
(Bạn có thể thay đổi hậu tố `_id` bằng cách thay đổi biến `$foreign_key_suffix` trong mô hình.)

Hãy nói rằng lược đồ bảng cơ sở dữ liệu `Post` không có một cột `user_id` nhưng thay vào đó có một cột `author_id` cái mà là một khoá ngoại cho một bản ghi trong bảng `User`. Bạn có thể sử dụng mã như thế này:

	protected $_belongs_to = array(
		'user' => array(
			'foreign_key' => 'author_id',
		),
	);

Nếu bạn muốn truy cập một tác giả của bài viết bằng cách sử dụng mã giống `$post->author` thì bạn sẽ đơn giản chỉ cần thay đổi bí danh và thêm chỉ số `model`:

	protected $_belongs_to = array(
		'author' => array(
			'model'       => 'user',
		),
	);

## has_many

Quan hệ `has_many` chuẩn sẽ có khả năng rơi vào mặt kia của quan hệ một `belongs_to`.
Trong ví dụ ở trên, một bài viết thuộc về một người dùng.
Từ quan điểm của người dùng, một người dùng có nhiều bài viết.
Một quan hệ `has_many` được định nghĩa dưới đây:

	protected $_has_many = array(
		'[alias name]' => array(
			'model'       => '[model name]',
			'foreign_key' => '[column]',
		),
	);

Một lần nữa, bạn có thể bỏ qua tất cả khóa trong mảng bên phải để sử dụng các giá trị mặc định:

	protected $_has_many = array('[alias name]' => array());

Đối với ví dụ người dùng và bài viết, quan hệ này trông giống như sau trong mô hình người dùng:

	protected $_has_many = array('posts' => array());

Sử dụng quan hệ ở trên, các bài viết có thể được truy cập bằng các sử dụng `$user->posts->find_all()`.
Chú ý `find_all()` đã sử dụng trong ví dụ này.
Với kiểu quan hệ `belongs_to` và `has_one`, mô hình đã được nạp với dữ liệu cần thiết.
Tuy nhiên, đối với quan hệ `has_many` bạn có thể muốn giới hạn số kết quả hoặc thêm các điều kiện bổ xung tới truy vấn SQL;
bạn có thể làm như vậy trước khi `find_all()`.

Tên mô hình được sử dụng mặc định sẽ là tên số ít của bí danh bằng cách sử dụng lớp `inflector`.
Trong trường hợp này, `posts` sử dụng `post` như tên mô hình.
Khoá ngoại được sử dụng mặc định là tên chính mô hình được theo bởi `_id`.
Trong trường hợp này, khoá ngoại sẽ là `user_id` và nó phải tồn tại trong bảng bài viết trước.

Giả sử bây giờ bạn muốn truy cập các bài viết bằng các sử dụng tên `stories` thay thế, và vẫn đang sử dụng khoá `author_id` như trong ví dụ `belongs_to`.
Bạn sẽ định nghĩa quan hệ has_many của bạn như:

	protected $_has_many = array(
		'stories' => array(
			'model'       => 'post',
			'foreign_key' => 'author_id',
		),
	);

## has_one

Một quan hệ `has_one` gần giống một quan hệ `has_many`.
Trong một quan hệ `has_one`, có thể có quan hệ 1 và chỉ 1 (thay vì 1 hoặc nhiều trong `has_many`).
Nếu một người dùng chỉ có một bài viết hoặc câu chuyện, thay vì có nhiều thì mã sẽ trông giống như thế này:

	protected $_has_one = array(
		'story' => array(
			'model'       => 'post',
			'foreign_key' => 'author_id',
		),
	);

## has_many "through"

Một quan hệ `has_many "through"` được sử dụng cho quan hệ nhiều-nhiều (many-to-many).
Ví dụ, chúng ta giả sử bây giờ chung ta có thêm một mô hình, gọi là `Category`.
Các bài viết có thể thuộc nhiều hơn một chuyên mục, và mỗi chuyên mục có thể có nhiều hơn một bài viết.
Để liên kết chúng với nhau, một bảng bổ xung là cần thiết với các cột cho một `post_id` và một `category_id` (đôi khi được gọi là một bảng trục quay).
Chúng ta sẽ đặt tên mô hình cho `Post_Category` này và tương ứng với bảng `categories_posts`.

Để định nghĩa quan hệ `has_many "through"`, cùng cú pháp cho quan hệ `has_many` chuẩn được sử dụng với việc thêm một tham số 'through'.
Giả sử chúng ta đang làm việc với mô hình `Post`:

	protected $_has_many = array(
		'categories' => array(
			'model'   => 'category',
			'through' => 'categories_posts',
		),
	);

Trong mô hình `Category`:

	protected $_has_many = array(
		'posts' => array(
			'model'   => 'post',
			'through' => 'categories_posts',
		),
	);

Để truy cập vào các chuyên mục và bài viết, bạn đơn giản là sử dụng `$post->categories->find_all()` và `$category->posts->find_all()`

Các phương thức có sẵn để kiểm tra có, thêm, và loại bỏ quan hệ cho quan hệ nhiều-nhiều.
Giả sử bạn có một mô hình `$post` đã nạp, và một mô hình `$category` đã nạp theo.
Bạn có thể kiểm tra để xem nếu `$post` có liên quan đến `$category` với lời gọi sau:

	$post->has('categories', $category);

Tham số đầu tiên là tên bí danh để sử dụng (trong trường hợp mô hình bài viết có nhiều hơn một quan hệ tới mô hình chuyên mục) và tham số thứ hai là mô hình để kiểm tra quan hệ với nó.

Giả sử bạn muốn thêm quan hệ (bằng việc tạo một bản ghi mới trong bảng `categories_posts`), bạn chỉ cần làm:

	$post->add('categories', $category);

Để loại bỏ:

	$post->remove('categories', $category);
