# Mở rộng lớp trong suốt

[Hệ thống tập tin phân cấp](files) cho phép mở rộng các lớp một cách trong suốt.
Ví dụ, lớp [Cookie] được định nghĩa trong SYSPATH/classes/cookie.php như:

    class Cookie extends Kohana_Cookie {}

Các lớp Kohana mặc định, và nhiêu mở rộng, sử dụng định nghĩa này để cho hầu hết tất cả các lớp có thể được mở rộng.
Bạn mở rộng bất kỳ lớp nào một cách trong suốt, bằng việc định nghĩa lớp của riêng bạn trong `APPPATH/classes/cookie.php` và thêm các phương thức của riêng bạn.

[!!] Bạn **không** nên sửa đổi bất kỳ tập tin nào mà được phân phối với Kohana. Luôn luôn thực hiện các thay đổi tới các lớp bằng cách sử dụng mở rộng trong suốt để ngăn chặn các vấn đề nâng cấp.

Ví dụ, nếu bạn muốn tạo phương thức mà thiết lập các cookie được mã hoá bằng cách sử dụng lớp [Encrypt] bạn nên tạo một tập tin tại `application/classes/cookie.php` mà mở rộng từ Kohana_Cookie, và thêm các hàm của bạn:

    <?php defined('SYSPATH') or die('No direct script access.');

    class Cookie extends Kohana_Cookie {

        /**
         * @var  mixed  default encryption instance
         */
        public static $encryption = 'default';

        /**
         * Sets an encrypted cookie.
         *
         * @uses  Cookie::set
         * @uses  Encrypt::encode
         */
         public static function encrypt($name, $value, $expiration = NULL)
         {
             $value = Encrypt::instance(Cookie::$encrpytion)->encode((string) $value);

             parent::set($name, $value, $expiration);
         }

         /**
          * Gets an encrypted cookie.
          *
          * @uses  Cookie::get
          * @uses  Encrypt::decode
          */
          public static function decrypt($name, $default = NULL)
          {
              if ($value = parent::get($name, NULL))
              {
                  $value = Encrypt::instance(Cookie::$encryption)->decode($value);
              }

              return isset($value) ? $value : $default;
          }

    } // End Cookie

Bây giờ việc gọi `Cookie::encrypt('secret', $data)` sẽ tạo một cookie được mã hoá cái mà chúng ta có thể giải mã với `$data = Cookie::decrypt('secret')`.

## Cách nó làm việc

Để hiểu cách làm việc này, chúng ta hãy nhìn vào những gì xảy ra bình thường.
Khi bạn sử dụng lớp Cookie [Kohana::autoload] tìm kiếm `classes/cookie.php` trong [hệ thống tập tin phân cấp](files).
Nó tìm trong `application`, sau đó với mỗi mô-đun, sau đó `system`. Tập tin được tìm thấy trong system và được bao gồm.
Tất nhiên, `system/classes/cookie.php` chỉ là một lớp rỗng mà mở rộng từ `Kohana_Cookie`.
Một lần nữa, `Kohana::autoload` được gọi lần này tìm `classes/kohana/cookie.php` cái mà nó tìm thấy trong `system`.

Khi bạn thêm lớp cookie đã mở rộng trong suốt ở `application/classes/cookie.php` tập tin này về cơ bạn "thay thế" tập tin ở `system/classes/cookie.php` mà không thực sự chạm vào nó.
Điều này xảy ra bởi vì thời điểm này khi chúng ta sử dụng lớp Cookie [Kohana::autoload] tìm kiếm cho `classes/cookie.php` và tìm tập tin trong `application` và bao gồm tập tin đó, thay vì tập tin trong hệ thống

## Ví dụ: thay đổi thiết lập [Cookie]

Nếu bạn đang sử dụng lớp [Cookie](cookies), và muốn thay đổi một thiết lập, bạn nên làm bằng cách sử dụng mở rộng trong suốt, thay vì chỉnh sửa tập tin trong thư mục hệ thống.
Nếu bạn chỉnh sửa nó một cách trực tiếp, và trong tương lai bạn nâng cấp phiên bản Kohana của bạn bằng cách thay thế thư mục hệ thống, các thay đổi của bạn sẽ được hoàn nguyên và các cookie của bạn sẽ có thể không hợp lệ.
Thay vào đó, tạo một tập tin cookie.php hoặc ở trong `application/classes/cookie.php` hoặc trong một mô-đun (`MODPATH/<modulename>/classes/cookie.php`).

	class Cookie extends Kohana_Cookie {
	
		// Thiết lập một biến salt mới
		public $salt = "vài cái mới tốt hơn cụm từ salt ngẫu nhiên;
		
		// Không cho phép javascript truy cập tới các cookie
		public $httponly = TRUE;
		
	}

## Ví dụ: TODO: một ví dụ

Chỉ cần đăng mã và mô tả ngắn gọn những hàm gì nó bổ xung, bạn không phải làm "Cách nó làm việc" giống như ở trên.

## Ví dụ: TODO: cái gì đó khác

Chỉ cần đăng mã và mô tả ngắn gọn những hàm gì nó bổ xung, bạn không phải làm "Cách nó làm việc" giống như ở trên.

## Các ví dụ khác

TODO: Cung cấp vài liên kết tới các mô-đun trên github, vv mà có ví dụ về mở rộng trong suốt đang sử dụng.

## Nhiều mức mở rộng

Nếu bạn đang mở rộng một lớp Kohana trong một mô-đun, bạn nên duy trì mở rộng trong suốt.
Nói cách khác, không bao gồm bất kỳ biến hoặc hàm trong lớp "cơ sở" (vd. Cookie).
Thay vào đó làm cho lớp có không gian tên của riêng bạn, vào có lớp "có sở" mở rộng từ lớp đó.
Với ví dụ cookie được mã hoá chúng ta có thể tạo `MODPATH/mymod/encrypted/cookie.php`:

	class Encrypted_Cookie extends Kohana_Cookie {

		// Sử dụng cùng phương thức encrypt() decrypt() ở trên

	}

Và tạo `MODPATH/mymod/cookie.php`:

	class Cookie extends Encrypted_Cookie {}

Điều này vẫn cho phép người dùng thêm phần mở rộng của riêng họ tới [Cookie] trong khi để lại phần mở rộng của bạn nguyên vẹn.
Để làm được điều đó họ sẽ làm một lớp cookie mà mở rộng `Encrypted_Cookie` (chứ không phải `Kohana_Cookie`) trong thư mục ứng dụng của họ.
