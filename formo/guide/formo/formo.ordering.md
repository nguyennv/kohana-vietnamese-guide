Thứ tự các trường
===============

Bạn có thể chỉ định một vị trí cho một trường mới được chèn hoặc di chuyển các trường xung quanh.

### Thứ tự trường mới

Tùy chọn mà mô tả một vị trí đặc biệt cho trường mới của bạn được nối thêm vào là "order".
Bạn có thể hoặc sử dụng một số cho một vị trí cụ thể hoặc đặt nó có liên quan đến trường khác.

#### Vị trí cụ thể

Ví dụ sau thêm 'email' tại một vị trí cụ thể:

	$form = Formo::form()
		->add('username')
		->add('password')
		->add('email', array('order' => 1));
		
Thứ tự bắt đầu tại 0, bởi vậy form ở trên thêm các trường theo thứ tự sau:

	'username', 'email', 'password'
	
#### Thứ tự liên quan

Bạn cũng có thể chọn một vị trí liên quan tới trường khác.
Các lựa chọn là "before" và "after".

	$form = Formo::form()
		->add('username')
		->add('password')
		->add('email', array('order' => array('before', 'password')))
		->add('firstname', array('order' => array('after', 'email')));
		
Ví dụ này đặt các trường theo thứ tự sau:

	'username', 'email', 'firstname', 'password'

### Phương thức order

Di chuyển các trường xung quanh sau khi tạo bằng cachs sử dụng phương thức `order`.

	$form->order(array('email' => 1));
	
Hoặc
	
	$form->order(array(
		'email' => 2,
		'username' => array('before', 'first_name'),
		'password' => array('after', 'last_name'),
	));

### Giới hạn

Bạn không thể để dành chỗ khi sắp xếp các trường.
Tức là, nếu một trường được chỉ đình như là "before email" và email được thêm sau đó, trường gốc sẽ không di chuyển.

Thứ tự luôn luôn được dựa trên các trường đã được thêm tới form hoặc subform.

Formo sẽ không chọn lọc một các ma thuật thông qua chiều sâu của subform của bạn khi chỉ định một thứ tự.
Bạn phải hoặc là chỉ định thứ tự dựa trên ngữ cảnh của trường.
