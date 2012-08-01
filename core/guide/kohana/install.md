# Cài đặt

1. Tải về bản phát hành **ổn định** mới nhất từ [website Kohana](http://kohanaframework.org/).
2. Giải nén gói đã tải về để tạo một thư mục `kohana`.
3. Tải lên nội dung của thư mục này lên máy chủ web của bạn.
4. Mở tập tin `application/bootstrap.php` và thực hiện những thay đổi sau: 
	- Đặt [múi giờ](http://php.net/timezones) mặc định cho ứng dụng của bạn.
	- Thiết lập `base_url` trong lời gọi hàm [Kohana::init] để ánh xạ vị trí của thư mục kohana trên máy chủ của bạn liên quan đến gốc tài liệu.
6. Đảm bảo rằng các thư mục `application/cache` và `application/logs` có thể ghi được bởi máy chủ web.
7. Kiểm tra cài đặt của bạn bằng việc mở URL mà bạn thiết lập ở `base_url` trong trình duyệt ưa thích của bạn.

[!!] Tùy thuộc vào nền tảng của bạn, các thư mục con của cài đặt có thể mất quyền nhờ vào việc giải nén. Chmod tất cả tới 755 bằng việc chạy lệnh `find . -type d -exec chmod 0755 {} \;` từ thư mục gốc của cài đặt Kohana của bạn.

Bạn sẽ thấy trang cài đặt. Nếu nó báo cáo ra bất kỳ lỗi nào, bạn sẽ cần sửa chữa chúng trước khi tiếp tục.

![Install Page](install.png "Ví dụ trang cài đặt")

Sau khi trang cài đặt của bạn báo cáo rằng môi trường của bạn đã thiết lập đúng bạn cần hoặc đổi tên hoặc xoá tập tin `install.php` trong thư mục gốc của Kohana. Kohana giờ đã được cài đặt và bạn sẽ thấy màn của điều khiển welcome:

![Welcome Page](welcome.png "Ví dụ về trang chào mừng")

## Cài đặt Kohana 3.2 từ GitHub

[Mã nguồn](http://github.com/kohana/kohana) cho Kohana 3.2 được đặt tại [GitHub](http://github.com). Để cài đặt Kohana sử dụng mã nguồn trên github trước tiên bạn cần cài đặt git. Ghé thăm [http://help.github.com](http://help.github.com) để biết chi tiết về cách cài đặt git trên nền tảng của bạn.

[!!] Để biết thêm thông tin về cách cài đặt Kohana sử dụng mô-đun con git, xem hướng dẫn [Làm việc với Git](tutorials/git).
