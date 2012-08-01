# Kết quả

## Thực thi

Một khi bạn có một đối tượng truy vấn đã xây dựng, hoặc thông qua một câu lệnh đã chuẩn bị hoặc thông qua trình dựng truy vấn, bạn sau đó phải `execute()` truy vấn và nhận các kết quả.
Tuỳ thuộc vào loại truy vấn được sử dụng, các kết quả trả về sẽ khác nhau.

## Lựa chọn

[DB::select] sẽ trả về một đối tượng [Database_Result] cái mà bạn có thể duyệt lặp qua.
Ví dụ này hiển thị các bạn có thể duyệt lặp qua [Database_Result] bằng cách sử dụng một câu lệnh foreach.

	$results = DB::select()->from('users')->where('verified', '=', 0)->execute();
	foreach($results as $user)
	{
		// Gửi email nhắc nhở đến $user['email']
		echo $user['email']." needs to verify his/her account\n";
	}

### Lựa chọn - `as_object()` và `as_assoc()`

Khi đang duyệt lặp qua một tập kết quả, kiểu mặc định sẽ là một mảng liên kết với các tên cột hoặc các bí danh như là các khoá.
Như một tuỳ chọn, trước khi gọi `execute()`, bạn có thể chỉ định để trả lại các hàng kết quả như một đối tượng bằng cách sử dụng phương thức `as_object()`.
Phương thức `as_object()` có một tham số, tên của lớp của chọn lựa của bạn, nhưng sẽ mặc định là TRUE mà sử dụng `stdClass`.
Dưới đây là ví dụ nữa bằng cách sử dụng `stdClass`.

	$results = DB::select()->from('users')->where('verified', '=', 0)->as_object()->execute();
	foreach($results as $user)
	{
		// Gửi email nhắc nhở đến $user->email
		echo $user->email." needs to verify his/her account\n";
	}

[!!] Phương thức `as_assoc()` sẽ loại bỏ tên đối tượng và trả về tập các kết quả trở lại một mảng liên kết. Vì đây là mặc định, phương thứ này là ít khi cần thiết.

### Lựa chọn - `as_array()`

Đôi khi bạn sẽ yêu cầu các kết quả như là một mảng thuần hơn là một đối tượng.
Phương thức `as_array()` của `Database_Result` sẽ trả về một mảng của tất cả các hàng.

	$results = DB::select('id', 'email')->from('users')->execute();
	$users = $results->as_array();
	foreach($users as $user)
	{
		echo 'User ID: '.$user['id'];
		echo 'User Email: '.$user['email'];
	}

Nó cũng chấp nhận hai tham số mà có thể rất hữu ích: `$key` và `$value`.
Khi truyền một giá trị tới `$key` bạn sẽ đánh chỉ số mảng kết quả bằng cột được chỉ định.

	$results = DB::select('id', 'email')->from('users')->execute();
	$users = $results->as_array('id');
	foreach($users as $id => $user)
	{
		echo 'User ID: '.$id;
		echo 'User Email: '.$user['email'];
	}

Tham số thứ hai, `$value`, sẽ tham chiếu tới cột chỉ định và trả về giá trị hơn là toàn bộ hàng.
Điều này đặc biệt hữu ích khi tạo các danh sách thả xuống `<select>`.

	$results = DB::select('id', 'name')->from('users')->execute();
	$users = $results->as_array('id','name');
	// Hiển thị môt danh sách thả xuống tất cả người dùng trong nó.
	echo Form::select('author', $users)

Để trả về mộ mảng không liên kết, bỏ `$key` là NULL và chỉ truyền `$value`.

	$results = DB::select('email')->from('users')->execute();
	$users = $results->as_array(NULL, 'email');
	foreach($users as $email)
	{
		echo 'User Email: '.$email;
	}

### Lựa chọn - `get()`

Đôi khi bạn chỉ muốn một giá trị duy nhất từ một truy vấn.
Phương thức `get()` trả về giá trị của cột có tên từ hàng hiện thời.
Tham số thứ hai, `$default`, được sử dụng để cung cấp một giá trị mặc định khi kết quả là NULL.

	$total_users = DB::select(array('COUNT("username")', 'total_users'))->from('users')->execute()->get('total_users', 0);

### Lựa chọn - `cached()`

Trình điều khiển cơ sở dữ liệu mysql trả về một `Database_Result` mà làm việc với một kiểu dữ liệu Tài nguyên MySQL.
Để làm được việc này đối tượng `Database_Result` có phương thức `cached()` mà trả về một đối tượng `Database_Result_Cached` của tập kết quả.
`Database_Result_Cached` có thể được làm mã hoá tuần tự và lưu vào bộ nhớ đệm, nhưng có thể mất nhiều bộ nhớ hơn.

[!!] Lưu ý: Hiện nay, trình điều khiển PDO luôn trả về một lớp của `Database_Result_Cached`, do đó `cached()` chỉ trả về bản thân nó.

Hàm `cached()` không thực sự làm bất kỳ bộ nhớ đệm nào, nó đơn giản trả về kết quả trong một mảng mà có thể được mã hoá tuần tự và lưu vào bộ nhớ đệm.
Bạn sẽ cần sử dụng ]Mô-đun bộ nhớ đệm](../cache) hoặc mọt vài phương thức bộ nhớ đệm khác.

### Lựa chọn - `count()`

Đối tượng `Database_Result` thực hiện gia diện `Countable`.
Phương thức `count()` trả về tổng số cột đếm trong tập kết quả.

[!!] Lưu ý: Đây là việc đếm tập kết quả hiện thời, không phải là một việc đếm có bao nhiêu bản ghi trong cơ sở dữ liệu. Điều quan trọng là chỉ ra đặc biệt là khi sử dụng `limit()` và `offset()` trong truy vấn của bạn.

[!!] Đối với một danh sách hoàn chỉnh có sẵ khi làm việc với tập kết quả hãy xem [Database_Result].

## Chèn

[DB::insert] trả về mộ mảng hai giá trị: giá trị id chèn cuối cùng và số hàng bị ảnh hưởng.
	
	$insert = DB::insert('tools')
		->columns(array('name', 'model', 'description'))
		->values(array('Skil 3400 10" Table Saw', '3400', 'Powerful 15 amp motor; weighs just 54-pounds'));
		
	list($insert_id, $affected_rows) = $insert->execute();

## Cập nhật & Xóa

[DB::update] và [DB::delete] cả hai trả về số hàng bị ảnh hưởng như một số nguyên.

	$rows_deleted = DB::delete('tools')->where('model', 'like', '3400')->execute();
