# Thực hiện các truy vấn

Có hai cách khác nhau để thực hiện các truy vấn.
Cách đơn giản nhất để thực hiện một truy vấn là sử dụng [Database_Query], thông qua [DB::query], để tạo các truy vấn bằng tay.
Các truy vấn này được gọi là [các câu lệnh đã chuẩn bị](query/prepared) và cho phép bạn thiết lập các tham số truy vấn cái mà tự động được thoát.
Cách thứ hai để tạo một truy vấn là bằng cách xây truy vấn sử dụng các lời gọi phương thức.
Cách này được thực hiện bằng cách sử dụng [trình dựng truy vấn](query/builder).

[!!] Tất cả truy vấn được chạy bằng cách sử dụng phương thức `execute`, cái mà chấp nhận một đối tượng [Database] hoặc một tên thể hiện. Xem [Database_Query::execute] để biết thêm thông tin.
