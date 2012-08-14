# Tìm hiểu Formo, người bạn tốt nhất của bạn

Mục đích của Formo là để làm việc với các form hướng đối tượng.
Vì vậy, "o" trong "Formo" là viết tắt của "object".

Cụ thể, một form Formo được hiểu như sau:

1. Dữ liệu vào
2. Dữ liệu đã xác nhận hợp lệ
3. Dữ liệu ra

Dữ liệu vào sẽ được gửi tới Formo như là dữ liệu thô.
Dữ liệu có thể là POST, GET, XML, JSON, bất cứ thứ gì.
Formo hoạt động như một trung tập cho việc xác nhận hợp lệ dữ liệu bằng cách sử dụng thư viện Xác nhận hợp lệ dựng sẵn của Kohana.

Dữ liệu ra là bất kỳ loại phản hồi nào.
Có thể là JSON cho một yêu cầu AJAX, có thể là XML tinh khiết trong một phản hồi SOAP, và, tất nhiên một form HTML cho hầu hết các yêu cầu HTML. 

Ngoài ra, Formo có thể hoạt động như một trung tập hữu ích cho dữ liệu.
Một ví dụ của việc này là tích hợp dữ liệu với một mô hình.
Mô hình cung cấp các công cụ cho việc tương tác với CSDL, nhưng Formo cung cấp các công cụ cho việc hiển thị phương pháp để nhận dữ liệu cũng như phản hồi phù hợp tới một yêu cầu.

## Cấu trúc

Tài liệu này giải thích cấu trúc của một đối tượng Formo và cung cấp thông tin cơ bản về từng phần
Hệ thống phân cấp của một đối tượng trông như thế này:

- Formo
- Trình chứa (Container)
	- Trình xác nhận (Validator)
		- Form/Trường
			- ORM
			- Trình điều khiển (Driver)
				- Khung nhìn (View)
			
### Formo

Lớp này hoặt động như một giao tiếp tới các đối tượng Formo.

### Trình chứa

Lớp trình chứa xử lý các khả năng cho một formo hoặc trường để chứa các trường hoặc form khác bên trong chính nó và tìm kiếm chúng.
Nó cũng hiểu là làm thế nào để truy cập các đổi tượng trình điều khiển và ORM.

### Trình xác nhận

Lớp trình xác nhận mang khả năng để thêm và loại bỏ các quy tắc cũng như làm thể nào để chạy chúng.
Lớp này cũng theo dõi các lỗi.

### Form/Trường

Mặc dù các hai form và trường có khả năng mang các trường và form bên trong chính chúng vì chúng là đối tượng trình chứa, mỗi lớp này nắm giữ chức năng cụ thể cho việc xử lý với các form, subform và các trường cụ thể.

### ORM

Đối tượng ORM hoặt động như một trình điều khiển và giao tiếp để kết nối tới một thư viện ORM.

### Trình điều khiển

Trình điều khiển xử lý form và các chức năng trường cụ thể.
Ví dụ, một nhóm các nút chọn được chọn là not_empty khác nhau từ một trường mật khẩu.

### Khung nhìn

Khung nhìn thêm các chức năng mở rộng tới một đối tượng form hoặc trường.
Một ví dụ của một việc trang trí là trang trí HTML mà thêm `attr()` và `add_class()` và cũng kết xuất các form và trường như các đối tượng HTML DOM.

[Tiếp tục tới mục Bắt đầu >](formo.getting-started)
