# Danh sách các phương thức tiện lợi

Bởi vì Formo sử dung `__get()` cho việc lấy các trường thay cho các biến,
các phương thức tiên lợi sau đây cung cập việc truy cập trực tiếp đến các biến protected cụ thể.

Tên phương thức								|	Hành động
--------------------------------------------|------------------------------------------------
`fields()`									|	Trả về tất cả các trường trong một trình container
`val([$value])`								|	Trả về hoặc thiết lập giá trị
`error([$msg])`								|	Trả về hoặc thiết lập thông báo lỗi
`errors([$file], [$translate = TRUE])`		|	Trả về hoặc thiết lập thông báo lỗi nhóm
`alias([$alias])`							|	Trả về hoặc thiết lập một bí danh trường