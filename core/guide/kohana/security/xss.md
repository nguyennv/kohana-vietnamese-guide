# Bảo mật Cross-Site Scripting (XSS)

*Trang này không toàn diện và không nên được coi là một hướng dẫn đầy đủ đến việc phòng chống XSS.*

Đầu tiên để ngăn chặn các cuộc tấn công [XSS](http://wikipedia.org/wiki/Cross-Site_Scripting) là biết khi nào bạn cần phải tự bảo vệ mình.
XSS chỉ có thể được kích hoạt khi nó được hiển thị trong nội dung HTML, đôi khi thông qua một đầu vào mẫu biểu hoặc được hiển thị từ kết quả cơ sở dữ liệu.
Bất kỳ biến toàn cục nào chứa thông tin khách hàng có thể bị nhiễm độc.
Điều này bao gồm dữ liệu `$_GET`, `$_POST`, và `$_COOKIE`.

## Phòng chống

Có một quy tắc đơn giản làm theo để bảo vệ HTML ứng dụng của bạn chống lại XSS.
Nếu bạn không muốn HTML nằm trong một biến, sử dụng hàm [strip_tags](http://php.net/strip_tags) để loại bỏ tất cả các thẻ HTML không mong muốn từ một giá trị.

[!!] Nếu bạn cho phép người dùng gửi HTML tới ứng dụng của bạn, rất khuyến khích sử dụng một công cụ làm sạch HTML như [HTML Purifier](http://htmlpurifier.org/) hoặc [HTML Tidy](http://php.net/tidy).

Thứ hai là để luôn luôn thoát (escape) dữ liệu khi chèn vào HTML.
Lớp HTML cung cấp bộ tạo cho nhiều thẻ thông thường, bao gồm các thẻ script và liên kết stylesheet, các mỏ leo, hình ảnh và liên kết gửi email (mailto).
Bất kỳ nội dung không tin tưởng nào nên được thoát - escape bằng cách sử dụng [HTML::chars].

## Tài liệu tham khảo

* [OWASP XSS Cheat Sheet](http://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet)