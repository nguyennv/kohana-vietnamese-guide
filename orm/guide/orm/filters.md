# Bộ lọc

Các bộ lọc trong ORM làm việc giống như chúng được sử dụng khi chúng là một phần của lớp Validate trong 3.0.x tuy nhiên chúng được thay đổi để phù hợp với cú pháp linh hoạt của các quy tắc [Validation] trong 3.1.x.
Các bộ lọc chạy ngay trong trường mà được thiết lập trong mô hình của bạn và sẽ được sử dụng để định dạng dữ liệu trước khi nó được chèn vào trong Cơ sở dữ liệu.

Để định nghĩa các bộ lọc của bạn giống như cách bạn định nghĩa các quy tắc, như một mảng được trả về bởi phương thức `ORM::filters()` giống như sau:

	public function filters()
	{
		return array(
			'username' => array(
				array('trim'),
			),
			'password' => array(
				array(array($this, 'hash_password')),
			),
			'created_on' => array(
				array('Format::date', array(':value', 'Y-m-d H:i:s')),
			),
		);
	}

[!!] Khi định nghĩa các bộ lọc, bạn có thể sử dụng các tham số `:value`, `:field`, và `:model` để tham chiếu tới giá trị trường, tên trường, và thể hiển của mô hình tương ứng.
