Giá trị
=======

Formo gán và theo dõi một giá trị cho mỗi trường.
Nó là giá trị mà được xác nhận hợp lệ đối với trường.
Đây cũng là giá trị bạn sẽ truyền cùng với các đối tượng ORM của bạn.

Mặc dù chúng sẽ bắt chước nhau, không nhầm lẫn giá trị của trường với một thuộc tính `value` của các thẻ input HTML.
Chúng là các thứ khác nhau, và thuộc tính thực sự được thiết lập bằng tay bởi một trình điều khiển trương khi kết xuất một trường.

Một giá trị trường có thể là một chuỗi hoặc mộ mảng tùy thuộc vào loại trường.
Ví dụ, các trường text sử dụng các giá trị chuỗi, và một nhóm các nút chọn sẽ phụ thuộc vào một mảng các giá trị.

### Thiết lập và lấy giá trị

**Không truy cập trược tiếp một tham số `value` của trường.
bạn phải luôn luôn sử dụng phương thức `val()` để thiết lập và nhận giá trị, hoặc bạn sẽ kết thúc với kết quả bất ngờ.**

Đê thiết lập một giá trị của trường, hãy dùng `val($value)`:

	$form->email->val('john@doe.com');

Để lấy một giá trị của trường, hãy dùng `val()`:

	$posted_email = $form->email->val();
