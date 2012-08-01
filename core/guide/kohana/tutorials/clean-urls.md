# Làm sạch URL

Loại bỏ `index.php` ra khỏi các url của bạn.

Để giữ các URL của bạn sạch sẽ, bạn có thể sẽ muốn có thể truy cập ứng dụng của bạn mà không có `/index.php/` trong URL.
Có hai bước để loại bỏ `index.php` ra khỏi URL.

1. Sửa tập tin khởi động
2. Thiết lập viết lại - rewriting

## 1. Cấu hình khởi động

Điều đầu tiên bạn sẽ cần thay đổi là thiết lập `index_file` của [Kohana::init] tới false:

    Kohana::init(array(
        'base_url'   => '/myapp/',
        'index_file' => FALSE,
    ));

Sự thay đổi này sẽ làm cho tất cả các liên kết được tạo bằng cách sử dụng [URL::site], [URL::base], và [HTML::anchor] sẽ không còn bao gồm "index.php" trong URL.
Tất cả các liên kết được tạo sẽ bắt đầu với `/myapp/` thay vì `/myapp/index.php/`.

## 2. Viết lại URL

Việc kích hoạt viết lại được thực hiện khác nhau, tuỳ thuộc vào máy chủ web của bạn.

Viết lại sẽ làm cho các url sẽ được truyền tới index.php.

## Apache

Đổi tên `example.htaccess` thành `.htaccess` và thay đổi dòng `RewriteBase` phù hợp với thiết lập `base_url` từ [Kohana::init] của bạn

    RewriteBase /myapp/

Phần còn lại của tập tin `.htaccess` viết lại tất cả yêu cầu thông qua index.php, trừ tập tin tồn tại trên máy chủ (do đó css, các hình ảnh, favicon, v.v. của bạn vẫn được tải bình thường).
Trong hầu hết trường hợp, bạn đã làm xong!

### Thất bại!

Nếu bạn nhận một lỗi "Internal Server Error" hoặc "No input file specified", hãy thử thay đổi:

    RewriteRule ^(?:application|modules|system)\b - [F,L]

Thay vào đó, chúng ta có thể thử một dấu gạch chéo:

    RewriteRule ^(application|modules|system)/ - [F,L]

Nếu việc đó không làm việc, hãy thử thay đổi:

    RewriteRule .* index.php/$0 [PT]

Tới vài thứ đơn giản hơn:

    RewriteRule .* index.php [PT]

### Vẫn thất bại!

Nếu bạn vẫn gặp các lỗi, kiểm tra để chắc chắn máy của bạn hỗ trợ URL `mod_rewrite`.
Nếu bạn có thể thay đổi cấu hình Apache, thêm các dòng này tới cấu hình, thường là `httpd.conf`:

    <Directory "/var/www/html/myapp">
        Order allow,deny
        Allow from all
        AllowOverride All
    </Directory>

Bạn cũng nên kiểm tra lưu vết của Apache của bạn để xem nếu chúng có thể đổ ra vài ánh sáng lỗi.

## NGINX

Thật khó để đưa ra các ví dụ về cấu hình nginx, nhưng đây là một mẫu cho một máy chủ:

    location / {
        index     index.php index.html index.htm;
        try_files $uri index.php;
    }

    location = index.php {
        include       fastcgi.conf;
        fastcgi_pass  127.0.0.1:9000;
        fastcgi_index index.php;
    }

Nếu bạn có gặp vấn đề về việc làm này, bật mức gỡ rối của lưu vết trong nginx và kiểm tra các dấu vết truy cập và lỗi.
