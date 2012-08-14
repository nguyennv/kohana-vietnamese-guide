# Xác nhận hợp lệ một trường đơn

Nó đơn giản là để thực hiện việc xác nhận hợp lệ đối với một trường đơn.
Nó làm việc giống như xác nhận hợp lệ trên form đầy đủ.

### Xác nhận hợp lệ

Cũng giống như ở cấp form, mặc định trường được yêu cầu phải được gửi để thông qua xác nhận hợp lệ.

	$field_validated = $form->username->validate();

Nếu không bạn có thể chỉ xác nhận hợp lệ trường với giá trị hiện thời của nó bằng việc truyền `FALSE`

	$form->username->val($some_value);
	$field_validated = $form->username->validate(FALSE);

### Xác nhận hợp lệ trường đối với giá trị khác

Đôi khi bạn sẽ muốn kiểm tra xem một trường sẽ vượt qua xác nhận hợp lệ trên một giá trị mà vẫn chưa được thiết lập tới trường.
Một cái gì đó như thế này:

	if ($field_passed_validation)
	{
		$form->$field->val($new_val);
	}

Hoặc sử dụng phổ biết khác cho kịch bản này là xác nhận hợp là ajax khi bạn cần kiểm tra nếu chỉ một trường sẽ vượt qua xác nhận hợp lệ.

#### Làm việc với đối tượng xác nhận với quy tắc, nhãn đã sao chép.

Cách bạn làm việc việc này là để nhận một đối tượng xác nhận hợp lệ bạn có thể kiểm tra đối với nó.
Dưới đây là một ví dụ.
Lưu ý một đối tượng xác nhận hợp lệ đầy đủ được lấy ra mà bạn có thể làm việc với nó.

	// Lấy một đối tượng xác nhận hợp lệ và đưa vào nó với giá trị $some_username
	$val = $form->username->validation($some_username);
	$passed = $val->check();
	$errors = $val->errors($message_file);