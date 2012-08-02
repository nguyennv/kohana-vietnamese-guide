# Cú pháp Markdown

Mô-đun userguide sử dúng [Markdown](http://daringfireball.net/projects/markdown/) và [Markdown Extra](http://michelf.com/projects/php-markdown/extra/) cho các trang userguide, và các chú thích trong mã được sử dụng để sinh bộ duyệt API. Đây là một bản tóm tắt ngắn gọn hầu hết các tính năng của Markdown và tính năng mở rộng của Markdown. Nó không bao gồm mọi thứ, và nó không bao gồm tất cả sự báo trước.

[!!] Hãy chắc chắn để kiểm tra **[Cú pháp Userguide cụ thể](#userguide-specific-syntax)** cho các thứ mà Userguide thêm tới markdown.

## Headers

    # Header 1
	
	## Header 2
	
	### Header 3
	
	#### Header 4

## Đoạn văn
~~~
Văn bản thông thường sẽ được chuyển đổi thành các đoạn văn.
Trả lại đơn lẻ sẽ không tạo một đoạn văn mới, điều này
cho phép việc bao gói (đặc biết cho các chú thích
trong mã).

Một đoạn văn mới sẽ bắt đầu nếu có một dòng trống (blank) giữa
các khối văn bản. Các ký tự như > và & được giải thoát (escape) cho bạn.

Để làm một ngắt dòng,  
đặt hai khoảng trắng vào  
cuối một dòng.
~~~
Văn bản thông thường sẽ được chuyển đổi thành các đoạn văn.
Trả lại đơn lẻ sẽ không tạo một đoạn văn mới, điều này
cho phép việc bao gói (đặc biết cho các chú thích
trong mã).

Một đoạn văn mới sẽ bắt đầu nếu có một dòng trống (blank) giữa
các khối văn bản. Các ký tự như > và & được giải thoát (escape) cho bạn.

Để làm một ngắt dòng,  
đặt hai khoảng trắng vào  
cuối một dòng.

## Liên kết
~~~
Đây là một liên kết bình thường: [Kohana](http://kohanaframework.org).

Liên kết này có tiêu đề: [Kohana](http://kohanaframework.org "Khung làm việc PHP nhanh")
~~~
Đây là một liên kết bình thường: [Kohana](http://kohanaframework.org).

Liên kết này có tiêu đề: [Kohana](http://kohanaframework.org "Khung làm việc PHP nhanh")

## Các khối mã

	Đối với mã nội dòng chỉ đơn giản bao quanh một vài `văn bản với các dấu kiểm (tick).`
	
Đối với mã nội dòng chỉ đơn giản bao quanh một vài `văn bản với các dấu kiểm (tick).`

	// Đối với một khối mã,
	// thụt dòng trong bốn khoảng trắng,
	// hoặc với một ký tự tab

Bạn cũng có thể làm một khối mã "được rào-fenced"

	~~~
	Một khối mã được rào có các dẫu ngã
	          ở trên và ở dưới nó
	Điều này đôi khi hữu ích khi mã gần danh sách
	~~~
~~~
Một khối mã được rào có các dẫu ngã
		  ở trên và ở dưới nó
Điều này đôi khi hữu ích khi mã gần danh sách
~~~

## Danh sách không thứ tự

~~~
*  Để làm một danh sách không thứ tự, đặt một dấu hoa thị, dấu trừ, hoặc dấu + tài đầu
-  mỗi dòng, bao quanh bởi các khoảng trắng. Bạn có thể kết hoăpj * - và +, nhưng nó
+  làm cho không có sự khác biệt.
~~~
*  Để làm một danh sách không thứ tự, đặt một dấu hoa thị, dấu trừ, hoặc dấu + tài đầu
-  mỗi dòng, bao quanh bởi các khoảng trắng. Bạn có thể kết hoăpj * - và +, nhưng nó
+  làm cho không có sự khác biệt.

## Danh sách thứ tự

~~~
1.  Đối với các danh sách thứ tự, đặt một số và một giai đoạn
2.  Trên mỗi dòng mà bạn muốn đánh số.
9.  Nó không thực sự có được thứ tự số đúng
5.  Chỉ cần miễn là mỗi dòng có một số
~~~
1.  Đối với các danh sách thứ tự, đặt một số và một giai đoạn
2.  Trên mỗi dòng mà bạn muốn đánh số.
9.  Nó không thực sự có được thứ tự số đúng
5.  Chỉ cần miễn là mỗi dòng có một số

## Danh sách lồng nhau

~~~
*  Để lồng danh sách bạn chỉ cần thêm bốn khoảng trắng trước dấu * hoặc số
	1. Giống thế này
		*  Nó khá cơ bản, dòng này có tắm khoảng trắng, do đó nó được lồng hai lần
	1. Và dòng này được trở lại tới mức thứ hai
		*  Trở ra mức thứ ba lần nữa
*  Và trở lại mức đầu tiên
~~~
*  Để lồng danh sách bạn chỉ cần thêm bốn khoảng trắng trước dấu * hoặc số
	1. Giống thế này
		*  Nó khá cơ bản, dòng này có tắm khoảng trắng, do đó nó được lồng hai lần
	1. Và dòng này được trở lại tới mức thứ hai
		*  Trở ra mức thứ ba lần nữa
*  Và trở lại mức đầu tiên

## In nghiêng và đậm

~~~
Bao quanh văn bản mà bạn muốn *in nghiêng* với *dấu hoa thị* hoặc _dấu gạch dưới_.

**Gấp hai hoa thị** hoặc __Gấp hai gạch dưới__ để làm văn bản in đậm.

***Gấp ba*** sẽ làm *cả hai tại cùng **thời gian***.
~~~
Bao quanh văn bản mà bạn muốn *in nghiêng* với *dấu hoa thị* hoặc _dấu gạch dưới_.

**Gấp hai hoa thị** hoặc __Gấp hai gạch dưới__ để làm văn bản in đậm.

___Gấp ba___ sẽ làm *cả hai tại cùng **thời gian***.

## Thước ngang

Thước ngang được làm bằng việc thay thế 3 hoặc nhiều dấu gạch ngang, dấu hoa thị, hoặc dấu gạch dưới trên một dòng của chính chúng.
~~~
---
* * * *
_____________________
~~~
---
* * * *
_____________________

## Hình ảnh

Cú pháp hình ảnh trong giống như thế này:

	![Văn bản thay thế](/path/to/img.jpg)
	
	![Văn bản thay thế](/path/to/img.jpg "Tiêu đề tuỳ chọn")

[!!] Lưu ý hình ảnh trong userguide là [không gian tên](#namespacing).

## Bảng
~~~
Tiêu đề đầu tiên  | Tiêu đề thứ hai
----------------- | ----------------
Ô nội dung        | Ô nội dung
Ô nội dung        | Ô nội dung
~~~

Tiêu đề đầu tiên  | Tiêu đề thứ hai
----------------- | ----------------
Ô nội dung        | Ô nội dung
Ô nội dung        | Ô nội dung

Lưu ý rằng các đường bên mặt trái và mặt phải là tuỳ chọn, và bạn có thể thay đổi canh lên văn bản bằng việc thêm một dấu hai chấm ở bên phải, hoặc ở cả hai mặt cho canh lên giữa.
~~~
| Mục       | Giá trị | Tiết kiệm |
| --------- | -------:|:---------:|
| Computer  | $1600   |    40%    |
| Phone     |   $12   |    30%    |
| Pipe      |    $1   |     0%    |
~~~
| Mục       | Giá trị | Tiết kiệm |
| --------- | -------:|:---------:|
| Computer  | $1600   |    40%    |
| Phone     |   $12   |    30%    |
| Pipe      |    $1   |     0%    |

# Cú pháp riêng của userguide 

Ngoài các đặc tính và cú pháp của [Markdown](http://daringfireball.net/projects/markdown/) và [Markdown Extra](http://michelf.com/projects/php-markdown/extra/) những thứ sau áp dụng tới các trang userguide và tài liệu api:

## Namespacing

Điều đầu tiên cần lưu ý là tất cả các liên kết là "không gian tên" tới mô-đun hiện thời. Ví dụ. từ bất kỳ nơi nào bên trong tài liệu lõi Kohana bạn không cần phải bao gồm `kohana` tại phần đầu của một liên kết url. Ví dụ: `[Hướng dẫn Hello World](tutorials/hello-world)` hơn là `(kohana/tutorials/hello-world)`.

Để liên kết tới một trang chỉ mục của các mô-đun, có một url trống giống như: `[Kohana]()`.

Để liên kết tới trang trong mô-đun khác, đặt tiền tố url của bạn với `../` và tên mô-đun. Ví dụ: `[Định tuyến Kohana](../kohana/routing)`

**Hình ảnh cũng là không gian tên**, sử dụng `![Văn bản thay thế](imagename.jpg)` sẽ tim kiếm `media/guide/<modulename>/imagename.jpg`.

## Liên kết API

Bạn có thể tạo các liên kết tới bộ duyệt api bằng việc bọc bất kỳ tên lớp nào trong dấu ngoặc vuông. Bạn cũng có thể bao gồm môt tiên hàm, hoặc một tên thuộc tính tới liên kết cụ thể. Tất cả các liên kết sau sẽ liên kết tới bộ duyệt API: 

	[Request]  
	[Request::execute]  
	[Request::execute()]  
	[Request::$status]  

[Request]  
[Request::execute]  
[Request::execute()]  
[Request::$status]  

Nếu bạn muốn có các tham số và có các hàm có thể bấm chuột được, chỉ đặt các dấu ngoặc vuông quanh lớp và hàm (không bao quanh các tham số), và đặt một gạch chéo ngược (\) đằng trước dấu ngoặc đơn mở.

	[Kohana::config]\('foobar','baz')
	
[Kohana::config]\('foobar','baz')

## Lưu ý

Nếu bặn đặt `[!!]` trước một dòng nó sẽ là một "lưu ý" và được đặt vào trong một hộp với một bóng đèn.

	[!!]  Đây là một lưu ý

sẽ hiển thị giống như:
	
[!!] Đây là một lưu ý

## Tiêu đề tự đống lấy các ID

Các tiêu đề được tự đống gán một id, dựa trên nội dung của tiêu đề, do đó mỗi tiêu đề có thể được liên kết tới. Bạn có thể tự gán một id khác bằng việc sử dụng cú pháp như đã định nghĩa trong Markdown Extra. Nếu nhiều tiêu đề có cùng nội dung (v.d. nhiều hơn một tiêu đề "Ví dụ"), chỉ tiêu đề đầu tiên sẽ tự động được gán một id, do đó bạn nên tự gán thêm các id mô tả. Ví dụ:

	### Ví dụ     {#more-descriptive-id}

## Bao gồm các View

Nếu bạn cần bạn có thể bao gồm một tập tin Kohana View bình thường bằng việc thay thế têm một view trong thẻ ngoặc kép nhọn. **Nếu view không được tìm thấy, không có lỗi hoặc ngoại lệ được hiển thị, các dấu ngoặc nhọn và tên view chỉ đơn giản là vẫn còn ở đó**

	{{some/view/file}}
