# Định nghĩa mô hình

Nơi đầu tiên để bắt đầu với bất kỳ ORM nào là định nghĩa các mô hình của bạn.
Jelly chia tác các mô hình vào trong một vài thành phần riêng biệt mà làm cho API chặt chẽ hơn và có thể mở rộng.

Trước tiên, hãy bắt đầu với một mô hình mẫu:

	class Model_Post extends Jelly_Model
	{
		public static function initialize(Jelly_Meta $meta)
		{
			// Một nhóm cơ sở dữ liệu tuỳ chọn bạn muốn sử dụng
			$meta->db('default');
			
			// Bảng mà mô hình được đính kèm tới
			// Nó mặc định là tên của của mô hình số nhiều
			$meta->table('posts');

			// Tuỳ chọn bạn có thể tự động nạp các quan hệ mỗi lần bạn nạp mô hình
			$meta->load_with('author');
		
			// Các trường được định nghĩa bởi mô hình
			$meta->fields(array(
				'id'      => Jelly::field('primary'),
				'name'    => Jelly::field('string'),
				'body'    => Jelly::field('text'),
				'status'  => Jelly::field('enum', array(
					'choices' => array('draft', 'review', 'published')
				)),
				
				// Quan hệ với các mô hình khác
				'author'   => Jelly::field('belongsto'),
				'comments' => Jelly::field('hasmany'),
				'tags'     => Jelly::field('manytomany'),
			));
		}
	}

Như bạn có thể thấy tất cả mô hình phải làm vài điều để được đăng ký với Jelly:

 * Chúng phải mở rộng `Jelly_Model`
 * Chúng phải định nghĩa một phương thức `initialize()`, cái mà được truyền một đối tượng `Jelly_Meta`
 * Chúng phải thêm các thuôc tính tới đối tượng `$meta` đó để định nghĩa các trường, bảng và một số thứ khác. Hầu hết các mục này là tuỳ chọn.

Phương thức `initialize()` chỉ được gọi một lần mỗi lần thực thi cho mỗi mô hình và đối tượng meta của mô hình được lữu giữ tĩnh.
Nếu bạn cần đề tìm ra bất cứ điều gì về một mô hình cụ thể, bạn có thể sử dụng `Jelly::meta('model')`.

## Các trường Jelly

Jelly định nghĩa [nhiều đối tượng trường](field-types) mà bao gồm các kiểu phổ biến nhất các cột mà được sử dụng trong bảng cơ sở dữ liệu.

Trong Jelly, các đối tượng trường chứa tất cả logic cho việc truy lục, thiết lập, và lưu các giá trị cơ sở dữ liệu.

Vì tất cả các quan hệ được xử lý thông qua các trường quan hệ, nó có thể thực hiện logic quan hệ phức tạp, tuỳ biến trong một mô hình bằng việc [định nghĩa một trường tuỳ biến](extending-field).
