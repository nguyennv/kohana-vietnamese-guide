# Cấu hình

Theo mặc định Kohana được thiết lập để nạp các giá trị cấu hình từ [các tập tin cấu hình](files/config) trong hệ thống tập tin phân cấp.
Tuy nhiên, nó là rất dễ dàng để thích ứng với nó để nạp các giá trị cấu hình tại các địa điểm/định dạng khác.

## Nguồn

Hệ thống được thiết kế xung quanh khái niệm **Các nguồn cấu hình**,
cái mà có nghĩa lỏng lẻo là một phương thức lưu trữ giá trị cấu hình.

Để đọc cấu hình từ một nguồn mà bạn cần một **Bộ đọc cấu hình - Reader Config**.
Tương tự như vậy, để viết cấu hình tới một nguồn, bạn cần một **Bộ ghi cấu hình - Writer Config**.

Thực hiện chúng đơn giản là mở rộng giao diện [Kohana_Config_Reader] / [Kohana_Config_Writer]:

	class Kohana_Config_Database_Reader implements Kohana_Config_Reader
	class Kohana_Config_Database_Writer extends Kohana_Config_Database_Reader implements Kohana_Config_Writer

Bạn sẽ nhận thấy trong ví dụ trên là Database Writer mở rộng Database Reader.
Đây là quy ước với các nguồn cấu hình, lý do là nếu bạn có thể viết các thay đổi tới một nguồn bạn cũng có thể đọc từ nó như vậy.
Tuy nhiên, quy ước này không được thực thi và được để lại theo quyết định của nhà phát triển.

## Nhóm

Để trợ giúp tổ chức các giá trị cấu hình và chia thành các "nhóm" logic.
Ví dụ thiết lập cơ sở dữ liệu liên quan trong một nhóm `database`, và cài đặt phiên làm việc liên quan trong một nhóm `session`.

Làm thế nào các nhóm được lưu trữ/tổ chức tới nguồn cấu hình.
Ví dụ như các nguồn tập tin đặt cấu hình các nhóm khác nhau vào các tập tin khác nhau (`database.php`, `session.php`) trong khi các nguồn cơ sở dữ liệu sử dụng một cột để phân biệt giữa các nhóm.

Để nạp một nhóm cấu hình chỉ cần gọi `Kohana::$config->load()` với tên của nhóm bạn muốn để nạp:

	$config = Kohana::$config->load('my_group');

`load()` sẽ trả về một thể hiện của [Config_Group] mà đóng gói các giá trị cấu hình và đảm bảo rằng bất kỳ thay đổi được thực hiện sẽ được chuyển lại cho các trình ghi cấu hình.

Để có được một giá trị cấu hình từ một đối tượng [Config_Group] chỉ cần gọi [Config_Group::get]:

	$config = Kohana::$config->load('my_group');
	$value  = $config->get('var');

Để sửa đổi một giá trị gọi [Config_Group::set]:

	$config = Kohana::$config->load('my_group');
	$config->set('var', 'new_value');

### Phương thức thay thế cho việc nhận / thiết lập cấu hình

Ngoài các phương thức được mô tả ở trên, bạn cũng có thể truy cập các giá trị cấu hình bằng cách sử dụng dấu chấm để phác thảo một đường dẫn từ nhóm cấu hình tới giá trị bạn muốn:

	// Tập tin cấu hình: database.php
	return array(
		'default' => array(
			'connection' => array(
				'hostname' => 'localhost'
			)
		)
	);
	
	// Mã mà cần giá trị hostname:
	$hostname = Kohana::$config->load('database.default.connection.hostname');
	

Mà tương đướng với:

	$config = Kohana::$config->load('database')->get('default');
	
	$hostname = $config['connection']['hostname'];

Rõ ràng phương pháp này là nhỏ gọn hơn rất nhiều so với bản gốc, tuy nhiên hãy ghi nhớ rằng sử dụng `dot.notation` là chậm hơn rất nhiều so với việc gọi `get()` và duyệt qua các mảng chính mình.
Dot notation có thể có ích nếu bạn chỉ cần một biến cụ thể, nhưng nếu không nó là tốt nhất là sử dụng `get()`.

Như [Config_Group] mở rộng [Array_Object](http://php.net/manual/en/class.arrayobject.php) bạn cũng có thể sử dụng cú pháp mảng để nhận/thiết lập cấu hình vars:

	$config = Kohana::$config->load('database');
	
	// Nhận var
	$hostname = $config['default']['connection']['hostname'];
	
	// Thiết lập var
	$config['default']['connection']['hostname'] = '127.0.0.1';

Một lần nữa, cú pháp này là tốn kém hơn so với việc gọi `get()` / `set()`.

## Nhập cấu hình

Một trong những tính năng hữu ích của hệ thống cấu hình là nhập nhóm cấu hình.
Tính năng này làm việc trong một cách tương tự như hệ thống tập tin phân cấp,
với cấu hình từ các nguồn thấp hơn xuống ngăn xếp nguồn được sáp nhập với các nguồn rồi đẩy lên ngăn xếp.

Nếu hai nguồn chứa các biến cấu hình tương tự sau đó là một nguồn đẩy lên ngăn xếp sẽ ghi đè lên một trong những cấu hình từ nguồn "thấp hơn".
Tuy nhiên, nếu nguồn từ cao hơn trong ngăn xếp không có một biến cấu hình cụ thể nhưng một nguồn thấp hơn trong ngăn xếp có thì khi đó giá trị từ nguồn thấp hơn sẽ được sử dụng.

Vị trí của nguồn trong ngăn xếp được xác định bởi cách chúng được nạp trong bootstrap của bạn.
Theo mặc định khi bạn nạp một nguồn, nó được đẩy lên phía trên cùng của một ngăn xếp:

    // Ngăn xếp: <trống>
	Kohana::$config->attach(new Config_File);
	// Ngăn xếp: Config_File
	Kohana::$config->attach(new Config_Database);
	// Ngăn xếp: Config_Database, Config_File

Trong ví dụ trên, bất kỳ giá trị cấu hình được tìm thấy trong cơ sở dữ liệu sẽ ghi đè lên những người được tìm thấy trong hệ thống tập tin.
Ví dụ, bằng cách sử dụng các thiết lập đã nêu ở trên:

	// Cấu hình trong hệ thống tập tin:
		email:
			sender: 
				email: my.awesome.address@example.com
				name:  Unknown
			method: smtp
	
	// Cấu hình trong cơ sở dữ liệu:
		email:
			sender:
				email: my.supercool.address@gmail.com
				name:  Kohana Bot
	
	// Cấu hình trả lại bởi Kohana::$config->load('email')
		email:
			sender:
				email: my.supercool.address@gmail.com
				name:  Kohana Bot
			method: smtp

[!!] **Lưu ý:** Cú pháp trên chỉ đơn giản là giả mã để minh họa cho khái niệm về kết hợp cấu hình.

Trên một số dịp, bạn có thể muốn thêm một nguồn cấu hình để dưới cùng của ngăn xếp, để làm điều này bằng cách truyền `FALSE` như là tham số thứ hai tới `attach()`:

	// Ngăn xếp: <trống>
	Kohana::$config->attach(new Config_File);
	// Ngăn xếp: Config_File
	Kohana::$config->attach(new Config_Database, FALSE);
	// Ngăn xếp: Config_File, Config_Database

Trong ví dụ này, bất kỳ giá trị được tìm thấy trong hệ thống tập tin sẽ ghi đè lên những giá trị được tìm thấy trong db. Ví dụ:

	// Cấu hình trong hệ thống tập tin:
		email:
			sender: 
				email: my.awesome.address@example.com
				name:  Unknown
			method: smtp
	
	// Cấu hình trong cơ sở dữ liệu:
		email:
			sender:
				email: my.supercool.address@gmail.com
				name:  Kohana Bot
	
	// Cấu hình trả lại bởi Kohana::$config->load('email')
		email:
			sender:
				email: my.awesome.address@example.com
				name:  Unknown
			method: smtp

## Sử dụng nguồn cấu hình khác dựa trên môi trường

Trong một số tình huống của bạn sẽ cần phải sử dụng các giá trị cấu hình khác nhau tùy thuộc vào trạng thái của `Kohana::$environment` là gì.
Kiểm thử đơn vị là một ví dụ điển hình của tình huống như vậy.
Hầu hết các thiết lập có hai cơ sở dữ liệu, một cho phát triển bình thường và một riêng biệt cho kiểm thử đơn vị (để cô lập các thử nghiệm từ sự phát triển của bạn).

Trong trường hợp này bạn vẫn cần phải truy cập các thiết lập cấu hình được lưu trữ trong thư mục `config` vì nó có chứa các thiết lập chung mà cần thiết cho bất cứ môi trường ứng dụng của bạn (vd thiết lập mã hóa), thay thế nguồn `Config_File` mặc định là không thực sự là một lựa chọn.

Để làm việc này, bạn có thể đính kèm một trình đọc tập tin cấu hình riêng biệt mà nạp cấu hình của nó từ một thư mục con của thư mục `config` được gọi là "testing":

	Kohana::$config->attach(new Config_File);
	
	Kohana::$config->attach(new Config_Database);
	
	if (Kohana::$environment === Kohana::TESTING)
	{
		Kohana::$config->attach(new Config_File('config/testing'));
	}

Trong quá trình phát triển bình thường, ngăn xếp nguồn cấu hình trông giống như `Config_Database, Config_File('config')`.
Tuy nhiên, khi `Kohana::$environment === Kohana::TESTING` ngăn xếp trông giống như `Config_File('config/testing'), Config_Database, Config_File('config')`
