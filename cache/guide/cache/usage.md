# Cách dùng bộ nhớ đệm Kohana

[Kohana_Cache] cung cấp một giao diện đơn giản cho phép nhận, thiết lập và xoá các giá trị được nhớ đệm.
Hai giao diện được bao gồm trong _Bộ nhớ đệm Kohana_ cung cấp bổ xung _đánh thẻ_ và _thu gom rác_ nơi chúng được hỗ trợ bằng các điều khiển tương ứng.

## Lấy một thể hiện bộ nhớ đệm mới

Việc tạo một thể hiện _Bộ nhớ đệm Kohana_ mới là đơn giản, tuy nhiên nó phải được thực hiện bằng cách sử dụng phương thức [Cache::instance], hơn là hàm dựng `new` truyền thống.

     // Tạo một thể hiện mới của bộ nhớ đệm bằng cách sử dụng nhóm mặc định
     $cache = Cache::instance();

Nhóm mặc định sẽ được sử dụng bất cứ gì được thiết lập tới [Cache::$default] và phải có một định nghĩa [cấu hình](config) tương đương cho nhóm đó.

Để tạo một thể hiện bộ nhớ đệm sử dụng một nhóm khác với _mặc định_, bạn chỉ cần cung cấp tên nhóm làm đối số.

     // Tạo một thể hiện mới của nhóm memcache
     $memcache = Cache::instance('memcache');

Nếu có một thể hiện bộ nhớ đệm đã được khởi tạo thì bạn có thể lấy nó trực tiếp từ thành viên lớp.

 [!!] Chú ý rằng việc này có thể gây ra các vấn đề nếu bạn không thử nghiệm cho thể hiện trước khi cố gắng truy cập nó.

     // Kiểm tra tồn tại của trình điều khiển bộ nhớ đệm
     if (isset(Cache::$instances['memcache']))
     {
          // Lấy thể hiện bộ nhớ đệm hiện có một cách trực tiếp (nhanh hơn)
          $memcache = Cache::$instances['memcache'];
     }
     else
     {
          // Lấy thể hiện trình điều khiển bộ nhớ đệm (chậm hơn)
          $memcache = Cache::instance('memcache');
     }

## Thiết lập và nhận các biến tới và từ bộ nhớ đệm

Thư viện bộ nhớ đệm hỗ trợ các giá trị vô hướng và đối tượng, sử dụng tuần tự hoá đối tượng khi yêu cầu (hoặc không hỗ trợ bởi động cơ bộ nhớ đêm).
Điều này có nghĩa rằng đa số hoặc các đối tượng có thể được lưu vào bộ nhớ đệm mà không cần bất kỳ sửa đổi nào.

 [!!] Tuần tự hoá không làm việc với các xử lý tài nguyên, chẳng hạn như các tài nguyên của hệ thống tập tin, curl hoặc ổ cắm.

### Thiết lập một giá trị cho bộ nhớ đệm

Việc thiết lập một giá trị tới bộ nhớ đệm bằng cách sử dụng phương thức [Cache::set] có thể được thực hiện trong hai cách;
hoặc sử dụng giao diện thể hiện Cache, cái mà tốt cho các hoạt động nguyên tử;
hoặc nhận một thể hiển và sử dụng cho nhiều hoạt động.

Ví dụ đầu tiên cho thấy cách nhanh chóng nạp và thiết lập một giá trị tới thể hiện bộ nhớ đệm mặc định.

     // Tạo một đối tượng có thể lưu vào bộ nhớ đệm
     $object = new stdClass;

     // Thiết lập một thuộc tính
     $object->foo = 'bar';

     // Lưu bộ nhớ đệm đối tượng bằng cách sử dụng nhóm mặc định (giao diện nhanh) với thời gian mặc định (3600 giây)
     Cache::instance()->set('foo', $object);

Nếu nhiều thao tác bộ nhớ đệm được yêu cầu, tốt nhất là gán một thể hiện của Cache tới một biến và sử dụng như dưới đây.

     // Thiết lập đối tượng bằng cách sử dụng một nhóm đã định nghĩa cho một khoảng thời gian quy định (30 giây)
     $memcache = Cache::instance('memcache');
     $memcache->set('foo', $object, 30);

#### Thiết lập một giá trị với các thẻ

Một số trình điều khiển bộ nhớ đệm hỗ trợ việc thiết lập các giá trị với các thẻ.
Để thiết lập một giá trị tới bộ nhớ đệm với thẻ sử dụng giao diện sau đây.

     // Nhận một thể hiện bộ nhớ đệm mà hỗ trợ thẻ
     $memcache = Cache::instance('memcachetag');

     // Kiểm tra giao diện gắn thẻ
     if ($memcache instanceof Cache_Tagging)
     {
          // Thiết lập một giá trị với vài thẻ trong 30 giây
          $memcache->set('foo', $object, 30, array('snafu', 'stfu', 'fubar'));
     }
     // Nếu không thiết lập mà không có thẻ
     else
     {
          // Thiết lập một giá trị trong 30 giây
          $memcache->set('foo', $object, 30);
     }

Nó có thể thực hiện các giải pháp gắn thẻ tuỳ biến hoặc các trình điều khiển bộ nhớ đệm mới bằng việc thực hiện giao diện [Cache_Tagging]. Kohana_Cache chỉ áp dụng giao diện tới các trình điều khiển mà hỗ trợ gắn thẻ tự nhiên như là tiêu chuẩn.

### Lấy một giá trị từ bộ nhớ đệm

Việc nhận các biến trở lại từ bộ nhớ đệm đạt được bằng cách sử dụng phương thức [Cache::get] sử dụng một khoá duy nhất để xác định mục bộ nhớ đệm.

     // Lấy một giá trị từ bộ nhớ đệm (nhanh)
     $object = Cache::instance()->get('foo');

Trong các trường hợp nơi mà khoá yêu cầu không có sẵn hoặc mục đã hết hạn, một giá trị mặc định sẽ được trả về (mặc định là __NULL__).
Nó có thể định nghĩa giá trị mặc định như là khoá được yêu cầu.

     // Nếu khoá bộ nhớ đệm có sẵn (với giá trị mặc định thiết lập là FALSE)
     if ($object = Cache::instance()->get('foo', FALSE))
     {
          // Làm điều gì đó
     }
     else
     {
          // Làm điều gì khác
     }

#### Lấy giá trị từ bộ nhớ đệm bằng cách sử dụng thẻ

Có thể lấy các giá trị từ bộ nhớ đệm được nhóm theo thẻ, bằng cách sử dụng phương thức [Cache::find] với các trình điều khiển hỗ trợ gắn thẻ.

 [!!] Trình điều khiển __Memcachetag__ không hỗ trợ giao diện `Cache::find($tag)` và sẽ ném ra một ngoại lệ.

     // Lấy môt thể hiện của bộ nhớ đệm
     $cache = Cache::instance('memcachetag');

     // Bao bọc trọng một câu lệnh try/catch để xử lý một cách tinh tế memcachetag
     try
     {
          // Tìm các giá trị dựa trên thẻ
          return $cache->find('snafu');
     }
     catch (Cache_Exception $e)
     {
          // Xử lý một cách tinh tế
          return FALSE;
     }

### Xóa các giá trị từ bộ nhớ đệm

Việc xoá các biến rất giống các phương thức lấy và thiết lập đã mô tả.
Thao tác xoá được chia thành ba loại:

 - __Xoá giá trị theo khoá__. Xoá một giá trị lưu bộ nhớ đệm bằng khoá liên kết.
 - __Xoá tất cả giá trị__. Xoá tất cả giá trị lưu bộ nhớ đệm đã lưu trong thể hiện bộ nhớ đệm.
 - __Xoá giá trị theo thẻ__. Xoá tất cả giá trị mà có cung cấp thẻ. Việc này chỉ được hỗ trợ bởi Memcached-Tag và Sqlite.

#### Xóa giá trị theo khóa

Để xóa một giá trị cụ thể bởi khoá liên kết của nó:

     // Nếu mục bộ nhớ đệm cho 'foo' bị xoá
     if (Cache::instance()->delete('foo'))
     {
          // Mục bộ nhớ đệm xoá thành công, làm một vài cái gì đó
     }

Theo mặc định một giá trị `TRUE` sẽ được trả về.
Tuy nhiên một giá trị `FALSE` sẽ được trả về trong các thể hiện nơi mà khoá không tồn tại trong bộ nhớ đệm.

#### Xóa tất cả giá trị

Để xóa tất cả giá trị trong một thể hiện cụ thể:

     // Nếu tât cả các mục bộ nhớ đệm được xoá thành công
     if (Cache::instance()->delete_all())
     {
           // Làm điều gì đó
     }

Nó cũng có thể xoá tất cả các mục bộ nhớ đệm trong mọi thể hiện:

     // Đối với mỗi thể hiện bộ nhớ đệm
     foreach (Cache::$instances as $group => $instance)
     {
          if ($instance->delete_all())
          {
               var_dump('instance : '.$group.' has been flushed!');
          }
     }

#### Xóa các giá trị theo thẻ

Một số trình điều khiển bộ nhớ đệm hỗ trợ việc xoá theo thẻ.
Việc này sẽ loại bỏ tất cả các giá trị được lưu bộ nhớ điệm mà được liên kết với một thẻ cụ thể.
Bên dưới là một ví dụ về cách xử lý mạnh mẽ việc xoá theo thẻ.

     // Lấy một thể hiện bộ nhớ đệm
     $cache = Cache::instance();

     // Kiểm tra giao diện gắn thẻ
     if ($cache instanceof Cache_Tagging)
     {
           // Xoá tất cả các mục theo thẻ 'snafu'
           $cache->delete_tag('snafu');
     }

#### Thu gom rác

Garbage Collection (GC) - Thu gom rác là làm sạch các mục bộ nhớ đệm hết hạn.
Đối với hầu hết các phần, động cơ bộ nhớ đệm sẽ lo việc thu gom rác ở bên trong.
Tuy nhên một vài trong số các hệ thống dựa trên tập tin không xử lý tác vụ này và trong các hoàn cảnh này sẽ được thận trong thu gom rác ở một tần số xác định trước.
Nếu không có việc thu gom rác được thực thi, tài nguyên lưu trữ các mục bộ nhớ đệm cuối cùng sẽ điền vào và trở lên không sử dụng được.

Khi không tự động, việc thu gom rác là trách nhiệm của nhà phát triển.
Thận trọng để có một giá trị xác suất GC mà ra lệnh cách có thể định tuyến thu gom rác sẽ được chạy.
Một ví dụ một hệ thống như vậy được thể hiện bên dưới.

     // Lấy một thể hiện bộ nhớ đệm
     $cache_file = Cache::instance('file');

     // Thiết lập một xác suất GC 10%
     $gc = 10;

     // Nếu xác suất GC là một lượt
     if (rand(0,99) <= $gc and $cache_file instanceof Cache_GarbageCollect)
     {
          // Thu gom rác
          $cache_file->garbage_collect();
     }

# Giao diện

Bộ nhớ đệm Kohana đi kèm với hai giai diện mà được thực hiện nơi các trình điều khiển hỗ trợ chúng:

 - __[Cache_Tagging] hỗ trợ tính năng gắn thể trên các mục bộ nhớ đệm__
    - [Cache_MemcacheTag]
    - [Cache_Sqlite]
 - __[Cache_GarbageCollect] cho việc thu gom rác với các trình điều khiển mà không hỗ trợ gốc__
    - [Cache_File]
    - [Cache_Sqlite]

Khi sử dụng giao diện các tính năng bộ nhớ đệm cụ thể, hãy đảm bảo rằng mã kiểm tra cho giao diện yêu cầu trước khi sử dụng các phương thức đã cung cấp.
Ví dụ sau kiểm tra xem giao diện thu gom rác có sẵn trước khi gọi phương thức `garbage_collect`.

    // Tạo một thể hiện bộ nhớ đệm
    $cache = Cache::instance();

    // Kiểm tra Thu gom rác
    if ($cache instanceof Cache_GarbageCollect)
    {
         // Thu gom rác
         $cache->garbage_collect();
    }