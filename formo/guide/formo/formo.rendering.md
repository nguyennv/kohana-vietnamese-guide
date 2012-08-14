Kết xuất các form và trường
===========================

Formo có thể kết xuất các trường và form bằng hoặc *kết xuất* chúng thông qua các tập tin khung nhìn đã xác định trước hoặc với phương thức `html()` trên đối tượng chính nó.

### Kết xuất

Mỗi form và trường có một trình điều khiển mà định nghĩa như một tập tin khung nhìn liên kết với nó.
Khi một trường được kết xuất, đối tường trường được truyền vào tập tin khung nhìn đã định nghĩa.

Một form hoặc trường *đã kết xuất* trả về một đối tượng View của trường đó.

Một tập tin khung nhìn có thể *kết xuất* một phần cụ thể của một form hoặc một trường bên trong nó.

Phương thức `render([$file])` cho phép hay tham số tùy chọn để chỉ định một tập tin khung nhìn.

### HTML

Đối tượng khung nhìn form xác định làm thế nào một form cụ thể hoặc đối tượng thực sự trở trành html.

Một ví dụ của việc này là kết xuất một phần tử trường HTML.
Các phần tử HTML tất cả đều có

Tìm khiểu thêm về các loại trong [mục phân loại](formo.html_kind)
