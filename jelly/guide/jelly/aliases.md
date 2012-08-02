# Hiểu bí danh và siêu bí danh (Meta-Aliases)

Jelly sử dụng bí danh mô hình và trường từ đầu đến cuối.
Thông thường, khi đặt tên một trường trong mô hình của bạn cột trong cơ sở dữ liệu có cùng tên, nhưng Jelly cho phép hầu hết các trường tham chiếu một cột với một tên khác.
Điều này có nghĩa rằng bạn có thể tách riêng một cách hiệu quả lược đồ cơ sở dữ liệu và mô hình của bạn.

Jelly còn có một hệ thống siêu bí danh mà cho phép bạn tham chiếu tới các trường cụ thể mà chung cho tất cả mô hình, chẳng hạn như khoá chính của nó.

### Bí danh

Khi định nghĩa các trường của bạn, bạn có thể chỉ định một cột mà trường này đại diện:

    class Model_Post extends Jelly_Model
    {
        public static function initialize($meta)
        {
            $meta->fields(array(
                'id' => new Field_Primary(array(
                    'column' => 'PostId')),
            ));
        }
    }

Bất cứ nơi nào bạn tham chiếu trường 'id', nó sẽ được ánh xạ tới cột 'PostId'. Ví dụ:

    $post->where('id', 'IN', array(1, 2, 3));
    
    // Câu lệnh sau sẽ làm việc, nhưng cột chặt logic của bạn tới
    // lược đồ cơ sở dữ liệu của bạn dó đó nó khó chịu
    $post->where('PostId', 'IN', array(1, 2, 3));
    
Bất kỳ nơi nào bạn đang tham chiếu tới một mô hình hoặc một trường, bạn nên sử dụng tên của một mô hình hoặc một trường, *không* là tên của một bảng hoặc một cột.

### Siêu bí danh (Meta-Aliases)

Siêu bí danh là một lối tắt cú pháp cho việc tham chiếu tới một trường cụ thể trong một mô hình. Hiện có bốn siêu bí danh đã định nghĩa:

  * **:primary_key** - tham chiếu tới khoá chính của mô hình
  * **:name_key** - tham chiếu tới khoá tên của mô hình
  * **:unique_key** - tham chiếu tới khoá duy nhất của mô hình
  * **:foreign_key** - tham chiếu tới khoá ngoại của mô hình
  
##### Ví dụ - Sử dụng một siêu bí danh

    $post->where(':primary_key', '=', $value);
    $post->where(':name_key', '=', $value);
    
    // Trong trường hợp này, giá trị được truyền tới phương thức unique_key() trong
    // lớp trình xây dựng của bạn, cái mà trả về trường thích hợp để sử dụng
    // dựa trên giá trị
    $post->where(':unique_key', '=', $value);
    
##### Ví dụ - Sử dụng siêu bí danh :foreign_key

Thông thường, bạn muốn có thể tham chiếu tới khoá ngoại của mô hình khác, do đó có một cú pháp đặc biệt để làm một điều như vậy.

    // Giả sử một bài viết belongs_to một tác giả
    $post->where('post.author:foreign_key', '=', $value);
    
    // Điều này cũng có thể, mặc dù không thực sự hữu ích hay thiết thực
    $post->where('author:primary_key', '=', $value);
    
Trong trường hợp này, bạn chỉ định một mô hình trước khi siêu bí danh kéo nó từ mô hình.

[!!] **Lưu ý**: Siêu bí danh chỉ có thể được sử dụng trong trình dựng truy vấn.

## Thay đổi siêu bí danh của bạn

primary\_key, name\_key, và foreign\_key của mô hình của bạn tất cả được định nghĩa trong phương thức initialize() của bạn.
Thông tin thêm có thể được tìm thấy trong [Tài liệu API cho Jelly_Meta](../api/Jelly_Meta).

unique\_key là một trường hợp đặc biệt từ khi một giá trị được truyền tới nó để nó có thể xác định các trường thích hợp để sử dụng.
Để thay đổi hành vi của nó bạn phải [tạo mô hình Jelly\_Builder đặc trưng của bạn](extending-builder).
