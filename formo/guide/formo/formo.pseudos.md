## Ngữ cảnh giả - pseudo

Nếu bạn cần xác định một ngữ cảnh mà không có sẵn cho đến thời điểm xác nhận hợp lệ cho các phương thức quy tắc của bạn để chạy với nó, bạn có thể sử dụng ngữ cảnh giả.
Các quy tắc này theo quy ước của Kohana 3 được viết với một dấu hai chấm (":") theo sau bởi tên của ngữ cảnh (v.d ":model").

Các ngữ cảnh có sẵn là:

* `:model` - Cho đối tượng mô hình
* `:field` - Cho đối tượng trường. Ngữ cảnh này cũng là ngữ cảnh mặc định cho một quy tắc
* `:parent` - Đối tượng cha trực tiếp của trường
* `:form` - Đối tượng cha trên cùng của form

Cú pháp để sử dụng các ngữ cảnh giả trông giống như một hàm tĩnh đơn giản.
Đây là một ví dụ bên trong một phương thức khởi tạo của mô hình Jelly:

	->fields(array(
		'username'	=> array
		(
			'rules'	=> array
			(
				':model::myrule' => array(25),
				':field::another_rule'	=> NULL,			
			)
		),
	))

Trong trường hợp này, phương thức nahf được chạy như một quy tắc Validator:

	$model->myrule(25);
	$field->another_rule($field->val());
	
Điều tốt đẹp về vấn đề này là bạn có được để làm việc với toàn bộ thể hiện của mô hình thay vì chỉ một phương thức tĩnh bên trong phương thức quy tắc.

## Tham số giả

Các tham số giả làm việc giống như ngữ cảnh giả cho các quy tắc, chỉ có chúng là các vết chích mà tham chiếu tới các tham số mà chỉ có sẵn khi xác nhận hợp lệ được gọi.
Các tham số cùng tồn tại với việc bổ xung ":value" và ":alias":

* `:value` - Giá trị của trường
* `:alias` - Bí danh của trường
* `:field` - Đối tượng trường
* `:parent` - Đối tượng cha trực tiếp của trường
* `:form` - Đối tượng cha cao nhất của trường
* `:model` - Mô hình

Với tham số giả, bạn có thể dễ dàng truyền bất cứ gì bạn cần để thực hiện đầy đủ các kiểm tra quy tắc phức tạp.

Chú ý rằng không giống Validate của Kohana, Validator của Formo không buộc bạn phải truyền các tham số theo một thứ tự cụ thể bởi quy ước.
Nếu không có tham số được quy định rõ ràng, giá trị của trường được truyền như là tham số duy nhất.

Nếu bạn định nghĩa bất kì tham số nào, bạn sẽ phải chỉ định tham số như là giá trị của trường.

Ví dụ:

	'rules' => array
	(
		'not_empty' => NULL,
		'max_length' => array(':value', 32),
		'preg_match' => array('/[a-z]+/', ':value'),s
	);