# Ví dụ đơn giản

Đây là một ví dụ đơn giản của một mô hình ORM duy nhất, mà không có quan hệ, nhưng sử dụng xác nhận hợp lệ trên các trường.

## Lược đồ SQL

	CREATE TABLE IF NOT EXISTS `members` (
	  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
	  `username` varchar(32) NOT NULL,
	  `first_name` varchar(32) NOT NULL,
	  `last_name` varchar(32) NOT NULL,
	  `email` varchar(127) DEFAULT NULL,
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
					array('regex', array(':value', '/^[-\pL\pN_.]++$/uD')),
				),
				'first_name' => array(
					array('not_empty'),
					array('min_length', array(':value', 4)),
					array('max_length', array(':value', 32)),
					array('regex', array(':value', '/^[-\pL\pN_.]++$/uD')),
				),
				'last_name' => array(
					array('not_empty'),
					array('min_length', array(':value', 4)),
					array('max_length', array(':value', 32)),
					array('regex', array(':value', '/^[-\pL\pN_.]++$/uD')),
				),
				'email' => array(
					array('not_empty'),
					array('min_length', array(':value', 4)),
					array('max_length', array(':value', 127)),
					array('email'),
				),
			);
		}
	}

[!!] Mảng được trả lại bởi `ORM::rules()` sẽ được truyền tới một đối tượng [Validation] và được kiểm tra khi bạn gọi `ORM::save()`.

[!!] Xin lưu ý rằng việc định nghĩa khoá chính "id" trong mô hình này là không cần thiết. Cũng như tên bảng trong cơ sở dữ liệu là số nhiều và tên mô hình là số ít.

## Điều khiển

	<?php defined('SYSPATH') or die('No direct access allowed.');
	
	class Controller_Member extends Controller_Template {
		
		public function action_index()
		{
			/**
			 * Ví dụ 1
			 */
			
			// Tạo một thể hiện của mô hình
			$members = ORM::factory('member');
			
			// Lấy tất cả thành viên với tên đầu là "Peter" find_all()
			// có nghĩa là chúng ta lất tất cả bản ghi phù hợp với truy vấn.
			$members->where('first_name', '=', 'Peter')->find_all();

			// Đếm các bản ghi trong đối tượng $members
			$members->count_all();
			
			/**
			 * Ví dụ 2
			 */
			
			// Tạo một thể hiện của mô hình
			$member = ORM::factory('member');
			
			// Lấy một thành viên với tên người dùng là "bongo" find() có nghĩa là
			// chúng ta chỉ muốn bản ghi đầu tiên phù hợp với truy vấn.
			$member->where('username', '=', 'bongo')->find();
			
			/**
			 * Ví dụ 3
			 */
			
			// Tạo một thể hiện của mô hình
			$member = ORM::factory('member');
			
			// Thực hiện một truy vấn INSERT
			$member->username = 'bongo';
			$member->first_name = 'Peter';
			$member->last_name = 'Smith';
			$member->save();
			
			/**
			 * Ví dụ 4
			 */
			
			// Tạo một thể hiện của mô hình nơi
			// trường "id" của bảng là "1"
			$member = ORM::factory('member', 1);
			
			// Thực hiện một truy vấn UPDATE
			$member->username = 'bongo';
			$member->first_name = 'Peter';
			$member->last_name = 'Smith';
			$member->save();
		}
	}

[!!] $member sẽ là một đối tượng PHP mà bạn có thể truy cập các giá trị từ truy vấn v.d echo $member->first_name
