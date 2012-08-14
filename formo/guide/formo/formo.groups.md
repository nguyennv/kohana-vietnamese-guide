Nhóm
======

Các hộp chọn, nhóm nút radio và nhóm nút chọn là tất cả ví dụ của các trường mà dựa trên một tập *các tùy chọn* để chọn từ chúng.

Vì nó thường cồng kềnh để chỉ định tham số các tùy chọn trong mảng tùy chọn khi thêm hoặc tạo một trường, Formo cung cấp phương thức tiện lợi `add_group()`.

## Các dùng add_group

Các tham số:

* alias
* driver
* options
* value
* settings (tùy chọn)

Lưu ý rằng nói chung quy tắc hàm tạo Formo không áp dụng tới `add_group`.
Mỗi trường đã được xác định rõ ràng với trường ngoại lệ *thiết lập*, cái mà là tùy chọn.

## Ví dụ

Đối các ví dụ sau, chúng ta sẽ sử dụng mảng các tùy chọn này:

	$options = array
	(
		'run' => 'Chạy',
		'swim' => 'Bơi',
		'bike' => 'Đi xe đạp',
		'hike' => 'Đi bộ đường dài',
	);
	
Bạn có thể hoặc sử dụng cặp `alias => value` hoặc chỉ định cụ thể tất cả các tham số để điều khiển.

Nếu không có `add_group()`, bạn sẽ phải làm như thế này:

	$form->add('hobbies', 'select', 2, array('options' => $options));
	
Nhưng `add_group()` làm cho cú pháp rõ ràng hơn:

	$form->add_group('hobbies', 'select', $options, 2);