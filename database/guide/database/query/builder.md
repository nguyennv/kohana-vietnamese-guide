# Trình dựng truy vấn

Việc tạo các truy vấn động bằng cách sử dụng các đối tượng và các phương thức cho phép các truy vấn được viết rất nhanh trong một cách bất khả tri.
Việc xây dựng truy vấn cũng thêm trích dẫn định danh (tên bảng và cột), cũng như trích dẫn giá trị.

[!!] Vào lúc này, nó không thể kết hợp việc xây dựng truy vấn với câu lệnh đã chuẩn bị.

## Lựa chọn

Mỗi loại truy vẫn cơ sở dữ liệu được đại diện bởi một lớp khác nhau, với mỗi phương thức của riêng chúng.
Ví dụ, để tạo một truy vấn SELECT, chúng ta sử dụng [DB::select] cái mà là một lối tắt để trả về một đối tượng [Database_Query_Builder_Select] mới:

    $query = DB::select();

Các phương thức xây dựng truy vấn trả về một tham chiếu tới chính nó để việc gọi dây chuyền phương thức có thể được sử dụng.
Các truy vấn lựa chọn thường yêu cầu một bảng và chúng được tham chiếu bằng cách sử dụng phương thức `from()`.
Phương thức `from()` có một tham số cái mà có thể là một tên bảng (kiểu chuỗi), một mảng hai chuỗi (tên bảng và bí danh), hoặc một đối tượng (Xem Truy vấn con ở khu vực Truy vấn nâng cao bên dưới).

    $query = DB::select()->from('users');

Giới hạn các kết quả của truy vấn được thực hiện bằng cách sử dụng các phương thức `where()`, `and_where()` và `or_where()`.
Các phương thức này có ba tham số: một cột, một toán tử, và một giá trị.

    $query = DB::select()->from('users')->where('username', '=', 'john');

Nhiều phương thức `where()` có thể được sử dụng để nối chuỗi với nhau nhiều điều kiện được kết nối bằng toán tử boolean trong tiền tố của phương thức.
Phương thức `where()` làm một trình bọc mà chỉ cần gọi and_where().

    $query = DB::select()->from('users')->where('username', '=', 'john')->or_where('username', '=', 'jane');
	
Bạn có thể sử dụng bất kỳ toán tử này bạn muốn.
Ví dụ bao gồm `IN`, `BETWEEN`, `>`, `=<`, `!=`, v.v.
Sử dụng một mảng các toán tử yêu cầu có nhiều hơn một giá trị.

	$query = DB::select()->from('users')->where('logins', '<=', 1);
	
	$query = DB::select()->from('users')->where('logins', '>', 50);
	
	$query = DB::select()->from('users')->where('username', 'IN', array('john','mark','matt'));
	
	$query = DB::select()->from('users')->where('joindate', 'BETWEEN', array($then, $now));

Theo mặc định, [DB::select] sẽ lựa chọn tất cả cột (SELECT * ...), nhưng bạn cũng có thể chỉ định cột nào bạn muốn trả về bằng cách truyền các tham số tới [DB::select]:

    $query = DB::select('username', 'password')->from('users')->where('username', '=', 'john');

Giờ hãy lấy một phút để nhìn vào những gì gọi dây truyền phương thức này đang làm.
Trước tiên chúng ta tạo một đối tượng chọn lựa mới bằng các sử dụng phương thức [DB::select].
Kết tiếp, chúng ta thiết lập các bảng bằng cách sử dụng phương thức `from()`.
Cuối cùng, chúng ta tìm kiếm các bạn ghi cụ thể bằng cách sử dụng phương thức` where()`.
Chúng ta có thể hiển thị SQL mà sẽ được chạy bằng cách đổi kiểu truy vấn tới một chuỗi:

    echo Kohana::debug((string) $query);
    // Sẽ hiển thị:
    // SELECT `username`, `password` FROM `users` WHERE `username` = 'john'

Chú ý cách các tên cột và bảng được tự động thoát, cũng như các giá trị?
Đây là một trong những lợi ích chính của việc sử dụng trình dựng truy vấn.

### Lựa chọn - AS (bí danh cột)

Nó cũng có thể tạo các bí danh `AS` khi lựa chọn, bằng việc truyền một mảng như mỗi tham số tới [DB::select]:

    $query = DB::select(array('username', 'u'), array('password', 'p'))->from('users');

Truy vấn này sẽ tạo ra SQL sau:

    SELECT `username` AS `u`, `password` AS `p` FROM `users`

### Lựa chọn - DISTINCT

Các giá trị cột duy nhất có thể được bật hoặc tắt (mặc định) bằng việc truyền TRUE hoặc FALSE, tương ứng, tới phương thức `distinct()`.

    $query = DB::select('username')->distinct(TRUE)->from('posts');

Truy vấn này sẽ tạo ra SQL sau:

    SELECT DISTINCT `username` FROM `posts`

### Lựa chọn - LIMIT & OFFSET

Khi truy vấn các tập lớn dữ liệu, thường tốt hơn để hạn chế kết quả và phân trang thông qua phân khúc dữ liệu tại một thời điểm.
Việc này được thực hiện bằng cách sử dụng các phương thức `limit()` và `offset()`.

    $query = DB::select()->from(`posts`)->limit(10)->offset(30);

Truy vấn này sẽ tạo ra SQL sau:

    SELECT * FROM `posts` LIMIT 10 OFFSET 30

### Lựa chọn - ORDER BY

Thường bạn sẽ muốn các kết quả trong một thứ tự cụ thể thay vì sắp xếp các kết quả, nó tốt hơn để có kết quả trả về cho bạn trong thứ tự đúng.
Bạn có thể làm việc này bằng cách sử dụng phương thức `order_by()`.
Nó lấy tên cột và chuỗi định hướng tuỳ chọn như các tham số.
Nhiều phương thức `order_by()` có thể được sử dụng để bổ xung thêm khả năng sắp xếp.

    $query = DB::select()->from(`posts`)->order_by(`published`, `DESC`);

Truy vấn này sẽ tạo ra SQL sau:

    SELECT * FROM `posts` ORDER BY `published` DESC

[!!] Đối với một danh sách đầy đủ các phương thức trong khi xây dựng một truy vấn lựa chọn xem [Database_Query_Builder_Select].

## Chèn

Để tạo các bản ghi vào cơ sở dữ liệu, sử dụng [DB::insert] để tạo một truy vấn INSERT, sử dụng `values()` để truyền vào dữ liệu:

    $query = DB::insert('users', array('username', 'password'))->values(array('fred', 'p@5sW0Rd'));

Truy vấn này sẽ tạo ra SQL sau:

    INSERT INTO `users` (`username`, `password`) VALUES ('fred', 'p@5sW0Rd')

[!!] Đối với một danh sách đầy đủ các phương thức trong khi xây dựng một truy vấn chèn xem [Database_Query_Builder_Insert].

## Cập nhật

Để thay đổi một bản ghi hiện có, sử dụng [DB::update] để tạo một truy vấn UPDATE:

    $query = DB::update('users')->set(array('username' => 'jane'))->where('username', '=', 'john');

Truy vấn này sẽ tạo ra SQL sau:

    UPDATE `users` SET `username` = 'jane' WHERE `username` = 'john'

[!!] Đối với một danh sách đầy đủ các phương thức trong khi xây dựng một truy vấn cập nhật xem [Database_Query_Builder_Update].

## Xoá

Để loại bỏ một bản ghi có sẵn, sử dụng [DB::delete] để tạo truy vấn DELETE:

    $query = DB::delete('users')->where('username', 'IN', array('john', 'jane'));

Truy vấn này sẽ tạo ra SQL sau:

    DELETE FROM `users` WHERE `username` IN ('john', 'jane')

[!!] Đối với một danh sách đầy đủ các phương thức trong khi xây dựng một truy vấn xoá xem [Database_Query_Builder_Delete].

## Truy vấn nâng cao

### Ghép nối

Nhiều bảng có thể được nối lại bằng cách sử dụng các phương thức `join()` và `on()`.
Phương thức `join()` có hai tham số.
Tham số đầu là hoặc một tên bảng, một mảng chứa bảng và bí dánh, hoặc một đối tượng (truy vấn con hoặc biểu thức).
Tham số thứ hai là kiểu ghép nối LEFT, RIGHT, INNER, v,v.

Phương thức `on()` thiết lập các điều kiển cho phương thức `join()` trước đó và rất giống với phương thức `where()` mà trong đó có ba tham số;
cột bên trái (tên hoặc đối tượng), một toán tử, và cột bên phải (tên hoặc đối tượng).
Nhiều phương thức `on()` có thể được sử dụng để cung cấp nhiều điều kiện và chúng sẽ được nối thêm với một toán tử 'AND'.

    // Truy vấn này sẽ tìm tất cả bài viết liên quan tới "smith" với JOIN
    $query = DB::select('authors.name', 'posts.content')->from('authors')->join('posts')->on('authors.id', '=', 'posts.author_id')->where('authors.name', '=', 'smith');

Truy vấn này sẽ tạo ra SQL sau:

    SELECT `authors`.`name`, `posts`.`content` FROM `authors` JOIN `posts` ON (`authors`.`id` = `posts`.`author_id`) WHERE `authors`.`name` = 'smith'

Nếu bạn muốn làm một LEFT, RIGHT hoặc INNER JOIN ban sẽ làm điều đó giống như `join('colum_name', 'type_of_join')`:

    // Truy vấn này sẽ tìm tất cả bài viết liên quan tới "smith" LEFT JOIN
    $query = DB::select()->from('authors')->join('posts', 'LEFT')->on('authors.id', '=', 'posts.author_id')->where('authors.name', '=', 'smith');

Truy vấn này sẽ tạo ra SQL sau:

    SELECT `authors`.`name`, `posts`.`content` FROM `authors` LEFT JOIN `posts` ON (`authors`.`id` = `posts`.`author_id`) WHERE `authors`.`name` = 'smith'

[!!] Khi ghép nối nhiều bảng với các tên cột tương tự, tốt nhất là đặt tiền tố các cột với tên bảng hoặc bí danh bảng để tránh các lỗi. Tên cột không rõ ràng cũng nên được đặt bí danh để chúng có thể được tham chiếu dễ dàng hơn.

### Hàm cơ sở dữ liệu

Sau cùng bạn sẽ có thể chạy vào môt tình huống mà bạn cần gọi hàm `COUNT` hoặc các hàm cơ sở dữ liệu khác trong truy vấn của bạn.
Trình dựng truy vấn hỗ trợ các hàm này trong hai cách.
Cách đầu tiên là bằng cách sử dụng dấu nháy kép với bí danh:

    $query = DB::select(array('COUNT("username")', 'total_users'))->from('users');

Truy vấn này nhìn hầu như chính xác giống như một bí danh `AS` chuẩn, nhưng lưu ý cách tên cột được bọc trong các dấu nháy kép.
Bất cứ lúc nào giá trị dấu nháy kém xuất hiện bên trong môt tên cột, **chỉ** có một phần bên trong các dấu nháy kép được thoát.
Truy vấn này sẽ tạo ra SQL sau:

    SELECT COUNT(`username`) AS `total_users` FROM `users`

[!!] Khi xây dựng các truy vấn phức tạp và bạn cần lấy một số lượng tổng cộng các hàng mà sẽ được trả về, xây dựng biểu thức với danh sách cột rỗng trước tiên. Sau đó nhân bản truy vấn và thêm hàm COUNT tới một bản sao và danh sách cột tới truy vấn khác. Việc này sẽ cắt giảm tổng số dòng mã và thưc hiện cập nhật truy vấn dễ dàng hơn.

    $query = DB::select()->from('users')
        ->join('posts')->on('posts.username', '=', 'users.username')
        ->where('users.active', '=', TRUE)
        ->where('posts.created', '>=', $yesterday);
    
    $total = clone $query;
    $total->select(array('COUNT( DISTINCT "username")', 'unique_users'));
    $query->select('posts.username')->distinct();

### Hàm tổng hợp

Các hàm tổng hợp như `COUNT()`, `SUM()`, `AVG()`, v.v. sẽ rất có thể được sử dụng với các phương thức `group_by()` và có thể là `having()` để nhóm và lọc các kết quả trên một tập các cột.

    $query = DB::select('username', array('COUNT("id")', 'total_posts')
        ->from('posts')->group_by('username')->having('total_posts', '>=', 10);

Truy vấn này sẽ tạo ra SQL sau:

    SELECT `username`, COUNT(`id`) AS `total_posts` FROM `posts` GROUP BY `username` HAVING `total_posts` >= 10

### Truy vấn con

Các đối tượng trình tạo truy vấn có thể được truyền như các tham số tới nhiều phương thức để tạo các truy vấn con.
Hãy lấy truy vấn ví dụ trước và truyền nó tới một truy vấn mới.

    $sub = DB::select('username', array('COUNT("id")', 'total_posts')
        ->from('posts')->group_by('username')->having('total_posts', '>=', 10);
    
    $query = DB::select('profiles.*', 'posts.total_posts')->from('profiles')
        ->join(array($sub, 'posts'), 'INNER')->on('profiles.username', '=', 'posts.username');

Truy vấn này sẽ tạo ra SQL sau:

    SELECT `profiles`.*, `posts`.`total_posts` FROM `profiles` INNER JOIN
    ( SELECT `username`, COUNT(`id`) AS `total_posts` FROM `posts` GROUP BY `username` HAVING `total_posts` >= 10 ) AS posts
    ON `profiles`.`username` = `posts`.`username`

Các truy vấn chèn cũng có thể sử dụng một truy vấn lựa chọn cho các giá trị đầu vào

    $sub = DB::select('username', array('COUNT("id")', 'total_posts')
        ->from('posts')->group_by('username')->having('total_posts', '>=', 10);
    
    $query = DB::insert('post_totals', array('username', 'posts'))->select($sub);

Truy vấn này sẽ tạo ra SQL sau:

    INSERT INTO `post_totals` (`username`, `posts`) 
    SELECT `username`, COUNT(`id`) AS `total_posts` FROM `posts` GROUP BY `username` HAVING `total_posts` >= 10 

### Toán tử boolean và điều kiện lồng nhau 

Nhiều điền kiện Where và Having có thể được thêm vào truy vấn các các toán tử Boolean kết nối mỗi biểu thức.
Toán tử mặc định cho cả hai phương thức là AND cái mà giống như phương thức có tiền tố and_.
Toán tử OR có thể được xác định bằng tiền tố các phương tức với or_.
Các điền kiện Where và Having có thể được lồng nhau hoặc nhóm lại bằng việc thêm hậu tố hoặc phương thức với _open và theo sau bởi một phương thức với một hậu tố _close.

    $query = DB::select()->from('users')
        ->where_open()
            ->or_where('id', 'IN', $expired)
            ->and_where_open()
                ->where('last_login', '<=', $last_month)
                ->or_where('last_login', 'IS', NULL)
            ->and_where_close()
        ->where_close()
        ->and_where('removed','IS', NULL);

Truy vấn này sẽ tạo ra SQL sau:

    SELECT * FROM `users` WHERE ( `id` IN (1, 2, 3, 5) OR ( `last_login` <= 1276020805 OR `last_login` IS NULL ) ) AND `removed` IS NULL 

### Biểu thức cơ sở dữ liệu

Có những trường hợp là bạn cần một biểu thức phức tạp hoặc các hàm cơ sở dữ liệu khác, mà bạn không muốn trình tạo truy vấn thử và thoát.
Trong các trường hợp này, bạn sẽ cần sử dụng một biểu thức cơ sở dữ liệu được tạo với [DB::expr].
**Một biểu thức cơ sở dữ liệu được lấy như đầu vào trực tiếp và không có việc thoát nào được thực hiện.**

    $query = DB::update('users')->set(array('login_count' => DB::expr('login_count + 1')))->where('id', '=', $id);

Lệnh này sẽ tạo truy vấn sau, giả sử $id = 45:

    UPDATE `users` SET `login_count` = `login_count` + 1 WHERE `id` = 45

Ví dụ khác để tính toán khoảng cách của hai điểm địa lý:

    $query = DB::select(array(DB::expr('degrees(acos(sin(radians('.$lat.')) * sin(radians(`latitude`)) + cos(radians('.$lat.')) * cos(radians(`latitude`)) * cos(radians(abs('.$lng.' - `longitude`))))) * 69.172'), 'distance'))->from('locations');

[!!] Bạn phải xác nhận và thoát bất kỳ đầu vào người dùng nào nhập vào DB::expr vì nó sẽ rõ ràng không thể thoát nó cho bạn.

## Thực thi

Một khi bạn đã làm xong việc xây dựng bạn có thể thực thi truy vấn bằng cách dùng `execute()` và sử dụng [các kết quả](results).

    $result = $query->execute();

Để sử dụng một [nhóm cấu hình](config) cơ sở dữ liệu khác, hãy truyền hoặc tên hoặc đối tượng cấu hình tới `execute()`.

	$result = $query->execute('config_name')