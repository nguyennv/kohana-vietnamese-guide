# Tìm các bản ghi

Mỗi mô hình có một `Jelly_Builder` được đính kèm tới nó mà được sử dụng cho tất cả thao tác truy vấn.
Các mô hình có thể chọn để sử dụng `Jelly_Builder` có sẵn hoặc [mở rộng Jelly_Builder](extending-builder) để thêm các phương thức xây dựng tuỳ biến tới mô hình của chúng.

### Tìm kiếm bản ghi

Jelly có các phương thức cái mà cho phép bạn lấy một bản ghi duy nhất bằng khoá hoặc một tập hợp các bản ghi mà khớp với các điều kiện.
Giao diện rất giống với Trình dựng truy vấn cơ sở dữ liệu của Kohana.

	// Tìm tất cả bài viết. Một Jelly_Collection của Model_Post được trả về.
	$posts = Jelly::query('post')->select();
	
	// Duyệt qua các bài viết của chúng ta để làm những thứ với chúng
	foreach($posts as $post)
	{
		if ($post->loaded())
		{
			echo "Bài viết #{$post->id} được nạp";
		}
	}
	
	// Tìm bài viết với một unique_key là 1.
	$post = Jelly::query('post', 1)->select();
	
	// Chúng ta không phải duyệt từ một mô hình được trả về trực tiếp
	if ($post->loaded())
	{
		echo "Bài viết #{$post->id} được nạp";
	}

Để thực thi truy vấn bạn kết thúc việc xây dựng truy vấn của bạn với phương thức `select()`, cái mà trả về một `Jelly_Collection`.
Một `Jelly_Collection` chứa một tập hợp các bản ghi đó, khi duyệt qua việc trả về các mô hình cho bản để làm việc với.

[!!] **Lưu ý**: Lưu ý: Bất cứ khi nào bạn `limit()` một truy vấn là 1, `select()` trả về một thể hiện của mô hình một cách trực tiếp, thay vì trả về một `Jelly_Collection`

### Điều kiện

Thay vì định nghĩa các điều kiện bằng cách sử dụng các mảnh SQL chung ta xâu chuỗi các phương thức có tên tương tự SQL.
Đây là nơi Trình dựng truy vấn cơ sở dữ liệu của Kohana sẽ có vẻ rất quen thuộc.

	// Tìm tất cả các bài viết đã xuất bản, sắp xếp theo ngày xuất bản
	$posts = Jelly::query('post')
	              ->where('status', '=', 'published')
	              ->order_by('publish_date', 'ASC')
	              ->select();

### Đếm các bản ghi

Tại bất kỳ thời gian nào trong quá trình một xâu chuỗi trình xây dựng truy vấn, bạn có thể gọi phương thức `count()` để tìm ra có bao nhiêu bản ghi sẽ được trả về.

	$total_posts = Jelly::query('post')->where('published', '=', 1)->count();

### Trả về các kết quả như một mảng

Bạn có thể nhận các kết quả cơ sở dữ liệu trong một mảng bằng cách sử dụng phương thức `as_array()`.

	// Nạp tất cả bài viết
	$posts = Jelly::query('post')->select();

	// Trả về dữ liệu như một mảng cho các trường id, name, và body
	$data = $posts->as_array(array('id', 'name', 'body'));

	// Trả về chỉ có tên trong một mảng
	$data = $posts->as_array(array(NULL, 'name'));

### Trả về `Jelly_Collection` bất kể giới hạn

Bạn có thể muốn trả về `Jelly_Collection` ngay cả khi giới hạn được thiết lập là 0.
Điều này có thể là trường hợp khi bạn đang lặp thông qua các kết quả trong một vòng lặp foreach ngay cả khi chỉ có một kết quả.
Trong trường hợp sử dụng này có thể sử dụng phương thức `select_all()` cái mà sẽ luôn trả về `Jelly_Collection`.

	// Nhận bài viết giới hạn từ url
	$limit = (int) $this->request->query('limit');

	// Nhận tất cả các bài viết
	$posts =  Jelly::query('post')->limit($limit)->select_all();

	// Hiển thị tất cả các bài
	foreach ($posts as $post)
	{
		echo $post->text;
	}

### Lựa chọn các cột chỉ định

Bạn có thể chọn các cột chỉ định trong các truy vấn của bạn với phương pháp `select_columns()`.

	// Chọn một tại một thời điểm
	$query->select_column('id')->select_column('name')->select();

	// Chọn một cột và định danh nó (chỉ có thể với các cột được xác định riêng lẻ)
	$query->select_column('name', 'name_alias')->select();

	// hoặc nhiều tại một thời điểm
	$query->select_column(array('id', 'name', 'body'))->select();

### Biểu thức cơ sở dữ liệu

Bạn có thể sử dụng các biểu thức cơ sở dữ liệu như trong mô-đun [Cơ sở dữ liệu](../database/query/builder#database-expressions) của Kohana, với cùng các cẩn trọng.
Điều này có nghĩa là **một biểu thức cơ sở dữ liệu được thực hiện như đầu vào trực tiếp và không thoát (escape) được thực hiện**

Biểu thức cơ sở dữ liệu có thể được sử dụng trong hình thức sau đây:

	// Chọn một cột và chuyển đổi nó thành chữ hõa trong khi định danh nó thành 'name_in_uppercase'
	Jelly::query('author')->select_column(DB::expr('UPPER(`name`)'), 'name_in_uppercase')->select();

	// Sử dụng biểu thức cơ sở dữ liệu trong câu lệnh where
	Jelly::query('author')->where(DB::expr("BINARY `hash`"), '=', $hash)->select();