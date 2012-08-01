# Gỡ rối

Kohana bao gồm vào cung cụ để trợ giúp bạn gỡ rối ứng dụng của bạn.

Công cụ cơ bản nhất trong số này là [Debug::vars]
Phương thức đơn giản này sẽ hiển thị bất kỳ số nào của các biết, tương tự như [var_export](http://php.net/var_export) hoặc [print_r](http://php.net/print_r), nhưng sử dụng HTML cho việc định dạng mở rộng.

    // Hiển thị một kết xuất của của các biến $foo và $bar
    echo Debug::vars($foo, $bar);

Kohana cũng cung cấp một phương thức để hiển thị mã nguồn của một tập tin cụ thể bằng cách sử dụng [Debug::source].

    // Hiển thị dòng này của mã nguồn
    echo Debug::source(__FILE__, __LINE__);

Nếu bạn muốn hiển thị thông tin về các tập tin ứng dụng của bạn mà không lộ thư mục cài đặt, bạn có thể sử dụng [Debug::path]:

    // Hiển thị "APPPATH/cache" hơn là đường dẫn thực
    echo Debug::path(APPPATH.'cache');

Nếu bạn đang gặp khó khăn gì đó để làm việc đúng, bạn có thể kiểm tra các lưu vết của Kohana và lưu vết máy chủ web của bạn, cũng như sử dụng một công cụ gỡ rối giống như [Xdebug](http://www.xdebug.org/).
