# Thêm mô-đun của bạn tới userguide

Việc thực hiện mô-đun của bạn làm việc với userguide là đơn giản.

Đầu tiên, sao chép cấu hình này vào đặt trong nó `<module>/config/userguide.php`, và thay thế bất kỳ thứ gì trong `<>` với những thứ thích hợp:

	return array
	(
		// Để một mình
		'modules' => array(
	
			//  Điều này cần đwọc dẫn đường tới các trang userguide của các module này, không có 'guide/'. Vd: '/guide/modulename/' sẽ là 'modulename'
			'<modulename>' => array(
	
				// Xác định các trang userguide của các module này sẽ được hiển thị
				'enabled' => TRUE,
				
				// Tên mà sẽ xuất hiện trên trang index của userguide
				'name' => '<Tên mô-đun>',
	
				// Một mô tả ngắn của module này, hiển thị trên trang index
				'description' => '<Mô tả tại đây.>',
				
				// Thông báo bản quyền, hiển thị trong footer cho module này
				'copyright' => '&copy; 2010–2012 <Tên của bạn>',
			)	
		)
	);

Tiếp theo, tạo một thư mục trong thư mục module của bạn được gọi là `guide/<modulename>` và tạo các tập tin `index.md` và `menu.md`.
Tất cả các trang userguide sử dụng [Markdown](markdown).
Trang index là những gì được hiển thị trong index của mô-đun của bạn, trình đơn là cái mà hiển thị trong cột bên cạnh.
Trình đơn nên được định dạng như thế này:

	## [Tên mô-đun]()
	 - [Tên trang](page-path)
	 - [Đây là một Danh mục](category)
		 - [Trang con](category/sub-page)
		 - [Trang khác](category/another)
			 - [Trang con khác](category/another/sub-page)
	 - Các danh mục không có một liên kết tới một trang
		 - [Vân vân](etc)

Các đường dẫn trang có liên quan tới `guide/<modulename>`.
Vì vậy, `[Tên trang](page-path)` sẽ trông giống `guide/<modulename>/page-name.md` và `[Trang khác](category/another)` sẽ trông giống như `guide/<modulename>/page-name.md`.
Các trang hướng dẫn có thể được đặt tên hoặc sắp xếp lại bất kỳ cách nào mà bạn muốn trong thư mục đó (với ngoại lệ của `menu.md` và `index.md`).
Breadcrumb và các tiêu để trang được kéo từ tập tin `menu.md`, không phải tên tập tin hoặc đường dẫn.
Bạn có thể có các mục mà không phải là các trang(một danh mục mà không có trang tương ứng).
Để liên kết tới trang `index.md`, bạn nên có một liên kết trống, v.d. `[Tên module]()`.
Không bao gồm `.md` trong liên kết của bạn.
