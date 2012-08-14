# Lỗi xác nhận hợp lệ Formo

Tài liệu này sẽ giúp bạn có được sự hiểu biết tốt hơn về việc làm với các lỗi xác nhận hợp lệ bên trong Formo.

## Lỗi và các lỗi
Mỗi đối tượng form, subform và trường có thể chứa một thông báo lỗi.

Thêm vào đó, mỗi form/subform theo dõi tất cả các lỗi bên trong nó.

## Thiết lập và lấy lỗi
Sau đây cho biết thêm một thông báo lỗi cho một form hoặc một trường.
Tham số đầu tiên là thông báo và tham số tùy chọn thứ hai là việc dịch các thông báo.

	// Đính kèm một thông báo lỗi
	$field->error('Điều này là sai, sai sai');

Bạn lấy lỗi với

	$field->error();

## Các lỗi form/subform
Bạn có thể lấy một mạng các lỗi của một form hoặc của các subform với

	$form->errors([$file], [$translate = TRUE]);

Các thông báo lỗi là phân cấp. Hãy khám phá một form với một subform.

Trong ví dụ này, giả sử mọi trường có quy tắc 'not_empty'.

	$address = Formo::form()
		->add('street')
		->add('zip');

	$form = Formo::form()
		->add('car', 'select', $hobbies)
		->add('address', $address);

Khi form submit rỗng, các lỗi sau dây tồn tại:

	$errors = $form->errors();
	$errors2 = $address->errors();

Trả về


	//$errors:
	array
	(
		'fields' => array
		(
			'car' => 'car không được để trống',
			'address'	=> array
			(
				'street'	=> 'street không được để trống',
				'zip'		=> 'zip không được để trống',
			)
		)
	)

	//$errors2:
	array
	(
		'fields' => array
		(
			'street'	=> 'street không được để trống',
			'zip'		=> 'zip không được để trống',
		)

	)

## Các lỗi form/subform

Nếu form hoặc subform tự bản thân có một lỗi, nó được bao gồm trong danh sách các lỗi của trường ở dưới khóa 'form'.
Ở đây là ví dụ của mảng các lỗi ở nơi mà form có lỗi.

	array
	(
		'form' => 'Tên truy cập hoặc mật khẩu không đúng',
	);

## Truyền tham số thông báo lỗi

Thực hiện theo hương dẫn sau đây trong hướng dẫn Kohana để truyền các tham số thông báo tới các thông bao lỗi. Đây là một ví dụ:

	public static function my_rule($validation, $field, $value)
	{
		if (condition_not_met())
		{
			$validation->error($field, 'rule_name', array($param1_name, $param2_name));
			return;
		}
		
		return TRUE;
	}
	
[!!] Nếu bạn rõ ràng thiết lập lỗi trên đối tượng xác nhận hợp lệ trong quy tắc, bạn cần trả về `void` thay vì `FALSE`.
## Tổng kết

Bạn có thể đính kèm các thông báo lỗi tới bất kỳ đối tượng Formo nào, nhưng các đối tượng form và subform có thể lấy một mảng của tất cả các lỗi bên trong chính nó.

Xác nhận hợp lệ thất bại trêm một subform dựa trên logic theo thứ tự này:

1. Nó có một thông báo lỗi
1. Nó chứa thông báo lỗi bên trong các trường của nó
