# Mô hình

Từ Wikipedia:

 > Mô hình quản lý các hành vi và dữ liệu của miền ứng dụng,
 > đáp ứng tới yêu cầu thông tin về trạng thái của nó (thường là từ khung nhìn),
 > và đáp ứng với các hướng dẫn để thay đổi trạng thái (thường từ điều khiển).

Tạo một mô hình đơn giản:

	class Model_Post extends Model
	{
		public function do_stuff()
		{
			// Đây là nơi bạn thực hiện miền logic...
		}
	}

Nếu bạn muốn truy cập cơ sở dữ liệu, phải mở rộng mô hình của bạn từ lớp Model_Database:

	class Model_Post extends Model_Database
	{
		public function do_stuff()
		{
			// Đây là nơi bạn thực hiện miền logic...
		}

		public function get_stuff()
		{
			// Nhận những thứ từ cơ sở dữ liệu:
			return $this->db->query(...);
		}
	}

Nếu bạn muốn khả năng CRUD/ORM, xem [mô-đun ORM](../../guide/orm)
