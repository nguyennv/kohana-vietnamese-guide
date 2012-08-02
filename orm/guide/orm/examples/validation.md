# Ví dụ xác nhận hợp lệ

Ví dụ này sẽ tạo tài khoản người dùng và làm thấy rõ cách xử lý xác nhận hợp lệ mô hình và điều khiển.
Chúng ta sẽ tạo một mẫu biểu, xử lý nó, và hiển thị bất kỳ lỗi nào tới người dùng.
Chúng ta sẽ giả thiết rằng một lớp `Model_User` chứa một phương thức được gọi là `hash_password` mà được sử dụng để chuyển các mật khẩu thô thành vài kiểu băm.
Việc thực hiện phương thức băm vượt qua phạm vi của ví dụ này và sẽ được cung cấp với thư viện Xác thực bạn quyết định sử dụng.

## Lược đồ SQL

	CREATE TABLE IF NOT EXISTS `members` (
	  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
	  `username` varchar(32) NOT NULL,
	  `password` varchar(100) NOT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB  DEFAULT CHARSET=utf8 AUTO_INCREMENT=1;

## Mô hình
	
	<?php defined('SYSPATH') or die('No direct access allowed.');

	class Model_Member extends ORM {

		public function rules()
		{
			return array(
				'username' => array(
					array('not_empty'),
					array('min_length', array(':value', 4)),
					array('max_length', array(':value', 32)),
					array(array($this, 'username_available')),
				),
				'password' => array(
					array('not_empty'),
				),
			);
		}
		
		public function filters()
		{
			return array(
				'password' => array(
					array(array($this, 'hash_password')),
				),
			);
		}

		public function username_available($username)
		{
			// Có các cách đơn giản để làm việc này, nhưng Tôi sẽ sử dụng ORM vì lợi ích của ví dụ
			return ORM::factory('member', array('username' => $username))->loaded();
		}

		public function hash_password($password)
		{
			// Làm gì đó để băm mật khẩu
		}
	}

## Mẫu biểu HTML

Vui lòng tha thứ mẫu biểu xấu xí một chút của tôi.
Tôi đang có gắng không sử dụng bất kỳ mô-đun nào hoặc ma thuật không liên quan. :)

	<form action="<?= URL::site('/members'); ?>" method="post" accept-charset="utf-8">
		<label for="username">Tên truy cập:</label>
		<input id="username" type="text" name="username" value="<?= Arr::get($values, 'username'); ?>" />
		<label for="username" class="error"><?= Arr::get($errors, 'username'); ?>

		<label for="password">Mật khẩu:</label>
		<input id="password" type="password" name="password" value="<?= Arr::get($values, 'password'); ?>" />
		<label for="password" class="error"><?= Arr::get($errors, 'password'); ?>

		<label for="password_confirm">Nhắc lại mật khẩu:</label>
		<input id="password_confirm" type="password" name="_external[password_confirm]" value="<?= Arr::path($values, '_external.password_confirm'); ?>" />
		<label for="password_confirm" class="error"><?= Arr::path($errors, '_external.password_confirm'); ?>

		<button type="submit">Create</button>
	</form>

## Điều khiển

[!!] Hãy nhớ rằng `password` sẽ được băm ngay khi nó được thiết lập trong mô hình, vì lý nó này, nó không thể xác nhận chiều dài của nó hoặc thực tế rằng nó khớp với trường `password_confirm`. Mô hình không nên quan tâm về việc xác nhận trường `password_confirm`, vì vậy chúng ta thêm logic đó tới điều khiển và chỉ đơn giản hỏi mô hình để đóng gói các lỗi vào trong một mảng gọn gàng. Đọc phần [bộ lọc](filters) để hiểu cách chúng làm việc.

	public function action_create()
	{
		$view = View::factory('members/create')
			->set('values', $_POST)
			->bind('errors', $errors);

		if ($_POST)
		{
			$member = ORM::factory('member')
				// Phương thức ORM::values() là đường tắt để gán nhiều giá trị cùng một lúc
				->values($_POST, array('username', 'password'));

			$external_values = array(
				// Mật khẩu chưa băm cần so sánh với trường password_confirm
				'password' => Arr::get($_POST, 'password'),
			// Thêm tất cả giá trị bên ngoài
			) + Arr::get($_POST, '_external', array());
			$extra = Validation::factory($external_values)
				->rule('password_confirm', 'matches', array(':validation', ':field', 'password'));

			try
			{
				$member->save($extra);
				// Điều hướng người dùng tới trang của anh ta
				$this->request->redirect('members/'.$member->id);
			}
			catch (ORM_Validation_Exception $e)
			{
				$errors = $e->errors('models');
			}
		}

		$this->response->body($view);
	}

## Thông điệp

**application/messages/models/member.php**

	return array(
		'username' => array(
			'not_empty' => 'Bạn phải cung cập một tên truy cập.',
			'min_length' => 'Tên truy cập phải có ít nhất :param2 ký tự.',
			'max_length' => 'Tên truy cập phải có ít hơn :param2 ký tự.',
			'username_available' => 'Tên truy cập này không có sẵn.',
		),
		'password' => array(
			'not_empty' => 'Bạn phải cung cấp một mật khẩu.',
		),
	);

**application/messages/models/member/_external.php**

	return array(
		'password_confirm' => array(
			'matches' => 'Các trường mật khẩu không khớp.',
		),
	);
