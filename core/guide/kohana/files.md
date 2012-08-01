# Hệ thống tập tin phân cấp

Hệ thống tập tin của Kohana là một hệ thống các cấu trúc thư mục phân cấp.
Hệ thống phân câp trong Kohana (được sử dụng khi một tập tin được nạp bởi hàm [Kohana::find_file]) là theo thứ tự sau:

1. **Đường dẫn ứng dụng**  
    Được định nghĩa là `APPPATH` trong `index.php`. Giá trị mặc định là `application`.

2. **Các đường dẫn mô-đun**  
    Đường dẫn này được thiết lập như một mảng kết hợp sử dụng `Kohana::modules` trong `APPPATH/bootstrap.php`. Mỗi giá trị của mang sẽ được tìm kiếm **theo thứ tự mà các mô-đun được định nghĩa**.

3. **Đường dẫn hệ thống**  
    Được định nghĩa `SYSPATH` trong `index.php`. Giá trị mặc định là `system`. Tất cả các tập tin và các lớp chính hoặc "lõi" được định nghĩa ở đây.

Các tập tin mà trong các thư mục cao hơn bao gồm đường dẫn được ưu tiên hơn các tập tin cùng tên ở dưới theo thứ tự, cái mà làm cho nó có thể quá tải (overload) bất kỳ tật tin nào bằng việc đặt một tập tin cùng tên trong một thư mục "cao hơn":

![Cascading Filesystem Infographic](cascading_filesystem.png)

Hình ảnh này chỉ hiển thị các tập tin nhất định, nhưng chúng ta có thể sử dụng nó để minh hoạ vài ví dụ của hệ thống tập tin phân cấp:

* Nếu Kohana bắt một lỗi, nó sẽ hiển thị khung nhìn `kohana/error.php`, vì vậy nó sẽ gọi `Kohana::find_file('views', 'kohana/error')`. Hàm này sẽ trả lại `application/views/kohana/error.php` bởi vì nó ưu tiên hơn `system/views/kohana/error.php`. Bằng cách này chúng ta có thể thay đổi khung nhìn báo lỗi mà không chỉnh sửa thư mục hệ thống.

* Nếu chúng ta đã sử dụng `View::factory('welcome')` nó sẽ gọi `Kohana::find_file('views','welcome')` cái mà sẽ trả về `application/views/welcome.php` bởi vì nó ưu tiên hơn `modules/common/views/welcome.php`. Bằng cách này, bạn có thể ghi đè những thứ trong một mô-đun mà không chỉnh sửa các tập tin mô-đun.

* Nếu sử dụng lớp Cookie, [Kohana::auto_load] sẽ gọi `Kohana::find_file('classes', 'cookie')` cái mà sẽ trả về `application/classes/cookie.php`. Giả sử Cookie mở rộng Kohana_Cookie, bộ nạp tự động sau đó gọi `Kohana::find_file('classes','kohana/cookie')` cái mà sẽ trả về `system/classes/kohana/cookie.php` bởi vì tập tin không tồn tại ở bất cứ nơi nào cao hơn trong hệ thống phân cấp. Đây là một ví dụ của [mở rộng trong suốt](extension).

* Nếu bạn sử dụng `View::factory('user')` nó sẽ gọi `Kohana::find_file('views','user')` cái mà sẽ trả về `modules/common/views/user.php`.

* Nếu chúng ta muốn thay đổi vài thứ trong `config/database.php` chúng ta có thể sao chép tập tin tới `application/config/database.php` và thực hiện các thay đổi đó. Hãy nhớ rằng [tập tin cấu hình được kết hợp](files/config#merge) hơn là ghi đè bởi hệ thống phân cấp.

## Các loại tập tin

Các thư mục ở cấp cao nhất của các đường dẫn ứng dụng, mô-đun và hệ thống có các thư mục mặc định sau đây:

classes/
:  Tất cả các lớp mà bạn muốn [nạp tự động](autoloading) sẽ được lưu ở đây. Thư mục này bao gồm [điều khiển](mvc/controllers), [mô hình](mvc/models), và tất cả các lớp khác. Tất cả các lớp phải thực hiện theo [các quy tắc đặt tên lớp](conventions#class-names-and-file-location).

config/
:  Các tập tin cấu hình trả về một mảng liên kết của các tuỳ chọn mà có thể được nạp bằng cách sử dụng [Kohana::config]. Các tập tin cấu hình được kết hợp hơn là ghi đè bởi hệ thống phân cấp. Xem [các tập tin cấu hình](files/config) để biết thêm thông tin.

i18n/
:  Các tập tin phiên dịch trả về một mảng liên kết các chuỗi. Việc phiên dịch được thực hiện bằng cách sử dụng phương thức `__()`. Để dịch "Hello, world!" sang tiếng Tây Ban Nha, bạn sẽ gọi `__('Hello, world!')` với `I18n::$lang` thiết lập tới "es-es". Các tập tin I18n được kết hợp hơn là ghi đè bởi hệ thống phân cấp. Xem các [tập tin I18n](files/i18n) để biết thêm thông tin.

messages/
:  Các tập tin thông điệp trả về một mạng liên kết các chuỗi mà có thể được nạp bằng các sử dụng [Kohana::message]. Các tập tin thông điệp và i18n khác nhau là các thông điệp không được phiên dịch, nhưng luôn luôn được viết bằng ngôn ngữ mặc định và được tham chiếu tới bằng một khoá duy nhất. Các tập tin thông điệp được kết hợp hơn là ghi đè bởi hệ thống phân cấp. Xem các [tập tin thông điệp](files/messages) để biết thêm thông tin.

views/
:  Các khung nhìn là các tập tin PHP mà được sử dụng để tạo ra HTML hoặc đầu ra khác. Tập tin khung nhìn được nạp vào trong một đối tượng [View] và được gán các biến, mà sau đó nó chuyển vào trong một đoạn HTML. Nhiều khung nhìn có thể được sử dụng bên trong khung nhìn khác. Xem [khung nhìn](mvc/views) để biết thêm thông tin.

*khác*
:  Bạn có thể bao gồm bất kỳ thư mục khác trong hệ thống tập tin phân cấp của bạn. Các ví dụ bao gồm, nhưng không bị giới hạn, `guide`, `vendor`, `media`, bất cứ thứ gì bạm muốn. Ví dụ, để tìm `media/logo.png` trong hệ thống tập tin phân cấp bạn sẽ gọi `Kohana::find_file('media','logo','png')`.

## Tìm các tập tin

Đường dẫn tới bất kỳ tập tin nào bên trong hệ thống tập tin có thể được tìm thấy bằng cách gọi `Kohana::find_file`:

    // Tìm đường dẫn đầy đủ tới "classes/cookie.php"
    $path = Kohana::find_file('classes', 'cookie');

    // Tìm đường dẫn đầy đủ tới "views/user/login.php"
    $path = Kohana::find_file('views', 'user/login');
	
Nếu tập tin không có phần mở rộng `.php`, hãy truyền phần mở rộng như tham số thứ ba.

	// Tìm đường dẫn đầy đủ tới "guide/menu.md"
	$path = Kohana::find_file('guide', 'menu', 'md');

	// Nếu $name là "2000-01-01-first-post" hàm này sẽ tìm cho "posts/2000-01-01-first-post.textile"
	$path = Kohana::find_file('posts', $name, '.textile');


## Mở rộng nhà cung cấp

Chúng tôi gọi các phần mở rộng hoặc các thư viện bên ngoài mà không chỉ định cho mở rộng "nhà cung cấp" Kohana, và chúng nằm trong thư mục vendor, hoặc trong ứng dụng hoặc trong một mô-đun.
Bởi vì các thư viện này không tuần theo quy tắc đặt tên tập tin của Kohana, chúng không thể được nạp tự động bởi Kohana, vì vậy bạn sẽ phải bao gồm chúng bằng tay.
Một vài ví dụ về các thư viện nhà cung cấp là [Markdown](http://daringfireball.net/projects/markdown/), [DOMPDF](http://code.google.com/p/dompdf), [Mustache](http://github.com/bobthecow/mustache.php) và [Swiftmailer](http://swiftmailer.org/).

Ví dụ, nếu bạn muốn sử dụng [DOMPDF](http://code.google.com/p/dompdf), bạn sẽ sao chép nó tới `application/vendor/dompdf` và bao gồm lớp nạp tự động của DOMPDF.
Nó có thể hữu ích để làm việc này trong một phương thức before của điều khiển, như một phần của một tập tin init.php của mô-đun, hoặc hàm dựng của một lớp singleton.

    require Kohana::find_file('vendor', 'dompdf/dompdf/dompdf_config','inc');

Bây giờ bạn có thể sử dụng DOMPDF mà không cần nạp thêm bất kỳ tập tin nào:

    $pdf = new DOMPDF;

[!!] Nếu bạn muốn chuyển đổi các khung nhìn thành các PDF bằng cách sử dụng DOMPDF, háy thử mô-đun [PDFView](http://github.com/shadowhand/pdfview).
