# Nâng cấp

## Bí danh bảng

ORM [bây giờ sẽ đặt bí danh cho bảng chính](http://dev.kohanaframework.org/issues/4066 trong một truy vấn tới tên đối tượng số ít của của các mô hình.
v.d. Trước 3.2 ORM thiết lập từ bảng dữ liệu như vậy:

	$this->_db_builder->from($this->_table_name);

Và tới 3.2 nó đặt định danh giống như thế này:

	$this->_db_builder->from(array($this->_table_name, $this->_object_name));

Nếu bạn có một mô hình `Model_Order` thì khi xây dựng một truy vấn sử dụng bí danh giống như:

	$model->where('order.id', '=', $id);
