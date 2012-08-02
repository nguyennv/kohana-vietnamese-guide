[!!]  Khi tài liệu được kết hợp với những hình ảnh/liên kết này nên cập nhật

# Đóng góp

Kohana là hướng cộng đồng, và chúng tôi dựa trên đóng góp cộng đồng cho các tài liệu.

## Hướng dẫn

Tài liệu nên sử dụng các câu hoàn chỉnh, ngữ phát tốt, và được làm rõ ràng nhất có thể.
Sử dụng nhiều mã ví dụ, nhưng chắc chắn rằng các ví dụ thực hiện theo các quy tắc và phong cách của Kohana.

Hãy cố gắng cập nhật thường xuyên, với mỗi lần cập nhật chỉ thay đổi một vài tập tin, hơn là thay đổi một tấn tập tin và cập nhật tất cả một lần.
Điều này sẽ làm nó dễ dàng hợ để cung cấp thông tin phản hồi và hợp nhất các thay đổi của bạn.
Hãy chắc chắn rằng các thông điệp cập nhật của bạn là rõ ràng và có tính mô tả.
Tồi: "Thêm tài liệu", Tốt: "Thêm dự thảo ban đầu của bản hướng dẫn", Tồi: "Sửa lỗi chính tả", Tốt: "Sửa lỗi chính tả trên trang xây dựng truy vấn"

Nếu bạn cảm thấy một trình đơn cần sắp xếp lại hoặc một module cần các trang mới, hay mở một [báo cáo lỗi](http://dev.kohanaframework.org/projects/userguide3/issues/new) và thảo luận nó.

## Phương pháp nhanh

Để nhanh chóng chỉ ra một điều gì đó cần cải tiến, hãy thông báo một [báo cáo lỗi](http://dev.kohanaframework.org/projects/userguide3/issues/new).

Nếu bạn muốn đóng góp một vài thay đổi, bạn có thể làm như vậy ngay từ trình duyệt của bạn mà không cần biết git!

Trước tiên tạo một tài khoản trên [Github](https://github.com/signup/free).

Bạn sẽ cần phân nhánh (fork) module cho vùng mà bạn muốn cải tiến.
Ví dụ, để cải tiến [tài liệu ORM](../orm) hãy phân nhánh <http://github.com/bluehawk/orm>.
Để cải tiến [tài liệu Kohana](../kohana), hãy phân nhánh <http://github.com/bluehawk/core>, v.v.
Vì vậy, hãy tìm module mà bạn muốn cải tiến và kích vào nút Fork ở phía trên bên phải.

![Fork the module](contrib-github-fork.png)

Các tập tin làm phần Hướng dẫn người dùng được tìm thấy trong `guide/<module>/`, và phần duyệt API đước làm từ các chú thích trong mã nguồn của chính nó.
Điều hướng đến một trong các tập tin mà bạn muốn thay đổi và kích và nút chỉnh sửa ở phía trên bên phải của trình xem tập tin.

![Click on edit to edit the file](contrib-github-edit.png)

Thực hiện các thay đổi và thêm một **thông điệp cập nhật chi tiết**.
Lặp lại việc này với nhiều tập tin mà bạn muốn cải tiến.
(Lưu ý rằng bạn không thể xem trước những thay đổi sẽ được thấy trừ khi bạn thực sự thử nghiệm nó tại cục bộ)

Sau khi bạn thực hiện những thay đổi của bạn, hãy gửi một yêu cầu để những cải tiến của bạn có thể được xem xét để được hợp vào trong tài liệu chính thức.

![Send a pull request](contrib-github-pull.png)

Sau khi yêu cầu của bạn được chấp nhận, bạn có thể xoá kho của bạn nếu bạn muốn.
Cập nhật của bạn sẽ được sao chép tới nhánh chính thức.

## Nếu bạn biết git

**Bluehawk rẽ nhánh tất cả mà có một nhánh `docs`. Hãy làm tất cả công việc trong nhánh đó**

Để kéo tất cả các nhánh tài liệu dễ dàng hơn, nhánh "docs" [http://github.com/bluehawk/kohana](http://github.com/bluehawk/kohana) chứa liên kết submodule git tới tất cả các nhánh "docs" khác, vì vậy bạn có thể nhân bản mà dễ dàng có được tất cả các tài liệu.
Tài liệu chính Kohana trong [http://github.com/bluehawk/core/tree/docs/guide/kohana/], và tài liệu cho mỗi mô-đun trong mô-đun tương ứng trong thư mục hướng dẫn.
(Một lần nữa, hãy chắc chắn bạn đang ở trong nhánh `docs`.)

**Phiên bản ngắn**: Rẽ nhánh nhánh của bluehawk của mô-đun có tài liệu mà bạn muốn cải thiện (v.d. `git://github.com/bluehawk/orm.git` hoặc `git://github.com/bluehawk/core.git`), lấy các nhánh tài liệu, thay đổi, và sau đó gửi tới bluehawk một yêu cầu kéo.

**Phiên bản dài:** (Điều này giả định bạn ít nhất biết cách của bạn xung quanh git, đặc biệt là các module con làm việc.)

 1. Rẽ nhánh kho cụ thể mà bạn muốn đóng góp trên github. (Ví dụ vào http://github.com/bluehawk/core và bấm nút fork.)

 1. Để thực hiện việc kéo thay đổi userguide mới dễ dàng hơn, Tôi đã tạo ra một nhánh của `Kohana` được gọi là `docs`, trong đó có các mô-đun con git của tất cả các nhánh doc khác. Bạn có thể hoặc thêm kho từ xa bằng tay tới kho Kohana có sẵn của bạn, hoặc tạo một cài đặt Kohana mới từ mỏ bằng cách thực hiện các lệnh này:
	
		git clone git://github.com/bluehawk/kohana
		
		# Lấy nhánh tài liệu
		git checkout origin/docs
		
		# Lấy thư mục hệ thống và tất cả mô-đun
		git submodule init
		git submodule update

 1. Bây giờ vào kho của khu vực tài liệu bạn muốn đóng góp và thêm kho đã rẽ nhánh của bạn như kho từ xa mới, và đẩy tới nó.
 
		cd system
		
		# chắc chắn rằng chúng ta cập nhật với nhánh tài liệu
		git merge origin/docs
		(nếu việc này thất bại hoặc bạn không thể gửi sau đó gõ "git checkout -b docs" để tạo một nhánh tài liệu cục bộ)
		
		# thêm kho của bạn như một kho từ xa mới
		git remote add <your name> git@github.com:<your name>/core.git
		
		# (thực hiện vài thay đổi tới tài liệu)
		
		# bây giờ gửi các thay đổi và đẩy tới kho của bạn
		git commit
		git push <your name> docs

 1. Gửi một yêu cầu kéo trên github.