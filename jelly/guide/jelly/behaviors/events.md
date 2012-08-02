# Sự kiện dựng sẵn

Các phương thức sau đây có thể được sử dụng để làm các hành động trên các sự kiện cụ thể.

## Sự kiện cho mô hình

#### Xác nhận hợp lệ

[!!] Các phương thức này nhận **đối tượng mô hình** và **đối tượng xác nhận hợp lệ** trong các tham số của chúng.

[!!] Giá trị trả về của sự kiện không ảnh hưởng đến hoạt động của Jelly.

- `model_before_validate($params, $event_data)`
- `model_after_validate($params, $event_data)`

#### Lưu dữ liệu

[!!] Các phương thức này nhận **đối tượng mô hình** trong tham số của chúng.

[!!] Nếu giá trị trả về của sự kiện `model_before_save()` là `FALSE`, thì việc lưu dữ liệu được bỏ qua.

- `model_before_save($params, $event_data)`
- `model_after_save($params, $event_data)`

#### Xóa dữ liệu

[!!] Nếu giá trị trả về của sự kiện `model_before_delete()` không là `NULL`, thì việc xóa bỏ được bỏ qua, nhưng `model_after_delete()` vẫn chạy.

- `model_before_delete($params, $event_data)`
- `model_after_delete($params, $event_data)`

***

## Sự kiện cho trình xây dựng

[!!] Các phương thức này nhận **đối tượng trình xây dựng** trong tham số của chúng.

#### Chọn lựa

[!!] Giá trị trả về của sự kiện không có ảnh hưởng đến hoạt động của Jelly.

- `builder_before_select($params, $event_data)`
- `builder_after_select($params, $event_data)`

#### Chèn dữ liệu

[!!] Giá trị trả về của sự kiện không có ảnh hưởng đến hoạt động của Jelly.

- `builder_before_insert($params, $event_data)`
- `builder_after_insert($params, $event_data)`

#### Cập nhật dữ liệu

[!!] Giá trị trả về của sự kiện không có ảnh hưởng đến hoạt động của Jelly.

- `builder_before_update($params, $event_data)`
- `builder_after_update($params, $event_data)`

#### Xóa dữ liệu

[!!] Nếu giá trị trả về của sự kiện `builder_before_delete()` không là `NULL`, thì việc xóa bỏ được bỏ qua, nhưng `builder_after_delete()` vẫn chạy.

- `builder_before_delete($params, $event_data)`
- `builder_after_delete($params, $event_data)`

***

## Sự kiện cho meta

[!!] Các phương thức này nhận **đối tượng meta** trong tham số của chúng.

[!!] Giá trị trả về của sự kiện không có ảnh hưởng đến hoạt động của Jelly.

- `meta_before_finalize($params, $event_data)`
- `meta_after_finalize($params, $event_data)`