# Câu lệnh đã chuẩn bị

Việc sử dụng các câu lệnh đã chuẩn bị cho phép bạn viết các truy vấn SQL bằng tay trong khi vẫn thoát các giá trị truy vấn một cách tự động để ngăn chặn [tiêm SQL](http://wikipedia.org/wiki/SQL_Injection).
Việc tạo một truy vấn là đơn giản:

    $query = DB::query(Database::SELECT, 'SELECT * FROM users WHERE username = :user');

Phương thức [DB::query] chỉ là một lối tắt để tạo một lớp [Database_Query] mới cho chúng ta, để cho phép gọi móc nối phương thức.
Truy vấn chứa một tham số `:user`, cái mà chúng ta sẽ nhận để trong một giây.

Tham số đầu tiên của [DB::query] là kiểu truy vấn.
Nó sẽ là `Database::SELECT`, `Database::INSERT`, `Database::UPDATE`, hoặc `Database::DELETE`.
Việc này được thực hiện vì lý do tương thích cho các trình điều khiển, và để xác định dễ dàng những gì `execute()` sẽ trả về.

Tham số thứ hai là bản thân truy vấn.
Thay vì cố gắng để ghép truy vấn của bạn và các biến với nhau, bạn nên sử dụng [Database_Query::param].
Việc này sẽ làm cho các truy vấn của bạn dễ dàng hơn để bảo trì, và sẽ thoát các giá trị đẻ nằng ngừa [tiêm SQL](http://wikipedia.org/wiki/SQL_Injection).

## Tham số

Truy vấn ví dụ của chúng ta trước đó chứa một tham số `:user`, cái mà chúng ta có thể gán tới một giá trị bằng cách sử dụng [Database_Query::param] như sau:

    $query->param(':user', 'john');

[!!] Các tên tham số có thể là bất kỳ chuỗi duy nhất nào, ví chúng được thay thế bằng cách sử dụng [strtr](http://php.net/strtr). Nó rất được khuyến khích để **không** sử dụng dấu dollar như các tên tham số để ngắn ngừa nhầm lẫn. Dấu hai chấm thường được sử dụng.

Bạn cũng có thể cập nhật tham số `:user` bằng các gọi [Database_Query::param] một lần nữa:

    $query->param(':user', $_GET['search']);

Nếu bạn muốn thiết lập nhiều tham số cùng một lúc, bạn có thể sử dụng [Database_Query::parameters].
	
	$query = DB::query(Database::SELECT, 'SELECT * FROM users WHERE username = :user AND status = :status');

	$query->parameters(array(
		':user' => 'john',
		':status' => 'active',
	));

Nó cũng có thể ràng buộc một tham số tới một biến, sử dụng một [tham chiếu biến](http://php.net/language.references.whatdo).
Việc này có thể rất hữu ích khi đang chạy cùng câu truy vấn nhiều lần:

    $query = DB::query(Database::INSERT, 'INSERT INTO users (username, password) VALUES (:user, :pass)')
        ->bind(':user', $username)
        ->bind(':pass', $password);

    foreach ($new_users as $username => $password)
    {
        $query->execute();
    }

Trong ví dụ ở trên, các biến `$username` và `$password` được thay đổi cho mỗi vòng lặp của câu lệnh `foreach`.
Khi tham số thay đổi, nó thay đổi hiệu quả các tham số truy vấn `:user` và `:pass`.
Hãy cẩn thận việc ràng buộc tham số có thể tiết kiệm nhiều mã khi nó được sử dụng đúng cách.

Sự khác biệt duy nhất giữa `param()` và `bind()` là `bind()` truyền các biến theo tham chiếu hơn là gán (sao chép), do đó các thay đổi trong tương lai tới biến có thể được "thấy" bởi truy vấn.

[!!] Mặc dù tất cả các tham số được thoát để ngăn chặn tiêm SQL, nó vấn là một ý kiến tốt để xác nhận/làm vệ sinh đầu vào của bạn.

## Hiển thị truy vấn thô

Nếu bạn muốn hiển thị SQL mà sẽ được thực thi, chỉ cần ép kiểm đối tượng tới một chuỗi.

    echo Kohana::debug((string) $query);
    // Should display:
    // SELECT * FROM users WHERE username = 'john'

## Thực thi

Một khi bạn đã được gán cái gì đó tới mỗi tham số, bạn có thể thực thi truy vấn bằng cách sử dụng `execute()` và sử dụng [các kết quả](results).

    $result = $query->execute();

Để sử dụng [nhóm cấu hình](config) cơ sở dữ liệu khác nhau hãy truyền tên hoặc đối tượng cấu hình tới `execute()`.

	$result = $query->execute('config_name')