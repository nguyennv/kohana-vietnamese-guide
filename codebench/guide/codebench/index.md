# Sử dụng Codebench

[!!] Nội dung của trang này được đưa ra (với một vài thay đổi nhỏ) từ <http://www.geertdedeckere.be/article/introducing-codebench> và là bản quyển của Geert De Deckere.

Trong một thời gian dài Tôi đã sử dụng một tập tin nhanh-và-bẩn `benchmark.php` để tối ưu hoá các mẩu mã PHP, nhiều lần biểu thức chính quy có liên quan đến các thứ.
Tập tin không chứa nhiều hơn một hàm [gettimeofday](http://php.net/gettimeofday) bao bọc quanh một vòng lặp `for`.
Nó đã làm việc, tuy không thật hiệu quả.
Một cái gì đó vững chắc hơn là cần thiết.
Tôi đặt ra để tạo một miến có thể sử dụng hơn nhiều của phần mềm để hỗ trợ trong việc truy tìm vĩnh viễn để siết chặt mỗi phần nghìn giây ra khỏi các biểu thức chính quy đó.

## Mục tiêu Codebench

### Đánh giá tiêu chuẩn nhiều biểu thức chính quy cùng một lúc

Có thể so sách tốc độ của một số lượng tuỳ ý các biểu thức chính quy cực kỳ hữu ích.
Trong trường hợp bạn phân vân - vâng, Tôi đã được viết lại các lần tiêu chuẩn cho mỗi biểu thức chính quy, bỏ chú thích cho chúng từ thứ một.
Bạn nhận được ý tưởng.
Những ngày đó được đi mãi mãi bây giờ.

### Đánh giá tiêu chuẩn nhiều chủ đề cùng một lúc

Những gì được xem xét quá thường xuyên khi thử nghiệm và tối ứu hoá các biểu thức chính quy là thực tế rằng tốc độ có thể khác nhau đáng kể phụ thuộc vào các chủ đề, còn gọi là đầu vào hoặc các chuỗi mục tiêu.
Chỉ vì biểu thức chính quy của bạn khớp, nói rằng, một địa chỉ email hợp lệ một cách nhanh chóng, không có nghĩa nó sẽ nhanh chóng nhận ra một địa chỉ email không hợp lệ được cung cấp.
Tôi có kế hoạch viết một bài viết tiếp theo với các ví dụ biểu thức chính quy trên tay để chứng minh điểm này.
Dù sao, Codebench cho phép bạn tạo một mảng các chủ đề cái mà sẽ được truyền tới mỗi đánh giá tiêu chuẩn.

### Làm cho nó đủ linh hoạt để làm việc cho tất cả hàm PCRE

Ban đầu Tôi đã đặt tên mô-đun “Regexbench”.
Tôi nhanh chóng nhận ra, mặc dù, nó sẽ đủ linh hoạt để đánh giá tiêu chuẩn tất cả loại mã PHP, vì thế mà thay đổi thành “Codebench”.
Trong khi các công cụ được xây dựng đặc biết để giúp lập hồ sơ các hàm PCRE, như [preg_match](http://php.net/preg_match) hoặc [preg_replace](http://php.net/preg_replace), chắc chắn phải sử dụng của chúng, linh hoạt hơn là cần thiết ở đây.
Bạn có thể so sánh tất cả các loại cấu trúc ở đây như việc kết hợp các hàm PCRE và các hàm chuỗi PHP gốc.

### Tạo các trường hợp đánh giá tiêu chuẩn sạch và khả chuyển

Việc ném dữ liệu đánh giá tiêu chuẩn có giá trị đi mỗi khi Tôi cần tối ưu biểu thức chính quy khác đã phải dừng lại.
Một tập tin sạch chứa tập hoàn chỉnh tất cả các biến thể biểu thức chính quy để so sánh, vùng với các tập của chủ đề để kiểm tra chúng với nhau, sẽ được chào đón nhiều hơn.
Hơn nữa, nó có thể dễ dàng trao đổi các trường hợp đánh giá tiêu chuẩn với các trường hợp khác.

### Trực quan hóa các đánh giá tiêu chuẩn

Rõ ràng là việc cung cấp một trình diễn trực quan cho các kết quả đánh giá tiêu chuẩn, thông qua các đồ thị đơn giản, sẽ thực hiện phiên dịch chúng dễ dàng hơn.
Phải không nghĩ về Internet Explorer một lần, thực hiện việc viết CSS nhiều hơn cả dễ dàng và vui vẻ.
Nó dẫn tới một vài đồ thị đẹp cái mà thay đôi kích cỡ đầy đủ.

Dưới đây là hai ảnh chụp màn hình Codebench trong hành động.
`Valid_Color` là một lớp làm cho việc đánh giá tiêu chuẩn các cách khác nhau để xác nhận các giá trị màu HTML hệ thập lục phân, v.d #FFF.
Nếu bạn quan tâm đến câu chuyện đằng sau các biểu thức thông thường thực sự, hãy xem [chủ đề này trong các diễn đàn Kohana](http://forum.kohanaphp.com/comments.php?DiscussionID=2192).

![Benchmarking several ways to validate HTML color values](codebench_screenshot1.png)
**Đánh giá tiêu chuẩn bảy cách để xác nhận các giá trị màu HTML**

![Collapsable results per subject for each method](codebench_screenshot2.png)
**Các kết quả có thể thu lại mỗi chủ để cho mỗi phương thức**

## Làm việc với Codebench

Codebench được bao gồm trong Kohana 3, nếu bạn cần bạn [có thể tải về nó](http://github.com/kohana/codebench/) từ GitHub.
Hãy chắc chắn Codebench được bật trong `application/bootstrap.php` của bạn.

Việc tạo đánh giá tiêu chuẩn của bạn chỉ là một vấn đề của việc tạo một lớp mà mở rộng lớp Codebench.
Lớp nên nằm trong `classes/bench` và tên lớp nên có tiền tố `Bench_`.
Đặt các phần mã bạn muốn so sánh vào trong các phương thức riêng biệt.
Hãy chắc chắn đặt tiền tố các phương thức đó với `bench_`, các phương thức khác sẽ không được đánh giá tiêu chuẩn.
Lướt qua các tập tin trong `modules/codebench/classes/bench/` cho các ví dụ thêm.

Dưới đây là một ví dụ ngắn khác với một vài giải thích thêm.
	
	// classes/bench/ltrimdigits.php
	class Bench_LtrimDigits extends Codebench {
	
		// Một vài chú thích giải thích tuỳ chọn về các tập tin đánh giá tiêu chuẩn.
		// HTML cho phép. Các URL sẽ được chuyển đổi tới các liên kết một cách tự động.
		public $description = 'Chopping off leading digits: regex vs ltrim.';
	
		// Bao nhiêu thời gian để thực thi mỗi phương thức cho mỗi chủ đề.
		// Tổng số vòng lặp = vòng lặp * số lượng phương thức * số lượng chủ đề
		public $loops = 100000;
	
		// Các chủ đề cung cấp một cách lặp lại tới các phương thức đánh giá tiêu chuẩn của bạn.
		public $subjects = array
		(
			'123digits',
			'no-digits',
		);
	
		public function bench_regex($subject)
		{
			return preg_replace('/^\d+/', '', $subject);
		}
	
		public function bench_ltrim($subject)
		{
			return ltrim($subject, '0..9');
		}
	}
	
	

Và người chiến thắng là… [ltrim](http://php.net/ltrim). Chúc mừng đánh giá điểm chuẩn!
