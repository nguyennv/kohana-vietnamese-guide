# Giới thiệu bộ nhớ đệm Kohana

[Kohana_Cache] cung cấp một giao diện chung cho một loạt các động cơ bộ nhớ đệm.
[Cache_Tagging] được hỗ trợ nơi có sẵn nguyên bản tới các hệ thống bộ nhớ đệm.
Bộ nhớ đệm Kohana hỗ trợ nhiều thể hiển của các động cơ bộ nhớ đệm thông qua mẫu singleton được nhóm lại.

## Các động cơ nhớ đệm được hỗ trợ

 *  APC ([Cache_Apc])
 *  File ([Cache_File])
 *  Memcached ([Cache_Memcache])
 *  Memcached-tags ([Cache_Memcachetag])
 *  SQLite ([Cache_Sqlite])
 *  Wincache ([Cache_Wincache])

## Giới thiệu về bộ nhớ đệm

Bộ nhớ đệm cần được thực hiện với sự xem xét.
Thông thường, việc tạo bộ nhớ đệm kết quả các tài nguyên nhanh hơn việc tái xử lý chúng.
Việc chọn cái gì, như thế nào và khi nào tạo bộ nhớ đệm là rất quan trọng.
[PHP APC](http://php.net/manual/en/book.apc.php) là một trong các hệ thống bộ nhớ đệm nhanh nhất có sẵn, theo sau là [Memcached](http://memcached.org/). [SQLite](http://www.sqlite.org/) và bộ nhớ đệm bằng tập tin là hai phương pháp bộ nhớ đệm chậm nhất, tuy nhiên thường nhanh hơn so với việc tái xử lý một tập phức tạp các chỉ dẫn.

Các động cơ bộ nhớ đệm mà sử dụng bộ nhớ nhanh hơn đáng kể so với động cơ dựa vào tập tin lựa chọn thay thế.
Nhưng bộ nhớ bị hạn chế trong khi không gian đĩa lại dồi dào.
Nếu việc tạo bộ nhớ đệm các tập dữ liệu lớn, chẳng hạn như tập kết quả cơ sở dữ liệu lớn, tốt nhất là sử dụng bộ nhớ đệm bằng tập tin.

 [!!] Các trình điều khiển bộ nhớ đệm yêu cầu các phần mở rộng PHP có liên quan phải được cài đặt. APC, eAccelerator, Memecached và Xcache là tất cả các phần mở rộng PHP phi tiêu chuẩn được yêu cầu.

## Những gì mà mô-đun bộ nhớ đệm Kohana làm (và không làm)

Mô-đun này cung cập một giao diện trừu tượng đơn giản có nhiều lựa chọn các động cơ bộ nhớ đệm phổ biến của PHP.
API bộ nhớ đệm cung cấp các phương thức bộ nhớ đệm cơ bản được thực hiện qua tất cả giải pháp, bộ nhớ, mạng hoặc dựa trên đĩa.
Việc lưu khoá / giá trị cơ bản được hỗ trợ bằng tất cả trình điều khiển, với tính năng gắn thẻ bổ xung và hỗ trợ thu gom rác nơi được thực hiện hoặc được yêu cầu.

_Bộ nhớ đệm Kohana_ không cung cấp bộ nhớ đệm phong cách HTTP cho trình khách (trình duyệt web) và/hoặc các proxy (_Varnish_, _Squid_). Có các mô-đun khác mà cung cấp chức năng này.

## Chọn một trình cung cập bộ nhớ đệm

Việc nhận và thiết lập các giá trị tới bộ nhớ đệm là rất đơn giản khi sử dụng gia diện _Bộ nhớ đệm Kohana_.
Sự lựa chọn khó khắn nhất là lựa chọn động cơ bộ nhớ đệm để sử dụng.
Khi chọn lựa một động cơ bộ nhớ đệm, các tiêu chí sau đây phải được xem xét:

 1. __Bộ nhớ đệm có cần phải được phân tán?__
    Đây là một xem xét quan trọng vì nó sẽ giới hạn khắc khe các tuỳ chọn có sẵn cho các giải pháp như Memcache khi một giải pháp phân tán được yêu cầu.
 2. __Bộ nhớ đệm có cần phải nhanh?__
    Trong hầu hết tất cả trường hợp việc lấy dữ lệu từ bộ nhớ đệm nhanh hơn việc thực thi. Tuỳ nhiên nhìn chung bộ nhớ đệm dựa trên bộ nhớ nhanh hơn đáng kể bộ nhớ đệm dựa trên đĩa (xem bảng dưới đây).
 3. __Bao nhiêu bộ nhớ đệm là cần thiết?__
    Bộ nhớ đệm không phải là vô tận, và các bộ nhớ đệm dựa trên bộ nhớ tuỳ thuộc vào tài nguyên lưu trữ giới hạn hơn một cách đáng kể.

Bộ điều khiển  | Lưu trữ    | Tốc độ   | Thẻ       | Phân tán | Tự động thu gom rác          | Lưu ý
---------------| -----------| -------- | --------- | -------- | ---------------------------- | -----------------------
APC            | __Bộ nhớ__ | Xuất sắc | Không     | Không    | Có    | Giải pháp bộ nhớ đệm opcode PHP phổ biến rộng rãi, cải thiện hiệu suất thực hiện php
Wincache       | __Bộ nhớ__ | Xuất sắc | Không     | Không    | Có    | Biến thể Windows của APC
File           | __Đĩa__    | Tồi      | Không     | Không    | Không | Nhanh hơn chút ít so với việc thực thi
Memcache (tag) | __Bộ nhớ__ | Tốt      | Không(có) | Có       | Có    | Giải pháp phân tán nhanh thông thường, nhưng có một tốc độ lượt truy cập do độ trễ mạng biến đổi
Sqlite         | __Đĩa__    | Tồi      | Có        | Không    | Không | Nhanh hơn chút ít so với việc thực thi

Nó có thể có các giải pháp bộ nhớ đẹm lai ghép mà sử dụng một sự kết hợp các máy ở trên trong các ngữ cảnh khác nhau.
Việc này được hỗ trợ tốt với __Bộ nhớ đệm Kohana__.


## Các yêu cầu tối thiểu

 *  Kohana 3.0.4
 *  PHP 5.2.4 or hoặc lớn hơn