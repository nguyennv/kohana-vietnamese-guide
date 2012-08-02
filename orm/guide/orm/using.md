# Cách dùng cơ bản

## Nạp một thể hiện mô hình mới

Để tạo một thể hiện `Model_User` mới, bạn có thể làm một trong hai cách:

	$user = ORM::factory('user');
	// Hoặc
	$user = new Model_User();

## Chèn dữ liệu

Để chèn một bản ghi mới vào cơ sở dữ liệu, hãy tạo một thể hiện mới của mô hình:

	$user = ORM::factory('user');

Sau đó, gán giá trị cho mỗi thuộc tính;

	$user->first_name = 'Trent';
	$user->last_name = 'Reznor';
	$user->city = 'Mercer';
	$user->state = 'PA';

Chèn bản ghi mới vào cơ sở dữ liệu bằng cách chạy [ORM::save]:

	$user->save();

[ORM::save] kiểm tra xem nếu một giá trị được thiết lập cho khoá trính (mặc định là `id`).
Nếu khoá chính được thiết lập, thì ORM sẽ thực thi một `UPDATE`, nếu không nó sẽ thực thiện một `INSERT`.


## Tìm một đối tượng

Để tim một đối tượng bạn có thể gọi phương thức [ORM::find] hoặc truyền id vào hàm dựng ORM:

	// Tìm người dùng với ID 20
	$user = ORM::factory('user')
		->where('id', '=', 20)
		->find();
	// Hoặc
	$user = ORM::factory('user', 20);

## Kiểm tra ORM đã nạp một bản ghi

Sử dụng phương thức [ORM::loaded] để kiểm tra ORM đã nạp thành công một bản ghi.

	if ($user->loaded())
	{
		// Nạp thành công
	}
	else
	{
		// Lỗi
	}

## Cập nhật và lưu dữ liệu

Một khi một mô hình ORM đã được nạp, bạn có thể sửa đổi các thuộc tính của mô hình như thế này:

	$user->first_name = "Trent";
	$user->last_name = "Reznor";

Và nếu bạn muốn lưu các thay đổi thực hiện trở lại tới cơ sở dũ liệu, chỉ cần chạy một lời gọi `save()` như thế này:

	$user->save();



## Xoá dữ liệu


Để xoá một đối tượng, bạn có thể gọi phương thức [ORM::delete] trên một mô hình ORM đã nạp.

	$user = ORM::factory('user', 20);
	$user->delete();

