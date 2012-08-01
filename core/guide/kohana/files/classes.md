# Classes

TODO: Giới thiệu vắn tắt tới các lớp.

[Mô hình](mvc/models) và [Điều khiển](mvc/controllers) là các lớp tốt, nhưng được đối xử hơi khác nhau bởi Kohana.
Đọc các trang tương ứng của chúng để tìm hiểu thêm.

## Helper or Library?

Kohana 3 không phân biết giữa các lớp "helper" và các lớp "library" giống như các phiên bản trước.
Chúng tất cả được đặt trong thư mục `classes/` và theo các quy tác tương tự.
Khác biệt ở đây là nói chung, một lớp "helper" được sử dụng tĩnh, (ví dụ xem các helper được bao gồm trong Kohana), và các lớp thư viện thông thường được khởi tạo và sử dụng như các đối tượng (giống [Trình xây dựng truy vấn CSDL](../database/query/builder)).
Sự phân biệt không phải là đen và trắng, và dù sao không liên quan, kể từ khi chúng được đối xử như nhau bởi Kohana.

## Tạo một lớp

Để tạo ra một lớp mới, chỉ cần đặt một tập tin trong thư mục `classes/` tại bất kỳ điểm nào trong [Hệ thống tập tin phân cấp](files), mà theo [Quy tắc đặt tên lớp](conventions#class-names-and-file-location).
Ví dụ, cho phép tạo ra một lớp `Foobar`.

	// classes/foobar.php
	
	class Foobar {
		static function magic() {
			// Làm điều gì đó
		}
	}
	
Chúng ta bây giờ có thể gọi `Foobar::magic()` ở bất kỳ đâu và Kohana sẽ [nạp tự động](autoloading) tập tin cho chúng ta.

Chúng ta cũng có thể đặt các lớp trong các thư mục con.

	// classes/professor/baxter.php
	
	class Professor_Baxter {
		static function teach() {
			// Làm điều gì đó
		}
	}
	
Chúng ta giờ có thể gọi `Professor_Baxter::teach()` ở bất cứ đâu chúng ta muốn.

Đối với các ví dụ về cách tạo và sử dụng các lớp, chỉ cần nhìn vào các thư mục 'classes' trong `system` hoặc bất kỳ mô-đun nào.

## Không gian tên các lớp của bạn

TODO: Thảo luận không gian tên để cung cấp chức năng mở rộng trong suốt trong các lớp/mô-đun của riêng bạn.
