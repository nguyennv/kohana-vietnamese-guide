# Lập hồ sơ

Kohana cung cấp cách rất đơn giản để hiết thị các thống kê về ứng dụng của bạn:

1. Gọi các phương thức [Kohana] chung, chẳng hạn như [Kohana::find_file()].
2. Các yêu cầu. Bao gôm yêu cầu chính, cũng như bất kỳ yêu cầu phụ.
3. Các truy vấn [Database]
4. Thời gian thực hiện trung bình cho ứng dụng của bạn

[!!]  Để việc lập hồ sơ làm việc, thiết lập `profile` phải là TRUE trong lời gọi [Kohana::init()] của bạn trong tập tin khởi động của bạn.

## Lập hồ sơ mã của bạn

Bạn có thể dễ dàng thêm việc lập hồ sơ cho mã và các hàm của riêng bạn.
Việc này được thực hiện bằng cách sử dụng hàm [Profiler::start()].
Tham số đầu tiên là nhóm, tham số thứ hai là tên của benchmark.

	public function foobar($input)
	{
		// Hãy chắc chắn là hồ sơ duy nhất nếu nó được kích hoạt
		if (Kohana::$profiling === TRUE)
		{
			// Bắt đầu một đánh giá
			$benchmark = Profiler::start('Your Category', __FUNCTION__);
		}

		// Làm vài cái gì đấy

		if (isset($benchmark))
		{
			// Ngừng đánh giá
			Profiler::stop($benchmark);
		}

		return $something;
	}

## Cách đọc báo cáo lập hồ sơ

Các đánh giá được sắp xếp vào trong các nhóm.
Mỗi đánh giá sẽ hiển thị tên của nó, bao nhiêu lần nó đã được chạy (hiển thị trong dấu ngặc đơn sau tên đánh giá), và thời gian nhỏ nhất, lớn nhát, trung bình và tổng và bộ nhớ dành cho đánh giá đó.
Cột tổng số sẽ có nền mờ hiển thị thời gian tương đối giữa các đánh giá trong cùng nhóm.

Ở cuối là một nhóm được gọi là "Application Execution".
Nhóm này theo dõi mỗi lần thực thị đã thực hiện bao lâu.
Số ở trong dấu ngoặc đơn là bao nhiêu lần việc thực thi đang được so sánh.
Nó hiển thị thời gian nhanh nhất, chậm nhất, và trung bình và bộ nhớ sử dụng của vài yêu cầu cuối.
Hộp cuối cung là thời gian và bộ nhớ sử dụng của yêu cầu hiện thời.

((Mục này có thể sử dụng một hình ảnh của lập hồ sơ với vài truy vấn cơ sở dữ liệu, v.v. với các chú thích chỉ ra tằng vùng như vừa mô tả.))

## Hiển thị hồ sơ

Bạn có thể hiển thị hoặc thu thập thống kế [profiler] hiện thời tại bất kỳ thời điểm nào:

    <?php echo View::factory('profiler/stats') ?>

## Xem trước

(Đây là thống kê lập hồ sơ thực sự cho trang này.)

{{profiler/stats}}