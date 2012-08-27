# Cơ sở dữ liệu 

Kohana 3.2 đi kèm với một mô-đun mạnh mẽ để làm việc với cơ sở dữ liệu.
Theo mặc định, mô-đun cơ sở dữ liệu hỗ trợ các trình điều khiển cho [MySQL](http://php.net/mysql) và [PDO](http://php.net/pdo), nhưng các trình điều khiển mới có thể được tạo cho các máy chủ cơ sở dũ liệu khác.

Mô-đun cơ sở dữ liệu được bao gồm với cài đặt Kohana 3.2 , nhưng cần phải được kích hoạt trước khi bạn sử dụng nó.
Để kích hoạt, hãy mở tập tin `application/bootstrap.php` và sửa đổi lời gọi đến [Kohana::modules] bằng cách bao gồm mô-đun cơ sở dữ liệu như sau:

    Kohana::modules(array(
        ...
        'database' => MODPATH.'database',
        ...
    ));

Tiếp theo, bạn cần [cấu hình](config) mô-đun cơ sở dữ liệu để kết nối tới cơ sở dữ liệu của bạn.

Khi mà đã thực hiện xong sau đó bạn có thể tạo các [truy vấn](query) và sử dụng [các kết quả](results).

Mô-đun cơ sở dữ liệu cũng cung cấp một a [trình điều khiển cấu hình](../api/Kohana_Config_Database) (cho việc lưu trữ [cấu hình](../kohana/files/config) trong cơ sở dữ liệu) và một [trình điều khiển phiên làm vệc](Session_Database).
