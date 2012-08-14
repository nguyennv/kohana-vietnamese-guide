# Khung nhìn HTML Formo

Tại thời điểm kết xuất, mọi form và trường được truyền vào trong các tập tin khung nhìn cho việc kết xuất.
Điều này cho phép bạn bọc các trường trong các thẻ HTML, hoặc thêm bất cứ styling cần thiết cho các trường, form và subform.

Formo làm cho phần kết xuất của bạn cực kỳ linh hoạt và cho phép bạn trộn bất cứ hỗn hợp của phương pháp tiếp cận Hướng đối tượng và văn bản thuần mà bạn thích trong việc định nghĩa các trường của bạn.

Các đối tượng form, subform và trường của khung nhì có thể truy cập tới các khung nhì như biến `$this`.

Các tập tin khung nhìn dựa trên một hệ thống tiền tố để tạo ra các form cụ thể dễ dàng hơn.

	$form->set('view_prefix', '/specialform')->render();

## Các form, trường như các đối tượng DOM

Khi kết xuất một form như HTML, các đối tượng form/subform và trường được truyền tới các khung nhìn trong một trạng thái có thể sử dụng như là các đối tượng `Formo_View_HTML`.
Hãy nghĩ rằng các đối tượng này như **các đối tượng HTML DOM**

Các đối tượng này được thao tác giống như các đối tượng jQuery DOM và sủ dụng cú pháp tương tự.

[!!] Lưu ý rằng tập tin khung nhìn có thể truy cập đối tượng khung nhìn của nó với biến ngữ cảnh `$this`
