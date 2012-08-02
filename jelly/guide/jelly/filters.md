# Bộ lọc

Các bộ lọc được sử dụng để định dạng dữ liệu trước khi nó được chèn vào cơ sở dữ liệu.
Bạn có thể định nghĩa các các bộ lọc giống cách bạn định nghĩa [các quy tắc xác nhận](validation).
Sự khác biệt duy nhất là bạn không có khả năng truyền đổi tượng xác nhận (ký tự đại diện `:validation`) trong các tham số như là các bộ lọc mà không là một phần của xác nhận hợp lệ.

## Thêm bộ lọc
Để thêm các bộ lọc hãy tạo một mảng `filters` khi định nghĩa trường.

	// ...
	'name' => Jelly::field('string', array(
		// Mảng cho các bộ lọc
		'filters' => array(),
	)),

### Một ví dụ

	// ...
	'name' => Jelly::field('string', array(
		'filters' => array(
			// Sử dụng hàm 'trim' của PHP có sẵn để loại bỏ các khoảng trắng
			array('trim'),
			// Sử dụng mộ bộ lọc được định nghĩa trong mô hình này và được đặt tên là 'custom_filter' để định dạng giá trị
			array(array(':model', 'custom_filter')),
			// Sử dụng hàm dựng sẵn của Kohana để làm cho mọi ký tự đầu của từ là chữ hoa
			array('UTF8::ucwords'),
		),
	)),