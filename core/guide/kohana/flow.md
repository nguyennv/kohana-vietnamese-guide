# Luồng yêu cầu

Mọi ứng dụng sẽ cùng một luồng sau:

1. Ứng dụng bắt đầu từ tập tin `index.php`. 
	1. Đường dẫn ứng dụng, mô-đun, và hệ thống được thiết lập. (`APPPATH`, `MODPATH`, và `SYSPATH`)
	2. Các mức thông báo lỗi được thiết lập.
	3. Tập tin cài đặt được nạp, nếu nó tồn tại.
	4. Tập tin khởi động, `APPPATH/bootstrap.php`, được bao gồm vào.
2. Sau khi chúng ta ở trong `bootstrap.php`:
	6. Lớp [Kohana] được nạp.
	7. [Kohana::init] được gọi, cái mà thiết lập việc quản lý lỗi, bộ nhớ đệm, và lưu vết.
	8. Trình đọc [Kohana_Config] và trình ghi [Kohana_Log] được đính kèm.
	9. [Kohana::modules] được gọi để bật các mô-đun thêm. 
	    * Đường dẫn mô-đun được thêm vào [hệ thống tập tin phân cấp](files).
		* Bao gồm tập tin `init.php` của mỗi mô-đun, nếu nó tồn tại.
	    * Tập tin `init.php` có thể thực hiện thiết lập môi trường thêm, bao gồm thêm các định tuyến.
	10. Hàm [Route::set] được gọi nhiều lần để định nghĩa các [định tuyến của ứng dụng](routing).
	11. Hàm [Request::instance] được gọi để bắt đầu sử lý yêu cầu. 
		1. Kiểm tra mỗi định tuyến đã được thiết lập cho đến khi một so khớp được tìm thấy.
		2. Tạo thể hiện của điều khiển và truyền đối tượng yêu cầu tới nó.
		3. Gọi phương thức [Controller::before].
		4. Gọi phương thức hành động của điều khiển, cái mà sinh ra phản hồi của yêu cầu.
		5. Gọi phương thức [Controller::after]. 
		    * 5 bước trên có thể được lặp lại nhiều lần khi sử dụng [các yêu cầu con HMVC](requests).
3. Luồng ứng dụng trả về tới index.php
	12. Trình phản hồi chính của [Request] được hiển thị
