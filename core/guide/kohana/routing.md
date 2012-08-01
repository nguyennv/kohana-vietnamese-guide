# Định tuyến

Kohana cung cấp một hệ thống định tuyến rất mạnh.
Về bản chất, các định tuyến cung cấp một giao diện giữa các url và điều khiển và hành động của bạn.
Với các định tuyến đúng bạn có thể làm hầu như bất kỳ lược đồ url tương ứng tới hấu hết bất kỳ sự sắp xếp của các điều khiển, và bạn có thể thay đổi một định tuyến mà không ảnh hưởng đến định tuyến khác.

Như đã đề cập trong phần [Luồng yêu cầu](flow), một yêu cầu được xử lý bởi lớp [Request], cái mà sẽ tìm một đối tượng [Route] phù hợp và nạp điều khiển thích hợp để xử lý yêu cầu đó.

[!!] Điều quan trọng là phải hiểu rằng **các định tuyến được khớp theo thứ tự mà chúng được thêm vào**, và ngay sau khi một URL khớp với một định tuyến, bộ định tuyến về cơ bạn là "dừng lại" và **các định tuyến còn lại không bao giờ được thử**. Bởi vì định tuyến mặc định phù hợp với hầu hết bất kỳ thứ gì, bao gồm một url rỗng, các định tuyến mới phải đặt trước nó.

## Tạo định tuyến

Nếu bạn xem trong `APPPATH/bootstrap.php` bạn sẽ thấy định tuyến "mặc định" như sau:

	Route::set('default', '(<controller>(/<action>(/<id>)))')
	->defaults(array(
		'controller' => 'welcome',
		'action'     => 'index',
	));
	
[!!] Định tuyến mặc định chỉ đơn giản là cung cấp như một mẫu, bạn có thể loại bỏ nó và thay thế nó bằng các định tuyến của riêng bạn.

Vì vậy việc này tạo một định tuyến với tên `default` mà sẽ khớp các url trong định dạng `(<controller>(/<action>(/<id>)))`.

Chúng ta hãy xem xét kỹ hơn mỗi tham số của [Route::set], cái mà là `name`, `uri`, và một mảng tuỳ chọn `regex`.

### Tên

Tên của định tuyến phải là một chuỗi **duy nhất**.
Nếu không nó sẽ ghi đè các định tuyến cũ hơn có cùng tên.
Tên được sử dụng cho việc tạo các url bằng đảo ngược định tuyến, hoặc kiểm tra định tuyến nào đã được khớp.

### URI

Uri là một chuỗi mà đại diện cho định dạng của các url mà phải được khớp.
Các dấu hiệu - token bao quanh với <> là các **khoá** và bất kỳ thứ gì bao quanh với `()` là các phần *tuỳ chọn* của uri.
Trong các định tuyến Kohana, bất kỳ ký tự nào được phép và được đối xử đúng là ngoài `()<>`.
Ký tự `/` không có ý nghĩa nào ngoài việc là một ký tự phải khớp trong uri.
Thông thường `/` được sử dụng như một dấu phân tách tĩnh nhưng miễn là làm regex có được thấy, không có hạn chế tới cách mà bạn có thể định dạng các định tuyến của bạn.

Cho phép xem định tuyến mặc định lại lần nữa, uri là `(<controller>(/<action>(/<id>)))`.
Chúng ta có ba khoá hay tham số: controller, action, và id.
Trong trường hợp này toàn bộ uri là tuỳ chọn, do đó một url trống sẽ khớp và điều khiển và hành động mặc định (thiết lập bởi defaults(), [bao phủ bên dưới](#defaults)) sẽ là kết quả giả định trong lớp `Controller_Welcome` được nạp và phương thức `action_index` được gọi để xử lý yêu cầu.

Bạn có thể sử dụng bất kỳ tên nào mà bạn muốn cho các khoá của bạn, nhưng các khoá sau có ý nghĩa đặc biết tới đối tượng [Request], và sẽ gây ảnh hưởng cái mà điều khiển và hành động được gọi:

 * **Directory** - Thư mục con của `classes/controller` để tìm điều khiển (\[bao phủ bên dưới\](#directory))
 * **Controller** - Điều khiển mà yêu cầu sẽ thực thi.
 * **Action** - Phương thức hành động để gọi.

### Biểu thức chính quy - Regex

Hệ thống định tuyến Kohana sử dụng [biểu thức chính quy tương thích perl](http://perldoc.perl.org/perlre.html) trong quá trình khớp của nó.
Theo mặc định mỗi khoá (bao quanh bởi `<>`) sẽ khớp `[^/.,;?\n]++` (hoặc bằng tiếng Anh: bất kỳ thứ gì mà không là một dấu gạch chéo, dấu chấm, dấu phẩy, dấu chấm phải, dấu hỏi, hoặc dòng mới).
Bạn có thể định nghĩa các mẫu riêng của bạn bằng cách truyền một mảng liên kết các khoá và các mẫu như một tham số thứ ba bổ xung tới Route::set.

Trong ví dụ này, chúng ta có các điều khiển trong hai thư mục, `admin` và `affiliate`.
Bởi vì định tuyến này sẽ khớp các url mà bắt đầu với `admin` hoặc `affiliate`, định tuyến mặc định vẫn làm việc cho các điều khiển trong `classes/controller`.

	Route::set('sections', '<directory>(/<controller>(/<action>(/<id>)))',
		array(
			'directory' => '(admin|affiliate)'
		))
		->defaults(array(
			'controller' => 'home',
			'action'     => 'index',
		));

Bạn có thể sử dụng một regex ít hạn chế để khớp với các tham số không giới hạn, hoặc bỏ qua việc tràn trong một định tuyến.
Trong ví dụ này, url `foobar/baz/and-anything/else_that/is-on-the/url` sẽ được định tuyến tới `Controller_Foobar::action_baz()` và tham số `"stuff"` sẽ là `"and-anything/else_that/is-on-the/url"`.
Nếu bạn muốn sử dụng định tuyến này cho các tham số không giới hạn, bạn có thể [explode](http://php.net/manual/en/function.explode.php) nó, hoặc bạn chỉ bỏ qua việc tràn.

	Route::set('default', '(<controller>(/<action>(/<stuff>)))', array('stuff' => '.*'))
		->defaults(array(
			'controller' => 'welcome',
			'action' => 'index',
	  ));


### Giá trị mặc định

Nếu một khoá trong định tuyến là tuỳ chọn (hoặc không hiển diện ở trong định tuyến), bạn có thể cung cấp một giá trị mặc định cho khoá đó bằng cách truyền một mảng liên kết của các khoá và các giá trị mặc định tới [Route::defaults], được gọi dây truyền sau hàm [Route::set] của bạn.
Việc này có thể hữu ích để cung cấp một điều khiển hoặc hành động mặc định cho trang của bạn, trong những thứ khác.

[!!] Khoá `controller` và `action` phải luôn luôn có một giá trị, dó đó chúng hoặc cần được yêu cầu trong định tuyến của bạn (không phải bên trong dấu ngoặc đơn) hoặc có một giá trị mặc định được cung cấp.

Trong định tuyến mặc định, tất cả khoá là mặc định, và điều khiển và hành động được cho một giá trị mặc định.
Nếu chúng ta gọi một url trống, các giá trị mặc định sẽ điền vào và `Controller_Welcome::action_index()` sẽ được gọi.
Nếu chúng ta gọi `foobar` sau đó chỉ giá trị mặc định cho hành động sẽ được sử dụng, do đó nó sẽ gọi `Controller_Foobar::action_index()` và cuối cùng, nếu chúng ta gọi `foobar/baz` thì giá trị mặc định sẽ không được sử dụng và `Controller_Foobar::action_baz()` sẽ được gọi.

TODO: cần một ví dụ ở đây

Bạn cũng có thể sử dụng các giá trị mặc định để thiết lập một khoá mà không có trong định tuyến nào cả.

TODO: ví dụ của việc sử dụng thư mục hoặc điều khiển nơi mà nó không ở trong định tuyến, nhưng được thiết lập bởi các giá trị mặc định

### Thư mục

## Logic định tuyến bằng Lambda/Callback

Trong 3.1, bạn có thể chỉ định các lược đồ định tuyến nâng cao bằng cách sử dụng các định tuyến lambda.
Thay vì một URI, bạn có thể sử dụng một hàm ẩn danh hoặc cú pháp callback để chỉ định một hàm mà sẽ sử lý các định tuyến của bạn.
Dưới đây là một ví dụ đơn giản:

Nếu bạn muốn sử dụng định tuyến đảo ngược với các định tuyến lambda, bạn phải truyển tham số thứ ba:

	Route::set('testing', function($uri)
		{
			if ($uri == 'foo/bar')
				return array(
					'controller' => 'welcome',
					'action'     => 'foobar',
				);
		},
		'foo/bar'
	);

Như bạn có thể thấy trong định tuyến bên dưới, tham số uri đảo ngược có thể không có ý nghĩa.

	Route::set('testing', function($uri)
		{
			if ($uri == '</language regex/>(.+)')
			{
				Cookie::set('language', $match[1]);
				return array(
					'controller' => 'welcome',
					'action'     => 'foobar'
				);
			}
		},
		'<language>/<rest_of_uri>
	);

Nếu bạn đang sử dụng php 5.2, bạn vẫn có thể sử dụng các callback cho hành vi này (ví dụ này bỏ sót định tuyến đảo ngược):

	Route::set('testing', array('Class', 'method_to_process_my_uri'));

## Ví dụ

Có vố số khả năng khác cho các định tuyến. Dưới đây là một số ví dụ:

    /*
     * Lối tắt xác thực
     */
    Route::set('auth', '<action>',
      array(
        'action' => '(login|logout)'
      ))
      ->defaults(array(
        'controller' => 'auth'
      ));
      
    /*
     * Multi-format feeds
     *   452346/comments.rss
     *   5373.json
     */
    Route::set('feeds', '<user_id>(/<action>).<format>',
      array(
        'user_id' => '\d+',
        'format' => '(rss|atom|json)',
      ))
      ->defaults(array(
        'controller' => 'feeds',
        'action' => 'status',
      ));
    
    /*
     * Các trang tĩnh
     */
    Route::set('static', '<path>.html',
      array(
        'path' => '[a-zA-Z0-9_/]+',
      ))
      ->defaults(array(
        'controller' => 'static',
        'action' => 'index',
      ));
      
    /*
     * Bạn không thích dấu gạch chéo?
     *   EditGallery:bahamas
     *   Watch:wakeboarding
     */
    Route::set('gallery', '<action>(<controller>):<id>',
      array(
        'controller' => '[A-Z][a-z]++',
        'action'     => '[A-Z][a-z]++',
      ))
      ->defaults(array(
        'controller' => 'Slideshow',
      ));
      
    /*
     * Tìm kiếm nhanh
     */
    Route::set('search', ':<query>', array('query' => '.*'))
      ->defaults(array(
        'controller' => 'search',
        'action' => 'index',
      ));

## Tham số của yêu cầu

Các tham số `directory`, `controller` và `action` có thể được truy cập từ [Request] như các thuộc tính công cộng giống như vậy:

	// Từ bên trong một controller:
	$this->request->action();
	$this->request->controller();
	$this->request->directory();
	
	// Có thể được sử dụng ở bất cứ đâu:
	Request::current()->action();
	Request::current()->controller();
	Request::current()->directory();

Tất cả khoá khác chỉ định trong một định tuyến có thể được truy cập từ [Request::param()]:

	// Từ bên trong một điều khiển:
	$this->request->param('key_name');
	
	// Có thể được sử dụng ở bất cứ đâu:
	Request::current()->param('key_name');

Phương thức [Request::param] có một đối số tuỳ chọn thứ hai để chỉ định một giá trị trả về mặc định trong trường hợp khoá không được thiết lập bởi định tuyến.
Nếu không có đối số được đưa ra, tất cả khoá được trả về như một mảng liên kết.
Ngoài ra, `action`, `controller` và `directory` không được truy cập từ [Request::param()].

Ví dụ, với định tuyến sau đây:

	Route::set('ads','ad/<ad>(/<affiliate>)')
	->defaults(array(
		'controller' => 'ads',
		'action'     => 'index',
	));
	
Nếu url phù hợp với định tuyến, sau đó `Controller_Ads::index()` sẽ được gọi.
Bạn có thể truy cập các thông số bằng cách sử dụng phương thức `param()` của đối tượng [Request] của bộ điều khiển.
Hãy nhớ để xác định một giá trị mặc ​​định (thông qua tham số tùy chọn, thứ hai của[Request::param]) nếu không ở trong `->defaults()`.

	class Controller_Ads extends Controller {
		public function action_index()
		{
			$ad = $this->request->param('ad');
			$affiliate = $this->request->param('affiliate',NULL);
		}
	

## Nơi nào các định tuyến cần được định nghĩa?

Quy tắc đã thành lập là hoặc đặt các định tuyến tuỳ biến của bạn ở trong tập tin `MODPATH/<module>/init.php` của mô-đun của bạn nếu các định tuyến của bạn thuộc về một mô-đun, hoặc đơn giản là chèn chúng vào trong tập tin `APPPATH/bootstrap.php` (hãy chắc chắn để đặt chúng ở **trên** định tuyến mặc định) nếu chúng chỉ định tới ứng dụng.
Tất nhiên, không có gì ngăn cản bạn bao gồm chúng từ một tập tin bên ngoài, hoặc thậm chí tạo ra chúng động.

## Nhìn sâu hơn vào cách các định tuyến làm việc

TODO: nói về cách các định tuyến được biên dịch

## Tạo URL và liên kết bằng cách sử dụng định tuyến

Cùng với khả năng định tuyến mạnh mẽ của Kohana được bao gồm vài phương thức cho việc tạo các URL cho các uri của định tuyến của bạn.
Bạn có thể luôn luôn chỉ định các uri của bạn như một chuỗi bằng các sử dụng [URL::site] để tạo một URL đầy đủ như sau:

    URL::site('admin/edit/user/'.$user_id);

Tuy nhiên, Kohana cũng cung cấp một phương thức để sinh uri từ định nghĩa định tuyến.
Điều này cực kỳ hữu ích nếu nếu định tuyến của bạn có thể từng thay đổi từ khi nó sẽ làm dịu bạn từ việc phải trở lại thông qua mã của bạn và thay đổi ở mọi nơi mà bạn muốn chỉ định như một chuỗi.
Ở đây là một ví dụ của việc tạo ra động mà tương ứng tới ví dụ định tuyến `feeds` ở trên:

    Route::get('feeds')->uri(array(
      'user_id' => $user_id,
      'action' => 'comments',
      'format' => 'rss'
    ));

Hãy nói rằng bạn đã quyết định sau để làm định nghĩa định tuyến đó tiết lộ thêm bằng việc thay đổi nó tới `feeds/<user_id>(/<action>).<format>`.
Nếu bạn đã viết mã của bạn với phương thức tạo uri ở trên bạn sẽ không phải thay đổi một dòng duy nhất!
Khi một phần của uri được kèm theo trong ngoặc đơn và chỉ định một khoá cho cái mà không có giá trị cung cấp cho việc tạo uri và không có giá trị mặc định chỉ định trong định tuyến, sau đó phần đó sẽ được loại bỏ khỏi uri.
Một ví dụ của việc này là phần `(/<id>)` của định tuyến mặc định; phần này sẽ không được bao gồm trong uri được sinh nếu một id không được cung cấp.

Một phương thức bạn có thể sử dụng thường xuyên là lối tắt [Request::uri] cái mà giống như ở trên ngoại trừ nó giả định định tuyến hiện thời, directory, controller và action.
Nếu định tuyến hiện thời là mặc định và uri là `users/list`, chúng ta có thể làm như sau để tạo các uri trong định dạng `users/view/$id`:

    $this->request->uri(array('action' => 'view', 'id' => $user_id));
    
Hoặc nếu bên trong một khung nhìn, phương thức thích hợp hơn là:

    Request::instance()->uri(array('action' => 'view', 'id' => $user_id));

TODO: các ví dụ về sử dụng html::anchor ngoài các ví dụ ở trên

## Thử nghiệm định tuyến

TODO: đề cập đến mô-đun devtools của bluehawk