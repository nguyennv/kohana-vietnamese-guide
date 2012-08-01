# Khung nhìn

Các khung nhìn là các tập tin mà chứa thông tin hiển thị cho ứng dụng của bạn.
Ở đây phổ biết nhất là HTML, CSS và Javascript nhưng có thể là bất cứ gì bạn yêu cầu như XML hoặc JSON cho đầu ra AJAX.
Mục đích của khung nhìn là giữ thông tin này tách biệt với logic ứng dụng của bạn để sử mã sạch và sử dụng lại dễ dàng.

Bản thân các khung nhìn có thể chữa mã được sử dụng cho việc hiển thị dữ liệu bạn truyền vào trong chúng.
Ví dụ, vòng lặp thông qua một mảng thông tin sản phẩm và hiển thị mỗi sản phẩm trên một hàng mới của bảng.
Các khung nhìn vẫn là các tập tin PHP vì thế bạn có thể sử dụng bất kỳ mã nào bình thường.
Tuy nhiên, bạn nên cố gắng giữ khung nhìn của bạn "lầm lì-dumb" khi có thể và nhận tất cả dữ liệu bạn cần trong các điều khiển của bạn, sau đó truyền nó tới khung nhìn.

# Tạo các tập tin khung nhìn

Các tập tin khung nhìn được lưu trong thư mục `views` của [hệ thống tập tin](files).
Bạn cũng có thể tạo các thư mục con bên trong thư mục `views` để tổ chức các tập tin của bạn.
Tất cả các ví dụ đây là các tập tin khung nhìn hợp lý:

    APPPATH/views/home.php
    APPPATH/views/pages/about.php
    APPPATH/views/products/details.php
    MODPATH/error/views/errors/404.php
    MODPATH/common/views/template.php

## Nạp khung nhìn

Các đối tượng [View] sẽ thông thường được tạo bên trong một [Điều khiển](mvc/controllers) bằng cách sử dụng phương thức [View::factory].
Thông thường khung nhìn sau đó được gán như thuộc tính [Request::$response] hoặc tới khung nhìn khác.

    public function action_about()
    {
        $this->response->body(View::factory('pages/about'));
    }

Khi một khung nhìn được gán như [Response::body], như ví dụ ở trên, nó sẽ tự động được kết xuất khi cần thiết.
Để nhận kết quả kết xuất của một khung nhìn bạn có thể gọi phương thức [View::render] hoặc ép kiểu nó thành một chuỗi.
Khi một khung nhìn được kết xuất, tập tin khung nhìn được nạp và HTML được sinh ra.

    public function action_index()
    {
        $view = View::factory('pages/about');

        // Kết xuất view
        $about_page = $view->render();

        // hoặc chỉ ép kiểu nó như một chuỗi
        $about_page = (string) $view;

        $this->response->body($about_page);
    }

## Các biến trong các khung nhìn

Một khi khung nhìn được nạp, các biến có thể được gán tới nó bằng cách sử dụng các phương thức [View::set] và [View::bind].

    public function action_roadtrip()
    {
        $view = View::factory('user/roadtrip')
            ->set('places', array('Rome', 'Paris', 'London', 'New York', 'Tokyo'));
            ->bind('user', $this->user);

        // Khung nhìn sẽ có các biến $places và $user
        $this->response->body($view);
    }

[!!] Sự khác biệt duy nhất giữa `set()` và `bind()` là `bind()` gán các biến theo tham chiếu. Nếu bạn `bind()` một biến trước khi nó đã được định nghĩa, biến sẽ được tạo với một giá trị NULL. 

Bạn cũng có thể gán các biến trực tiếp tới đối tượng View. Điều này giống hệt lời gọi `set()`;

	public function action_roadtrip()
	{
		$view = View::factory('user/roadtrip');
            
		$view->places = array('Rome', 'Paris', 'London', 'New York', 'Tokyo');
        $view->user = $this->user;

        // Khung nhìn sẽ có các biến $places và $user
        $this->response->body($view);
	}

### Các biến toàn cục

Một ứng dụng có thể có vài tập tin khung nhìn mà cần truy cập cùng các biến.
Ví dụ, để hiển thị tiêu để trang ở trong cả đầu đề và thân của nội dung trang.
Bạn có thể tạo các biến mà có thể truy cập trong bất kỳ khung nhìn nào bằng cách sử dụng các phương thức [View::set_global] và [View::bind_global].

    // Gán $page_title tới tất cả các khung nhìn
    View::bind_global('page_title', $page_title);

Nếu ứng dụng có ba khung nhìn mà được kết xuất cho trang chủ: `template`, `template/sidebar`, và `pages/home`.
Trước tiên, một điều khiển trừu tượng tạo khuôn mẫu sẽ được tạo:

    abstract class Controller_Website extends Controller_Template {

        public $page_title;

        public function before()
        {
            parent::before();

            // Làm $page_title có sẵn tới tất cả khung nhìn
            View::bind_global('page_title', $this->page_title);

            // Nạp $sidebar vào trong một khuôn mẫu như một khung nhìn
            $this->template->sidebar = View::factory('template/sidebar');
        }

    }

Kêt tiếp, điều khiển home sẽ mở rộng `Controller_Website:`

    class Controller_Home extends Controller_Website {

        public function action_index()
        {
            $this->page_title = 'Home';

            $this->template->content = View::factory('pages/home');
        }

    }

## Khung nhìn bên trong khung nhìn

Nếu bạn muốn bao gồm khung nhìn khác bên trong một khung nhìn, có hai lựa chọn.
Bằng cách gọi [View::factory] bạn có thể đóng khung - sandbox khung nhìn được bao gồm.
Điều này có nghĩa rằng bạn sẽ phải cung cấp tất cả các biến tới khung nhìn bằng cách sử dụng [View::set] hoặc [View::bind]:
	
	// Trong tập tin khung nhìn của bạn:
	
    // Chỉ biến $user variable sẽ có sẵn trong "views/user/login.php"
    <?php echo View::factory('user/login')->bind('user', $user) ?>

Tuỳ chọn khác để bao gồm khung nhìn trực tiếp, mà làm cho tất cả các biến có sẵn cho khung nhìn được bao gồm:

	// Trong tập tin khung nhìn của bạn:
	
    // Bất kỳ biến nào đã định nghĩa trong khung nhìn sẽ được bao gồm trong "views/message.php"
    <?php include Kohana::find_file('views', 'user/login') ?>

Bạn cũng có thể gán một biến của khung nhìn cha của bạn là khung nhìn con từ bên trong điều khiển của bạn. Ví dụ:

	// Trong điều khiển của bạn:

	public functin action_index()
	{
		$view = View::factory('common/template);
		
		$view->title = "Some title";
		$view->body = View::factory('pages/foobar');
	}
	
	// Trong views/common/template.php:
	
	<html>
	<head>
		<title><?php echo $title></title>
	</head>
	
	<body>
		<?php echo $body ?>
	</body>
	</html>

Tất nhiên, bạn cũng có thể nạp toàn bộ một [Request] bên trong một khung nhìn:

    <?php echo Request::factory('user/login')->execute() ?>

Đây là một ví dụ của \[HMVC], mà làm cho nó có thể tạo và đọc các lời gọi tới các URL khác bên trong ứng dụng của bạn.
