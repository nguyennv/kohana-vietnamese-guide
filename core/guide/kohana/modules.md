# Mô-đun

Các mô-đun chỉ đơn giản là một bổ xung tới [Hệ thống tập tin phân cấp](files).
Một mô-đun có thể thêm bất kỳ loại tập tin (điều khiển, khung nhìn, các lớp, tập tin cấu hình, v.v.) tới hệ thống tập tin có sẵn tới Kohana (thông qua [Kohana::find_file]).
Điều này rất hữu ích để làm bất kỳ phần nào của ứng dụng của bạn uyển chuyển hơn và có thể chia sẻ giữa các ứng dụng khác nhau.
Ví dụ, tạo một hệ thống mô hình mới, một máy tìm kiếm, một trình quản lý css/js v.v.

## Nơi để tìm các mô-đun

Kolanos đã tạo ra [kohana-universe](http://github.com/kolanos/kohana-universe/tree/master/modules/), một danh sách khá toàn diện các mô-đun mà có sẵn trên Github.
Để mô-đun của bạn được liệt kê ở đây, gửi cho anh ta một thông điệp từ Github.

Mon Geslani đã tạo ra [một trang rất đẹp](http://kohana.mongeslani.com/) mà cho phép bạn sắp xếp các mô-đun Github theo hoạt động, người theo dõi, rẽ nhánh, v.v.
Nó có vẻ không được toàn diện như kohana-universe.

Andrew Hutchings đã tạo [kohana-modules](http://www.kohana-modules.com) mà tương tự như các trang ở trên.

## Bật các mô-đun

Các mô-đun được bật bằng cách gọi [Kohana::modules] và truyền một mảng `'tên' => 'đường dẫn'`.
Tên thì không quan trọng nhưng đường dẫn thì rõ ràng là quan trọng.
Một đường dẫn của mô-đun không cần phải ở trong `MODPATH`, nhưng thường là như vậy.
Bạn chỉ có thể gọi [Kohana::modules] một lần.

	Kohana::modules(array(
		'auth'       => MODPATH.'auth',       // Basic authentication
		'cache'      => MODPATH.'cache',      // Caching with multiple backends
		'codebench'  => MODPATH.'codebench',  // Benchmarking tool
		'database'   => MODPATH.'database',   // Database access
		'image'      => MODPATH.'image',      // Image manipulation
		'orm'        => MODPATH.'orm',        // Object Relationship Mapping
		'oauth'      => MODPATH.'oauth',      // OAuth authentication
		'pagination' => MODPATH.'pagination', // Paging of results
		'unittest'   => MODPATH.'unittest',   // Unit testing
		'userguide'  => MODPATH.'userguide',  // User guide and API documentation
		));

## Init.php

Khi một mô-đun được bật, nếu một tập tin `init.php` tồn tài trong thư mục mô-đun đó, nó được bao gồm.
Đây là nơi lý tưởng để có một mô-đun bao gồm các định tuyến và các khởi tạo khác cần thiết cho module hoạt động.
Các mô-đun Userguide và Codebench có các tập tin init.php mà bạn có thể xem.

## Mô-đun làm việc như thế nào

Một tập tin trong một mô-đun đã bật hầu như giống như có tập tin chính xác đó trong cùng nơi trong thư mục ứng dụng.
Sự khác biệt chính là nó có thể được ghi đè bởi một tập tin cùng tên ở vị trí cao hơn (một mô-đun đã kích hoạt sau nó, hoặc thư mục ứng dụng) từ [Hệ thống tập tin phân cấp](files).
Nó cũng cung cấp cách dễ dàng để tổ chức và chia sẻ mã của bạn.

## Tạo mô-đun của riêng bạn

Để tạo một mô-đun đơn giản là tạo một thư mục (thường là trong `DOCROOT/modules`) và đặt các tập tin bạn muốn ở trong mô-đun đó, và bật mô-đun đó trong tập tin khởi động của bạn.
Để chia sẻ mô-đun của bạn, bạn có thể tải nó lên [Github](http://github.com).
Bạn có thể xem các ví dụ về mô-đun được làm bởi [Kohana](http://github.com/kohana) hoặc [người dùng khác](#where-to-find-modules).
