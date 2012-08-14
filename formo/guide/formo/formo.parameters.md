# Các tham số trường hữu ích

Sau đây là một danh sách các tham số mà bạn sẽ thường xử lý với.

Tham số			|	Kiểu	|	Chức năng
----------------|-----------|------------
`editable`		|	Bool	|	Khi `FALSE`, giá trị trường hiển thị như text thay vì hộp nhập. Khi `TRUE` nó sẽ được hiện thị như một trường.
`render`		|	Bool	|	Nếu thiết lập thành `FALSE`, Formo sẽ không kết xuất hoặc xác nhận hợp lệ trường.
`ignore`		|	Bool	|	Nếu thiết lập thành `TRUE`, Formo sẽ bỏ qua các quy tắc xác nhận hợp lệ của trường.
`attr`			|	Array	|	Mang vào các form/trường đã kết xuất như là HTML. Đây là cặp `key => value` của thẻ thuộc tính HTML.
`css`			|	Array	|	Mang vào các form/trường đã kết xuất như là HTML. Đây là cặp `key => value` của thẻ thuộc tính `style`.
`label`			|	String	|	Trở thành nhãn của trường. Nếu không chỉ định, bí danh là nhãn của nó.
`driver`		|	String	|	Trình điều khiển mà xử lý kiểu trường/form.
`options`		|	Array	|	Trở thành tùy chọn có sẵn cho hộp chọn, nhóm nút radio và nhóm nút chọn.
`order`			|	Mixed	|	Xác định nơi mà trường được đặt có liên quan đến trường khác.

## Thiết lập các tham số cụ thể

### Giá trị

Để thiết lập/nhận một **giá trị** của trường, hãy sử dụng `val()`:

	// Thiết lập giá trị tới "my_username"
	$form->username->val('my_username');
	
	// Trả về "my_username"
	$form->username->val();
	
### Bí danh

	// Thiết lập bí danh tới "special_form"
	$form->alias('special_form');
	
	// Trả về "special_form"
	$form->alias();