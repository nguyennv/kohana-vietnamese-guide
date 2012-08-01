# Điều khiển

Một Điều khiển là một tập tin lớp mà đứng giữa các mô hình và khung nhìn trong một ứng dụng.
Nó truyền thông tin tới mô hình khi dữ liệu cần được thay đổi và nó yêu cầu thông tin từ mô hình khi dữ liệu cần được nạp.
Các điều khiển sau đó truyền thông tin của mô hình tới khung nhìn nới mà đầu ra cuối cùng có thể được đưa ra cho người dùng.
Các điều khiển cơ bản điều khiển luồng của ứng dụng.

Các điều khiển được gọi bởi hàm [Request::execute()] dựa trên [Route] mà url khớp.
Hãy chắc chắn đọc trang [định tuyến](routing) để hiểu cách sử dụng các định tuyến ánh xạ các url tới các điều khiển của bạn.

## Tạo các điều khiển

Theo thứ tự để hoạt động, một điều khiển phải làm như sau:

* Lưu trú trong thư mục `classes/controller` (hoặc một thư mục con của nó)
* Tên tập tin phải là chữ thường, v.d. `articles.php`
* Tên lớp phải được ánh xạ tới tên tập tin (với `/` được thay thế bằng `_`) và mỗi từ có chữ in hoa viết đầu
* Phải có lớp Controller như là một lớp cha(ông)

Một vài ví dụ của tên điều khiển và vị trí tập tin:

	// classes/controller/foobar.php
	class Controller_Foobar extends Controller {
	
	// classes/controller/admin.php
	class Controller_Admin extends Controller {

Các điều khiển có thể ở trong thư mục con:

	// classes/controller/baz/bar.php
	class Controller_Baz_Bar extends Controller {
	
	// classes/controller/product/category.php
	class Controller_Product_Category extends Controller {
	
[!!] Lưu ý rằng các điều khiển trong thư mục con không thể được gọi bởi định tuyến mặc định, bạn sẽ cần định nghĩa một định tuyến mà có một tham số [directory](routing#directory) hoặc thiết lập một giá trị mặc định cho directory.

Các điều khiển có thể mở rộng điều khiển khác.

	// classes/controller/users.php
	class Controller_Users extends Controller_Template
	
	// classes/controller/api.php
	class Controller_Api extends Controller_REST
	
[!!] Controller_Template là một điều khiển ví dụ được cung cấp trong Kohana.

Bạn cũng có thể có một điều khiển mở rộng điều khiển khác để chia sẻ những thứ phổ biến, như yêu cầu bạn phải đăng nhập để sử dụng tất cả điều khiển đó.

	// classes/controller/admin.php
	class Controller_Admin extends Controller {
		// Điều khiển này sẽ có một hàm before() mà kiểm tra nếu một người dùng đã đăng nhập
	
	// classes/controller/admin/plugins.php
	class Controller_Admin_Plugins extends Controller_Admin {
		// Bởi vì điều khiển này mở rộng Controller_Admin, nó sẽ có cùng kiểm tra đăng nhập
		
## $this->request

Mọi điều khiển có thuộc tính `$this->request` cái mà là đối tượng [Request] mà đã gọi điều khiển.
Bạn có thể sử dụng nó để nhận thông tin về yêu cầu hiện thời, cũng như thiết lập nội dung phản hồi từ `$this->response->body($ouput)`.

Đây là danh sách một phần các thuộc tính và phương thức của `$this->request`.
Những thứ này cũng có thể được truy cập từ `Request::instance()`, nhưng `$this->request` được cung cấp như một lối tắt.
Xem lớp [Request] để biết thêm thông tin về bất kỳ những thứ này.

Thuộc tính/phương thức | Nó làm gì
--- | ---
[$this->request->route()](../api/Request#property:route) | [Route] mà được khớp với với url yêu cầu hiện thời
[$this->request->directory()](../api/Request#property:directory), <br /> [$this->request->controller](../api/Request#property:controller), <br /> [$this->request->action](../api/Request#property:action) | Thư mục, điều khiển và hành động mà khớp với định tuyến hiện thời
[$this->request->param()](../api/Request#param) | Bất kỳ tham số khác được định nghĩa trong định tuyến của bạn
[$this->request->redirect()](../api/Request#redirect) | Điều hướng yêu cầu tới một url khác

## $this->response
Thuộc tính/phương thức | Nó làm gì
--- | ---
[$this->response->body()](../api/Response#property:body) | Nội dung trả về cho yêu cầu này
[$this->response->status()](../api/Response#property:status) | Mã trạng thái HTTP cho yêu cầu (200, 404, 500, v.v.)
[$this->response->headers()](../api/Response#property:headers) | Tiêu đề HTTP trả về với phản hồi


## Hành động

Bạn tạo các hành động cho điều khiển của bạn bằng việc định nghĩa một hàm công cộng với tiền tố `action_`.
Bất kỳ phương thức nào mà không khai báo `public` và bắt đầu với `action_` KHÔNG thể được gọi từ định tuyến.

Một phương thức hành động sẽ quyết định cái gì nên được thực hiện dựa trên yêu cầu hiện thời, nó *điều khiển* ứng dụng.
Có phải người dùng muốn lưu một bài viết blog?
Có phải họ cung cấp các trường cần thiết?
Họ có quyền làm điều đó?
Điều khiển sẽ gọi các lớp khác, bao gồm các mô hình, để thực hiện việc này.
Mọi hành động nên thiết lập `$this->response->body($view)` tới [tập tin khung nhìn](mvc/views) để được gửi tới trình duyệt, trừ khi nó [chuyển hướng](../api/Request#redirect) hoặc kết thúc kịch bản trước đó.

Một phương thức hành động rất cơ bản mà chỉ nạp một tập tin [khung nhìn](mvc/views).

	public function action_hello()
	{
		$this->response->body(View::factory('hello/world')); // Câu lệnh này sẽ nập tập tin views/hello/world.php
	}

### Các tham số

Các tham số được truy cập bằng cách gọi `$this->request->param('name')` nơi mà `name` là tên được định nghĩa trong định tuyến.

	// Giả sử Route::set('example','<controller>(/<action>(/<id>(/<new>)))');
	
	public function action_foobar()
	{
		$id = $this->request->param('id');
		$new = $this->request->param('new');

Nếu tham số đó không được thiết lập nó sẽ được trả về là NULL.
Bạn có thể cung cấp một tham số thứ hai để thiết lập một giá trị mặc định nếu tham số đó không được thiết lập.

	public function action_foobar()
	{
		// $id sẽ là false nếu nó không được cung cấp trong url
		$id = $this->request->param('user', FALSE);

### Ví dụ

Một hành động hiển thị cho một trang sản phẩm.

	public function action_view()
	{
		$product = new Model_Product($this->request->param('id'));

		if ( ! $product->loaded())
		{
			throw new HTTP_Exception_404('Sản phẩm không thấy!');
		}

		$this->response->body(View::factory('product/view')
			->set('product', $product));
	}

Mộ hành động đăng nhập của người dùng.

	public function action_login()
	{
		$view = View::factory('user/login');

		if ($_POST)
		{
			// Try to login
			if (Auth::instance()->login(arr::get($_POST, 'username'), arr::get($_POST, 'password')))
			{
				Request::current()->redirect('home');
			}

			$view->errors = 'Email hoặc mật khẩu không hợp lệ';
		}

		$this->response->body($view);
	}

## Trước và sau

Bạn có thể sử dụng các hàm `before()` và `after()` để có mã được thực thi trước hoặc sau khi hành động được thực thi.
Ví dụ, bạn có thể kiểm tra nếu người dùng đã đăng nhập, thiết lập một khuôn mẫu khung nhìn, nạp các tập tin yêu cầu, v.v.

Ví dụ, nếu bạn nhìn trong `Controller_Template` bạn có thể thấy điều đó trong điều khiển này

Bạn có thể kiểm tra hành động nào đã được yêu cầu (từ `$this->request->action`) và làm vài điều gì đó dựa trên điều đó, chẳng hạn như yêu cầu người dùng đăng nhập để sử dụng một điều khiển, trừ khi họ đang sử dụng hành động đăng nhập.

	// Kiểm tra auth/login trong hàm before, và điều hướng nếu cần thiết:

	Controller_Admin extends Controller {

		public function before()
		{
			// Nếu người dùng này không có vai trò admin, và không cố gắng đăng nhập, điều hướng tới hành động đăng nhập
			if ( ! Auth::instance()->logged_in('admin') AND $this->request->action !== 'login')
			{
				$this->request->redirect('admin/login');
			}
		}
		
		public function action_login() {
			...

### Hàm __construct() tuỳ biến

Nói chung, bạn không cần phải thay đổi hàm `__construct()`, như bất cứ điều gì bạn cần cho tất cả hành động có thể được thực hiện trong `before()`.
Nếu bạn cần thay đổi hàm dựng điều khiển, bạn phải bảo tồn các tham số hoặc PHP sẽ phàn nàn.
Điều này là để đối tượng Request mà đã gọi điều khiển có sẵn.
*Một lần nữa, trong hầu hết các trường hợp bạn có lẽ nên sử dụng `before()`, và không thay đổi hàm dựng*, nhưng nếu bạn thực sự, *thực sự* cần nó sẽ trong giống như thế này:

	// Bạn hầu như không bao giờ cần làm điều này, thay vào đó sử dụng hàm before()!

	// Chắc chắn rằng Kohana_Request là ở trong các tham số
	public function __construct(Request $request, Response $response)
	{
		// Bạn phải gọi parent::__construct tại điểm nào đó trong hàm của bạn
		parent::__construct($request, $response);
		
		// Làm bất cứ điều gì khác mà bạn muốn
	}

## Mở rộng các điều khiển khác

TODO: Thêm mô tả và các ví dụ của việc mở rộng các điều khiển khác, nhiều mở rộng, v.v.
