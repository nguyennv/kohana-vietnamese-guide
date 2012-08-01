# Xác nhận hợp lệ

*Trang này cần phải được xem xét về độ chính xác bởi nhóm phát triển. Các ví dụ tốt hơn sẽ hữu ích.*

Việc xác nhận hợp lệ có thể được thực hiện trên bất kỳ mảng nào bằng cách sử dụng lớp [Validation].
Các nhãn và các quy tắc có thể được đính kèm tới một đối tượng Validate bằng khoá của mảng, gọi là một "tên trường".

nhãn
:  Một nhãn là một là một phiên bản người có thể đọc của tên trường.

quy tắc
:  Một quy tắc là một lời gọi lại - callback được sử dụng để quyết định có hoặc không để thêm một lỗi tới một trường.

[!!] Lưu ý rằng bất kỳ [PHP callback](http://php.net/manual/language.pseudo-types.php#language.types.callback) hợp lệ nào có thể được sử dụng như một quy tắc.

Việc sử dụng `TRUE` như tên trường khi thêm một quy tắc sẽ áp dụng tới tất cả trường có tên.

Việc tạo một đối tượng xác nhận hợp lệ được thực hiện bằng cách sử dụng phương thức [Validation::factory]:

    $post = Validation::factory($_POST);

[!!] Đối tượng `$post` sẽ được sử dụng cho phần còn lại của hướng dẫn này. Hướng dẫn này sẽ hiển thị cho bạn cách xác nhận hợp lệ việc đăng ký một người dùng mới.

## Các quy tắc đã cung cấp

Kohana cung cấp tập các tập quy tắc hữu ích cho lớp [Valid]:

Tên quy tắc               | Chức năng
------------------------- |-------------------------------------------------
[Valid::not_empty]     | Giá trị phải là một giá trị khác rỗng
[Valid::regex]         | Khớp giá trị đối với một biểu thức chính quy
[Valid::min_length]    | Số lượng các ký tự nhỏ nhất cho giá trị
[Valid::max_length]    | Số lượng các ký tự lớn nhất cho giá trị
[Valid::exact_length]  | Giá trị phải có một số lượng chính xác các ký tự
[Valid::email]         | Một địa chỉ email là bắt buộc
[Valid::email_domain]  | Kiểm tra miền của email tồn tại
[Valid::url]           | Giá trị phải là một URL
[Valid::ip]            | Giá trị phải là một địa chỉ IP
[Valid::phone]         | Giá trị phải là một số điện thoại
[Valid::credit_card]   | Yêu cầu một số thẻ tín dụng
[Valid::date]          | Giá trị phải là một ngày (và thời gian)
[Valid::alpha]         | Chỉ có ký tự alpha được phép
[Valid::alpha_dash]    | Chỉ ký tự alpha và dấu gạch nối được phép
[Valid::alpha_numeric] | Chỉ ký tự alpha và chữ số được phép
[Valid::digit]         | Giá trị phải là một số nguyên
[Valid::decimal]       | Giá trị phải là một giá trị thập phân hoặc dấu phẩy động
[Valid::numeric]       | Chỉ ký tự số được phép
[Valid::range]         | Giá trị phải nằm trong một phạm vi
[Valid::color]         | Giá trị phải là một màu HEX hợp lệ
[Valid::matches]       | Giá trị khớp với giá trị của trường khác

## Thêm các quy tắc

Tất cả các quy tắc xác nhân hợp lệ được định nghĩa như một tên trường, một phương thức hoặc một hàm (sử dụng cú pháp [PHP callback](http://php.net/callback)), và một mảng các tham số:

    $object->rule($field, $callback, array($parameter1, $parameter2));

Nếu không có tham số nào được chỉ định, giá trị của trường sẽ được truyền vào callback. Hai quy tắc sau đây là tương đương:

    $object->rule($field, 'not_empty');
    $object->rule($field, 'not_empty', array(':value'));

Các quy tặc được định nghĩa trong lớp [Valid] có thể được thêm bằng cách sử dụng tên phương thức một mình.
Ba quy tắc sau đây là tương đương:

    $object->rule('number', 'phone');
    $object->rule('number', array('Valid', 'phone'));
    $object->rule('number', 'Valid::phone');

## Ràng buộc các biến

Lớp [Validation] cho phép bạn ràng buộc các biến với các chuỗi nhất định để chúng có thể được sử dụng khi định nghĩa các quy tắc.
Các biến được ràng buộc bằng cách gọi phương thức [Validation::bind].

    $object->bind(':model', $user_model);
    // Mã tương lai sẽ có thể sử dụng :model để tham chiếu tới đối tượng
    $object->rule('username', 'some_rule', array(':model'));

Theo mặc định, đối tượng xác nhận hợp lệ sẽ tự động rằng buộc các giá trị sau cho bạn để sử dụng như các tham số quy tắc:

- `:validation` - tham chiếu tới đối tượng xác nhận hợp lệ
- `:field` - tham chiếu tới tên trường mà quy tắc dành cho
- `:value` - tham chiếu giá trị của trường mà quy tắc dành cho

## Thêm lỗi

Lớp [Validation] sẽ thêm một lỗi đối với một trường nếu bất kỳ quy tắc nào liên kết tới nó trả về `FALSE`.
Điều này cho phép nhiều hàm PHP dựng sẵn được sử dụng như các quy tắc, chẳng hạn hàm `in_array`.

    $object->rule('color', 'in_array', array(':value', array('red', 'green', 'blue')));

Các quy tắc đã thêm vào các trường trống sẽ chạy, nhưng việc trả về `FALSE` sẽ không tự động thêm một lỗi cho trường.
Để cho một quy tắc ảnh hưởng đên các trường trống, bạn phải thêm lỗi bằng tay bằng cách gọi phương thức [Validation::error].
Để làm được việc này , bạn phải truyền đối tượng xác nhận hợp lệ tới quy tắc.

    $object->rule($field, 'the_rule', array(':validation', ':field'));
    
    public function the_rule($validation, $field)
    {
        if (something went wrong)
        {
            $validation->error($field, 'the_rule');
        }
    }

[!!] `not_empty` và `matches` chỉ là các quy tắc mà sẽ chạy trên các trường trống và thêm các lỗi bằng cách trả về `FALSE`.

## Ví dụ

Để bắt đầu ví dụ của chúng ta, chúng ta sẽ thực hiện việc xác nhận hợp lệ trên một mảng `$_POST` mà chứa thông tin đăng ký người dùng:

    $post = Validation::factory($_POST);

Tiếp theo chúng ta cần xử lý thông tin của POST bằng cách sử dụng [Validation]. Để bắt đầu, chúng ta cần thêm vài quy tắc:

    $post
        ->rule('username', 'not_empty')
        ->rule('username', 'regex', array(':value', '/^[a-z_.]++$/iD'))
        ->rule('password', 'not_empty')
        ->rule('password', 'min_length', array(':value', '6'))
        ->rule('confirm',  'matches', array(':validation', 'confirm', 'password'))
        ->rule('use_ssl', 'not_empty');

Bất kỳ hàm PHP hiện có nào cũng có thể được sử dụng như một quy tắc.
Ví dụ, nếu chúng ta muốn kiểm tra nếu người dùng đã nhập một giá trị thích hợp cho câu hỏi SSL:

    $post->rule('use_ssl', 'in_array', array(':value', array('yes', 'no')));

Lưu ý rằng tất cả tham số mảng vẫn được bao bọc trong một mảng!
Nếu không bao bọc mảng, `in_array` sẽ được gọi là `in_array($value, 'yes', 'no')`, mà sẽ cho kết quả trong một lỗi PHP.

Bất kỳ quy tắc tuỳ biến nào có thể được thêm bằng các sử dụng một [PHP callback](http://php.net/manual/language.pseudo-types.php#language.types.callback]:

    $post->rule('username', 'User_Model::unique_username');

Phương thức `User_Model::unique_username()` sẽ được định nghĩa tương tự:

    public static function unique_username($username)
    {
        // Kiểm tra nếu username đã tồn tại trong cơ sở dữ liệu
        return ! DB::select(array(DB::expr('COUNT(username)'), 'total'))
            ->from('users')
            ->where('username', '=', $username)
            ->execute()
            ->get('total');
    }

[!!] Các quy tắc tuỳ biến cho phép nhiều kiểm tra bổ xung để được tái sử dụng cho nhiều mục đích. Các phương thức này sẽ hầu như luôn tồn tại trong một mô hình, nhưng có thể được định nghĩa trong lớp bất kỳ.

# Một ví dụ hoàn chỉnh

Trước tiên, chúng ta cần một [View] mà chứa mẫu biểu HTML, cái mà sẽ được đặt trong `application/views/user/register.php`:

    <?php echo Form::open() ?>
    <?php if ($errors): ?>
    <p class="message">Một số lỗi đã được bắt gặp, vui lòng kiểm tra các chi tiết bạn nhập vào.</p>
    <ul class="errors">
    <?php foreach ($errors as $message): ?>
        <li><?php echo $message ?></li>
    <?php endforeach ?>
    <?php endif ?>

    <dl>
        dt><?php echo Form::label('username', 'Tên truy cập') ?></dt>
        <dd><?php echo Form::input('username', $post['username']) ?></dd>

        <dt><?php echo Form::label('password', 'Mật khẩu') ?></dt>
        <dd><?php echo Form::password('password') ?></dd>
        <dd class="help">Mật khẩu phải có ít nhất 6 ký tự.</dd>
        <dt><?php echo Form::label('confirm', 'Confirm Password') ?></dt>
        <dd><?php echo Form::password('confirm') ?></dd>

        <dt><?php echo Form::label('use_ssl', 'Sử dụng bảo mật nâng cao?') ?></dt>
        <dd><?php echo Form::select('use_ssl', array('yes' => 'Luôn luôn', 'no' => 'Chỉ khi cần thiết'), $post['use_ssl']) ?></dd>
        <dd class="help">Để bảo mật, SSL luôn được sử dụng khi thực hiện thanh toán.</dd>
    </dl>

    <?php echo Form::submit(NULL, 'Đăng ký') ?>
    <?php echo Form::close() ?>

[!!] Ví dụng này sử dụng trình trợ giúp [Form] một cách rộng rãi. Việc sử dụng [Form] thay vì viết HTML để đảm bảo rằng tất cả đầu vào của mẫu biểu sẽ xử lý đúng đầu vào mà bao gồm các ký tự HTML. Nếu bạn tích viết HTML của chính bạn hơn, hãy chắc chắn sử dụng [HTML::chars] để thoát đầu vào của người dùng.

Tiếp theo, chúng ta cần một điều khiển và hành động để xử lý việc đăng ký, cái mà sẽ được đặt trong `application/classes/controller/user.php`:

    class Controller_User extends Controller {

        public function action_register()
        {
            $user = Model::factory('user');

            $post = Validation::factory($_POST)
                ->rule('username', 'not_empty')
                ->rule('username', 'regex', array(':value', '/^[a-z_.]++$/iD'))
                ->rule('username', array($user, 'unique_username'))

                ->rule('password', 'not_empty')
                ->rule('password', 'min_length', array(':value', 6))
                ->rule('confirm',  'matches', array(':validation', ':field', 'password'))

                ->rule('use_ssl', 'not_empty')
                ->rule('use_ssl', 'in_array', array(':value', array('yes', 'no')));

            if ($post->check())
            {
                // Dữ liệu đã được xác nhận, đăng ký người dùng
                $user->register($post);

                // Luôn luôn điều hướng dau khi một POST thành công để ngăn chặn cảnh báo làm mới.
                $this->request->redirect('user/profile');
            }

            // Xác nhận thất bại, thu thập các lỗi
            $errors = $post->errors('user');

            // Hiển thị mẫu biểu đăng ký
            $this->response->body(View::factory('user/register'))
                ->bind('post', $post)
                ->bind('errors', $errors);
        }

    }

Chúng ta cũng sẽ cần một mô hình người dùng, cái mà sẽ được đặt trong `application/classes/model/user.php`:

    class Model_User extends Model {

        public function register($array)
        {
            // Tạo một bản ghi người dùng mới trong cơ sở dữ liệu
            $id = DB::insert(array_keys($array))
                ->values($array)
                ->execute();

            // Lưu id người dùng mới tới một cookie
            cookie::set('user', $id);

            return $id;
        }

    }

Đó là, chúng ta có một ví dụ đăng ký người dùng hoàn chỉnh mà kiểm tra đúng đầu vào người dùng!
