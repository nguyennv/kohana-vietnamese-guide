# Cấu hình bộ nhớ đệm Kohana

Bộ nhớ đệm Kohana sử dụng các nhóm cấu hình để tạo các thể hiện bộ nhớ đệm.
Một nhóm cấu hình có thể sử dụng bất kỳ trình điều khiển nào được hỗ trợ, với các nhóm tiếp theo bằng các sử dụng nhiều thể hiện cho cùng kiểu trình điều khiển.

Nhóm bộ nhớ đệm mặc định được nạp dựa trên thiết lập `Cache::$default`.
Nếu nó được thiết lập tới trình điều khiển `file` như là chuẩn, tuy nhiển điều này có thể được thay đổi bên trong tập tin `/application/boostrap.php`.

     // Thay đổi trình điều khiển bộ nhớ đệm mặc định tới memcache
     Cache::$default = 'memcache';

     // Nạp trình điều khiển bộ nhớ đệm memcache bằng cách sử dụng thiết lập mặc định
     $memcache = Cache::instance();

## Thiết lập nhóm

Dưới đây là các nhóm cấu hình bộ nhớ đệm mặc định cho mỗi trình điều khiển được hỗ trợ.
Thêm vào - hoặc ghi đè các thiết lập này bên trong tập tin `application/config/cache.php`.

Tên            | Yêu cầu  | Mô tả
-------------- | -------- | ---------------------------------------------------------------
driver         | __YES__  | (_string_) Loại trình điều khiển để sử dụng
default_expire | __NO__   | (_string_) Hết hạn mặc định


	'file'  => array
	(
		'driver'             => 'file',
		'cache_dir'          => APPPATH.'cache/.kohana_cache',
		'default_expire'     => 3600,
	),

## Thiết lập Memcache & Memcached-tag

Tên            | Yêu cầu  | Mô tả
-------------- | -------- | ---------------------------------------------------------------
driver         | __YES__  | (_string_)  Loại trình điều khiển để sử dụng
servers        | __YES__  | (_array_) Mảng liên kết các chi tiết máy chủ, phải bao gồm một khoá __host__. (Xem __Cấu hình máy chủ Memcache__ bên dưới)
compression    | __NO__   | (_boolean_) Sử dụng nén dữ liệu khi nhớ đệm

### Thiết lập máy chủ Memcache

Tên              | Yêu cầu  | Mô tả
---------------- | -------- | ---------------------------------------------------------------
host             | __YES__  | (_string_) Tên máy của máy chủ memcache, v.d. __localhost__; hoặc __127.0.0.1__; hoặc __memcache.domain.tld__
port             | __NO__   | (_integer_) Trỏ tới cổng nơi mà memcached lắng nghe các kết nối. Thiết lập tham số này tới 0 khi đang sử dụng ổ cắm UNIX. Mặc định __11211__
persistent       | __NO__   | (_boolean_) Điều khiển việc sử dụng một kết nối liên tục. Mặc định __TRUE__
weight           | __NO__   | (_integer_) Số nhóm để tạo cho máy chủ này cái mà xác xuất đến lượt điều khiển của nó đang được chọn. Xác xuất là tương đối tới tổng trọng lượng của tất cả máy chủ. Mặc định __1__
timeout          | __NO__   | (_integer_) Giá trị tính bằng giây mà sẽ được sử dụng để kết nối tới trình nền - daemon. Hãy suy nghĩa hai lần trước khi thay đổi giá trị mặc định tới 1 giây - bạn có thể mất tất cả các lợi thế của bộ nhớ đệm nếu kết nối của bạn quá chậm. Mặc định __1__
retry_interval   | __NO__   | (_integer_) Điều khiểnt mức độ thường xuyên một máy chủ thất bại sẽ được thử lại, giá trị mặc định là 15 giây. Thiết lập tham số này tới -1 để vô hiệu hoá thử lại tự động. Mặc định __15__
status           | __NO__   | (_boolean_) Điều khiển nếu máy chủ sẽ được gắn cờ là trực tuyến. Mặc định __TRUE__
failure_callback | __NO__   | (_[callback](http://www.php.net/manual/en/language.pseudo-types.php#language.types.callback)_) Cho phép người dụng chỉ định một hàm callback để chạy khi gặp phải một lỗi. Callback được chạy trước khi chuyển đổi dự phòng cố gắng thử. hàm này lấy hai tham số, tên máy và cổng của máy chủ thât bại. Mặc định __NULL__

	'memcache' => array
	(
		'driver'             => 'memcache',
		'default_expire'     => 3600,
		'compression'        => FALSE,              // Sử dụng nén Zlib
		                                            // (có thể gây ra các vấn đề với số nguyên)
		'servers'            => array
		(
			array
			(
				'host'             => 'localhost',  // Máy chủ Memcache
				'port'             => 11211,        // Số của cổng Memcache
				'persistent'       => FALSE,        // Kết nối liên tục
			),
		),
	),
	'memcachetag' => array
	(
		'driver'             => 'memcachetag',
		'default_expire'     => 3600,
		'compression'        => FALSE,              // Sử dụng nén Zlib
		                                            // (có thể gây ra các vấn đề với số nguyên)
		'servers'            => array
		(
			array
			(
				'host'             => 'localhost',  // Máy chủ Memcache
				'port'             => 11211,        // Số của cổng Memcache
				'persistent'       => FALSE,        // Kết nối liên tục
			),
		),
	),

## Thiết lập APC

	'apc'      => array
	(
		'driver'             => 'apc',
		'default_expire'     => 3600,
	),

## Thiết lập SQLite

	'sqlite'   => array
	(
		'driver'             => 'sqlite',
		'default_expire'     => 3600,
		'database'           => APPPATH.'cache/kohana-cache.sql3',
		'schema'             => 'CREATE TABLE caches(id VARCHAR(127) PRIMARY KEY, 
		                                  tags VARCHAR(255), expiration INTEGER, cache TEXT)',
	),

## Thiết lập File

	'file'    => array
	(
		'driver'             => 'file',
		'cache_dir'          => 'cache/.kohana_cache',
		'default_expire'     => 3600,
	)

## Thiết lập Wincache

	'wincache' => array
	(
		'driver'             => 'wincache',
		'default_expire'     => 3600,
	),


## Ghi đè nhóm cấu hình hiện thời

Ví dụ sau đây cho thấy cách ghi đè một thiết lập cấu hình có sẵn, bằng cách sử dụng tập tin cấu hình trong `/application/config/cache.php`.

	<?php defined('SYSPATH') or die('No direct script access.');
	return array
	(
		// Override the default configuration
		'memcache'   => array
		(
			'driver'         => 'memcache',  // Sử dụng Memcached như trình điều khiển mặc định
			'default_expire' => 8000,        // Ghi đè hết hạn mặc định
			'servers'        => array
			(
				// Thêm một máy chủ mới
				array
				(
					'host'       => 'cache.domain.tld',
					'port'       => 11211,
					'persistent' => FALSE
				)
			),
			'compression'    => FALSE
		)
	);

## Thêm nhóm cấu hình mới

Ví dụ sau đây cho thấy cách thêm một thiết lập cấu hình mới, bằng cách sử dụng tập tin cấu hình `/application/config/cache.php`.

	<?php defined('SYSPATH') or die('No direct script access.');
	return array
	(
		// Override the default configuration
		'fastkv'   => array
		(
			'driver'         => 'apc',  // Sử dụng APC như trình điều khiển mặc định
			'default_expire' => 1000,   // Ghi đè hết hạn mặc định
		)
	);