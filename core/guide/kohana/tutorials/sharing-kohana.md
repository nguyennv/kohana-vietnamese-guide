# Chia sẻ Kohana

Bởi vì Kohana theo một mẫu [điều khiển trước], có nghĩa là tất cả yêu cầu được gửi tới `index.php`, hệ thống tập tin dễ có thể cấu hình.
Bên trong `index.php` bạn có thể thay đổi các đường dẫn `$application`, `$modules`, và `$system`.

[!!] Có một kiểm tra bảo mật ở đầu mỗi tập tin Kohana để ngăn ngừa nó khỏi bị truy cập mà không cần sử dụng điều khiển trước. Ngoài ra, tập tin `.htaccess` cũng nên bảo vệ các thư mục đó cũng tốt hơn. Việc di chuyển các thư mục ứng dụng, mô-đun, và hệ thống tới vị trí mà không thể được truy cập từ web có thể thêm lớp bảo mật khác, nhưng là tuỳ chọn.

Biến `$application` cho phép bạn thiết lập thư mục mà chứa các tập tin ứng dụng của bạn.
Theo mặc định, biến này là `application`.
Biến `$modules` cho phép bạn thiết lập thư mục mà chứa các tập tin mô-đun.
Biến `$system` cho phép bạn thiết đặt thư mục mà chứa các tập tin Kohana mặc định.
Bạn có thể di chuyển ba thư mục này ở bất cứ đâu.

Ví dụ, theo mặc định những thư mục được thiết lập như thế này:

    www/
        index.php
        application/
        modules/
        system/

Bạn có thể di chuyển các thư mục ra khỏi thư mục gốc web để chúng trông như thế này:

    application/
    modules/
    system/
    www/
        index.php

Sau đó, bạn sẽ cần phải thay đổi các thiết lập trong `index.php` là:

    $application = '../application';
    $modules     = '../modules';
    $system      = '../system';

## Chia sẻ hệ thống và các mô-đun

Để thực hiện việc này một bước xa hơn, chúng ta có thể điểm một số ứng dụng Kohana có cùng thư mục hệ thống và mô-đun.
Ví dụ (và việc này chỉ là một ví dụ, bạn có thể xắp xếp chúng nếu bạn muốn):

	apps/
		foobar/
			application/
			www/
		bazbar/
			application/
			www/
	kohana/
		3.0.6/
		3.0.7/
		3.0.8/
	modules/

Và bạn sẽ cần thay đổi các thiết lập này trong `index.php` là:

	$application = '../application';
	$system      = '../../../kohana/3.0.6';
	$modules     = '../../../kohana/modules';

Việc sử dụng phương thức này mỗi ứng dụng có thể trỏ đên một bản sao trung tập của Kohana, và bạn có thể thêm một phiên bản mới, và nhanh chóng cập nhật ứng dụng của bạn tới phiên bảng mới bằng việc sửa các tập tin `index.php` của chúng.
