# Callback

Callback có thể được gắn cho các đối tượng form hoặc trường, và có thể chạy dưới hai điều kiện sau khi xác nhận hợp lệ:

1. Callback chạy sau khi form hoặc trường vượt qua xác nhân hợp lệ ("pass")
2. Callback chạy sau khi form hoặc trường thất bại xác nhận hợp lệ ("fail")

Callback được chạy ngay lập tức sau khi các đối tượng được gắn để vượt qua hoặc không vượt qua xác nhận hợp lệ.
Như vậy, nếu một callback "vượt qua" được đính kèm tới một trường, nó sẽ chạy tại thời điểm sau khi trường đó vượt qua xác nhận hợp lệ.

Nếu cùng một callback được đính kèm tới một đối tượng form, nó sẽ được chạy sau khi form cha vượt qua xác nhận hợp lệ.

## Form callback

Callback cho bất kỳ trường nào trong form có thể được đính kèm tới một đối tượng form.

Đây là một ví dụ  của việc đính kèm một callback `some_model::post_process` tới một trường, và `another_model::post_process` tới một form mà cả hai chạy sau khi form vượt qua xác nhận hợp lệ và callback `my_model::failed_validation` khi form thất bại xác nhận hợp lệ.

	$form->callbacks(array(
		'pass' => array(
			':self' => array(
				array('another_model::post_process', array(':field'))
			),
			'email' => array(
				array('some_model::post_process', array(':value', ':last_val'))
			)
		),
		'fail' => array(
			'username' => array(
				array('my_model::failed_validation(':value', ':last_val'))
			)
		),
	));

## Thêm callback

### Thêm callback như một nhóm

Bạn có thể thêm một nhóm cac callback tới một đối tượng bằng cách sử dụng phương thức `callbacks`.

[!!] Khi đính kèm một callback tới một đối tượng form của riêng nó. xem nó như là `:self`

	$form->callbacks(array(
		'pass' => array(
			':self' => array(
				array('another_model::post_process', array(':field')),
				array('another_model2::post_process', array(':field', ':value'))
			),
			'email' => array(
				array('some_model::post_process', array(':value', ':last_val'))
			)
		),
		'fail' => array(
			':self' => array(
				array('some_method', array(':field', ':value'))
			),
			'username' => array(
				array('my_model::failed_validation(':value', ':last_val'))
			)
		),
	));

Khi đính kèm các callback tới một trường, bạn không chỉ định một tên trường.
Tất cả callback đã đính kèm tới trường này áp dụng cho đối tượng trường.

	$form->email->callbacks(array(
		'pass' => array(
			array('some_method', array(':value')),
			array('foo::bar', array(':field'))
		),
		'fail' => array(
			array('foobar', array(':value'))
		),
	));

### Thêm các callback riêng lẻ

Sử dụng `callback` để đính kèm callback riêng lẻ tới một form hoặc một trường.
	
Đối với một form, cú pháp như sau:

`callback(type, alias, method, [values])`

- type ("pass" hoặc "fail")
- alias (":self" cho đối tượng form)
- method
- values (mặc định `NULL`)

~~~
$form->callback('pass', 'email', 'foo::bar', array(':value'));
~~~

Đối với một trường, cú pháp như sau:

`callback(type, method, [values])`

- type ("pass" hoặc "fail")
- method
- values (mặc định `NULL`)

~~~
$form->$field->callback('fail', 'foo::bar', array(':field', ':value'));
~~~

## Định ngĩa các callback tại lúc tạo form hoặc trường

Tạo một mảng các callback mà trông giống như mảng đính kèm bằng cachs sử dụng phương thức `callback`.

~~~
$form = Formo::form(array(
	'callbacks' => array(
		'pass' => array(
			':self' => array(
				array('foo::bar', array(':field'))
			)
			'some_field' => array(
				array('foobar', array(':field', ':value', ':last_val'))
			),
		),
		'fail' => $fail_callbacks
	)
));
~~~

Thêm một trường
~~~
$form->add('username', array(
	'callbacks' => array(
		'pass' => array(
			array('foo::bar'),
			array('foobar', array(':value', 'last_val'))
		),
		'fail' => $fail_callbacks
	)
));
~~~

## Các tham số đặc biệt của callback

Bạn có thể truyền các tham số đặc biệt tới callback

- `:field` - đối tượng trường hoặc form
- `:value` - giá trị trường hoặc form
- `:last_val` - giá trị trường trước đó