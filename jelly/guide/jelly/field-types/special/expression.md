#### `Jelly_Field_Expression`

Trường này là một kiểu khá trừu tượng cho phép bạn kéo một biểu thức cơ sở dữ liệu trở lại trên các SELECT.
Đơn giản chỉ cần thiết lập `column` của bạn tới bất kỳ `DB::expr()` nào.

Ví dụ, nếu bạn luôn luôn muốn trường trả về một xâu chuỗi của hai cột trong cơ sở dữ liệu, bạn có thể làm được điều này:

	'field' => Jelly::field('expression', array(
		'column' => DB::expr("CONCAT(`first_name`, ' ', `last_name`)"),
	))

Nó có thể bỏ ép kiểu giá trị trả về bằng cách sử dụng một trường Jelly.
Việc này được định nghĩa trong thuộc tính `cast`:

	'field' => Jelly::field('expression', array(
		'cast'   => 'integer', // Thuộc tính này sẽ ép kiểu trường bằng cách sử dụng Jelly::field('integer')
		'column' => DB::expr("CONCAT(`first_name`, ' ', `last_name`)"),
	))

[!!] Hãy nhớ rằng việc đặt bí danh bị phá vỡ trong Database_Expressions.

[Tài liệu API](../api/Jelly_Field_Expression)