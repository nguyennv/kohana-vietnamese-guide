Subform
========

## Mô tả subform

Một subform là một form bên trong một form.
Formo hỗ trợ các đối tượng form sâu và cho phép nhiều form tồn tại bên trong form khác như bạn cho là cần thiết.

## Cơ bản subform

Khi một subform chỉ là một form bên trong một form, nó tạo ra là chỉ giống như một form. Sau đó nó được thêm vào một form cha.

	$address = Formo::form()
		->add('street')
		->add('city')
		->add('zip');
		
	$user_form = Formo::form()
		->add('first name')
		->add('last name')
		->add('address', 'group', $address);
		
Trong ví dụ trên, form *address* được thêm vào *$user_form* sau *last_name*.
Nó được cho bí danh *address* và sử dụng *nhóm* trình điều khiển để xử lý nó.

Nếu bạn muốn truy cập *street*, ví dụ để thiết lập giá trị của nó, bạn có thể làm như thế này:

	$user_form->address->street->val();
	
Ngoài ra, từ khi PHP truyền các đối tượng bằng tham chiếu, bạn cũng có thể truy cập cùng giá trị từ  đối tượng subform ban đầu

	$address->street->val();
	
Lưu ý rằng bạn không phải xác định một trình điều khiển cho đối tượng form của bạn.
Nó sẽ sử dụng trình điều khiển *form* mặc định là một thứ không được chỉ định.
Ví dụ này thêm subform *address* tới *user_form* nhưng trình điều khiển là trình điều khiển form mặc định.

	$user_form->add('address', $address);

## Tạo một subform tốc hành (on the fly) từ các trường hiện có

Bạn có thể muốn tạo không gian tên một phần form của bạn hoặc đơn giản bật tốc hành một vài trường trong một form 
Có nhiều lý do bạn có thể muốn làm việc này, nhưng một là để cho phép "nhóm" hoặc trình điều khiển cụ thể khác vì lý do định dạng.

Việc làm này luôn luôn gắn thêm subform của bạn tới dưới cùng của đối tượng cha của nó.

Để làm việc này, hãy sử dụng `create_sub`:

	$form = Formo::form()
		->add('username')
		->add('email')
		->add('password')
		->add('street')
		->add('city')
		->add('state');
		
	$form->create_sub('address', 'group', array('street', 'city', 'state'), array('before' => 'username'));

[!!] `create_sub()` tạo subform bên trong đối tượng cha của nó và trả về đối tượng subform đó.
	
Trong `create_sub`, các tham số là như sau:

	create_sub(alias, driver, fields, [order])
	
Và nếu thứ tự là một thứ tự tương đối, nó là một mảng 'position' => 'relative_to', nhưng nếu nó là một số cứng, chỉ cần chỉ định con số.
Subform sẽ được ở cùng một chỗ như trên nếu bạn đã làm như thế này:

	$form->create_sub('address', 'group', array('street', 'city', 'state'), 0);