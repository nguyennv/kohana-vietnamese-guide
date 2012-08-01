# Các tập tin cấu hình

Các tập tin cấu hình được sử dụng để lưu trữ bất kỳ loại cấu hình cần thiết cho một mô-đun, lớp, hoặc bất kỳ thứ gì khác mà bạn muốn.
Chúng là các tập tin thuần PHP, được lưu trong thư mục `config/`, mà trả về một mảng liên kết:

    <?php defined('SYSPATH') or die('No direct script access.');

    return array(
        'setting' => 'value',
        'options' => array(
            'foo' => 'bar',
        ),
    );

Nếu tập tin cấu hình ở trên được gọi `myconf.php`, bạn có thể truy cập nó bằng cách sử dụng:

    $config = Kohana::$config->load('myconf');
    $options = $config->get('options')

## Hợp nhất

Các tập tin cấu hình hơi khác so với hầu hết các tập tin khác bên trong [hệ thống tập tin phân cấp](files) ở chỗ chúng được **hợp nhất** hơn là được quá tải.
Điều này có nghĩa rằng tất cả tập tin cấu hình với cùng đường dẫn tập tin được kết hợp để tạo ra cấu hình cuối cùng.
Kết quả cuối cùng là bạn có thể quá tải các thiết lập **riêng lẻ** hơn là nhân bản toàn bộ một tập tin.

Ví dụ, nếu chúng ta muốn thay đổi hoặc thêm một mục trong tập tin cấu hình inflector, chúng ta sẽ không cần nhân bản tất cả các mục khác từ tập tin cấu hình mặc định.

    // config/inflector.php

    <?php defined('SYSPATH') or die('No direct script access.');

    return array(
        'irregular' => array(
            'die' => 'dice', // không tồn tại trong tập tin cấu hình mặc định
            'mouse' => 'mouses', // ghi đè 'mouse' => 'mice' trong tập tin cấu hình mặc định
    );


## Tạo các tập tin cấu hình của riêng bạn

Hãy nói rằng chúng ta có một tập tin cấu hình được lưu trữ và thay đổi dễ dàng các thứ như tiêu đề của một trang web, hoặc mã phân tích google.
Chúng ta sẽ tạo một tập tin cấu hình, chúng ta hay gọi nó là `site.php`:

    // config/site.php

    <?php defined('SYSPATH') or die('No direct script access.');

    return array(
        'title' => 'Our Shiny Website',
        'analytics' => FALSE, // mã phân tích đặt ở đây, thiết lập tới FALSE để vô hiệu
    );

Chúng ta bây giờ có thể gọi `Kohana::config('site.title')` để nhận tên trang, và `Kohana::config('site.analytics')` để nhận mã phân tích.

Hãy nói rằng chúng ta muốn một lưu trữ của các phiên bản của cùng phần mềm.
Chúng ta có thể sử dụng các tập tin cấu hình để lưu trữ mỗi phiên bản, và bao gồm các liên kết để tải về, tài liệu và theo dõi các vấn đề.

	// config/versions.php

	<?php defined('SYSPATH') or die('No direct script access.');
	
    return array(
		'1.0.0' => array(
			'codename' => 'Frog',
			'download' => 'files/ourapp-1.0.0.tar.gz',
			'documentation' => 'docs/1.0.0',
			'released' => '06/05/2009',
			'issues' => 'link/to/bug/tracker',
		),
		'1.1.0' => array(
			'codename' => 'Lizard',
			'download' => 'files/ourapp-1.1.0.tar.gz',
			'documentation' => 'docs/1.1.0',
			'released' => '10/15/2009',
			'issues' => 'link/to/bug/tracker',
		),
		/// ... etc ...
	);

Bạn sau đó có thể làm như sau:

	// Trong điều khiển của bạn
	$view->versions = Kohana::$config->load('versions');
	
	// Trong khung nhìn của bạn
	foreach ($versions as $version)
	{
		// echo html để hiển thị mỗi phiên bản
	}
