# Bắt đầu

Đây là tài liệu cho Jelly, một ORM cho Kohana 3.1+.

[!!] __Xin lưu ý:__ phiên bản này của Jelly làm một rẽ nhánh cộng đồng, và mục tiêu của nó là đảm bảo khả năng tương thích với phiên phản Kohana mới nhất và sửa các lỗi. Nó đã được tạo, bởi vì [mô-đun chính thức](http://github.com/jonathangeiger/kohana-jelly) không được cập nhật gần đây.

Trước hết, nếu bạn đã cảm thấy mất cảm giác tự do để đặt một câu hỏi trong [diễn đàn Kohana](http://forum.kohanaframework.org)-tất cả chúng ta rất tốt đẹp và hữu ích.
Nếu bạn cảm thấy tốt hơn nhìn vào mã nguồn, bạn có thể luôn luôn [xem tài liệu API](../api/Jelly) hoặc [duyệt mã nguồn trên Github](https://github.com/creatoro/jelly).

## Cài đặt

Để cài đặt Jelly đơn giản [tải về phiên bản phát hành mới nhất](https://github.com/creatoro/jelly) và đặt nó trong thư mục mô-đun của bạn.
Sau đó bạn phải chỉnh sửa tập tin `application/bootstrap.php` của bạn và sửa đổi `Kohana::modules` để bao gồm mô-đun Jelly:

	Kohana::modules(array(
	    ...
	    'database' => MODPATH.'database',
		'jelly'    => MODPATH.'jelly',
	    ...
	));
	
Chú ý rằng Jelly phụ thuộc vào [mô-đun database](http://github.com/kohana/database) của Kohana 3.x.
Hãy chắc chắn bạn cài đặt và cấu hình nó tốt.

Nếu bạn đang có kế hoạch sử dụng để bao gồm trình __[điều khiển Auth](https://github.com/creatoro/jelly-auth)__ bạn phải thiết lập salt của cookie bằng cách làm theo [các hướng dẫn này](../kohana/upgrading#cookie-salts).

## Yêu cầu

Jelly yêu cầu phiên bản Kohana sau mỗi nhánh [Git](https://github.com/creatoro/jelly):

 - Các nhánh __3.1/develop và 3.1/master:__ Kohana 3.1.3+
 - Các nhánh __3.2/develop và 3.2/master:__ Kohana 3.2+

## Cách dùng cơ bản

Các thao tác cơ bản cần thiết để làm việc vởi Jelly là:

1.  [Định nghĩa mô hình](getting-started/defining-models)
2.  [Nạp và liệt kê các bản ghi](getting-started/loading-and-listing)
3.  [Tạo, cập nhật và xoá các bản ghi](getting-started/cud)
4.  [Truy cập và quản lý quan hệ](relationships)

## Sử dụng nâng cao hơn

Jelly cực kỳ linh hoạt với hầu hết tất cả các khía cạnh của các hành vi của nó được mở rộng trong suốt.
Các hướng dẫn dưới đây đưa ra một tổng quan của một vài cách dùng nâng cao hơn.

1.  [Mở rộng trình dựng truy vấn](extending-builder)
2.  [Định nghĩa trường tuỳ biến](field-types/custom)

## Tham gia phát triển Jelly

Jelly đã luôn luôn là một dự án cộng đồng, đó là phát triển và trong tương lai phụ thuộc vào những người đang sẵn sàng để đưa một số thời gian vào nó.
Cách dễ nhất để đóng góp là rẽ nhánh dự án trên [GitHub](https://github.com/creatoro/jelly).

__Hãy nhớ rằng:__

* bạn có thể trực tiếp chỉnh sửa các tập tin trên GitHub (xem nút __Edit this file__), không cần phải làm quen với Git nếu bạn không muốn
* hãy làm theo các [quy ước Kohana](../kohana/conventions) cho viết mã
* đọc phần [giới thiệu các kiểm thử đơn vị](unit-tests) và chạy chúng nếu bạn thực hiện thay đổi Jelly để giảm thiểu cơ hội giới thiệu các lỗi mới
* và cảm ơn về việc giúp đỡ Jelly trở lên tốt hơn!
