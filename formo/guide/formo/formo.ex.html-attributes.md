# Xác định thuộc tính HTML

Thuộc tính HTML là các thuộc tính bên trong thẻ HTML.
Bạn thường cần xác định những thuộc tính khác nhau này khi làm việc với các form.

### Thuộc tính HTML là gì?

Chúng ta sẽ sử dụng trường sau như ví dụ của chúng ta:

	<input type="email" name="email" onclick="method" placeholder="some text" class="someclass" id="someid" />

Trong Formo, thuộc tính `name="email"` và `type="email"` được xác định ở bất cứ nơi đâu, nhưng các thuộc tính chung khác được định nghĩa như `attr`.

### Định nghĩa các thuộc tính bên ngoài khung nhìn

Vào lúc tạo trong mảng tùy chọn

	$form->add('email', 'email', array('type' => 'email', 'attr' => array('onclick' => 'method', 'id' => 'someid', 'class' => 'someclass')));
	
Sử dụng `set`

	$form->email->set('attr', array('onclick' => 'method', 'id' => 'someid', 'class' => 'someclass'));

### Định nghĩa các thuộc tính bên trong khung nhìn

	$this->attr('id', 'someid')->attr('onclick', 'method')->add_class('someclass');