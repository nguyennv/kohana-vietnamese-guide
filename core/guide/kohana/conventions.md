# Quy ước và Phong cách viết mã

Nó được khuyến khích rằng bạn thực hiện theo phong cách viết mã của Kohana. 
Việc này làm cho mã dễ đọc hơn và cho phép chia sẻ và đóng góp mã dễ dàng hơn.

## Tên lớp và vị trí tập tin

Tên lớp trong Kohana theo một quy ước nghiêm ngặt để tạo điều kiện cho việc [nạp tự động](autoloading).
Tên lớp lên có chữ chữ viết hoa đầu tiên với gạch dưới để phân tách các từ.
Gạch dưới có nghĩa khi chúng phản ánh trực tiếp lên vị trí tập tin trong hệ thống tập tin.

Các quy ước sau đây áp dụng:

1. Tên lớp CamelCased không được sử dụng, trừ khi nó là không mong muốn để tạo ra một mức thư mục mới.
2. Tất cả các tên tập tin lớp và tên thư mục là viết thường.
3. Tất cả các lớp nên ở trong thư mục `classes`. Điều này có thể ở bất cứ cấp nào trong [hệ thống tập tin phân cấp](files).

### Ví dụ  {#class-name-examples}

Hãy nhớ rằng trong một lớp, một gạch dưới có nghĩa là một thư mục mới. Hãy xem xét các ví dụ sau đây:

Tên lớp               | Đường dẫn tập tin
----------------------|-------------------------------
Controller_Template   | classes/controller/template.php
Model_User            | classes/model/user.php
Database              | classes/database.php
Database_Query        | classes/database/query.php
Form                  | classes/form.php

## Chuẩn viết mã

Để làm ra các mã nguồn có tính nhất quán cao, chúng tôi yêu cầu mọi người theo các tiêu chuẩn viết mã một cách chặt chẽ nhất có thể.

### Các dấu ngoặc

Vui lòng sử dụng [phong cách BSD/Allman](http://en.wikipedia.org/wiki/Indent_style#BSD.2FAllman_style) cho dấu ngoặc.

#### Dấu ngoặc nhọn

Dấu ngoặc nhọn được đặt trên dòng riêng của chúng, thụt dòng cùng mức với câu lệnh điều khiển.

	// Đúng
	if ($a === $b)
	{
		...
	}
	else
	{
		...
	}

	// Sai
	if ($a === $b) {
		...
	} else {
		...
	}

#### Dấu ngoặc cho lớp

Ngoại lệ duy nhất cho quy tắc dấu ngoặc nhọn là, dấu ngoặc mở của lớp trên cùng một dòng.

	// Đúng
	class Foo {

	// Sai
	class Foo
	{

#### Dấu ngoặc rỗng

Không đặt bất kỳ ký tự nào bên trong ngoặc rỗng.

	// Đúng
	class Foo {}

	// Sai
	class Foo { }

#### Dấu ngoặc cho mảng

Các mảng có thể là dòng đơn hoặc nhiều dòng.

	array('a' => 'b', 'c' => 'd')
	
	array(
		'a' => 'b', 
		'c' => 'd',
	)

##### Dấu ngoặc mở

Dấu ngoặc mở của mảng trên cùng một dòng.

	// Đúng
	array(
		...
	)

	// Sai:
	array
	(
		...
	)

##### Dấu ngoặc đóng

###### Một chiều

Dấu ngoặc đóng của một mảng một chiều nhiều dòng được đặt trên ròng của riêng nó, được thụt lề cùng mức với dấu gán hoặc câu lệnh.

	// Đúng
	$array = array(
		...
	)

	// Sai
	$array = array(
		...
		)

###### Nhiều chiều

Mảng lồng nhau được thụt lên một dấu tab về bên phải, theo quy tắc một chiều.

	// Đúng
	array(
		'arr' => array(
			...
		),
		'arr' => array(
			...
		),
	)
	
	array(
		'arr' => array(...),
		'arr' => array(...),
	)
	
##### Mảng như là tham số hàm


	// Đúng
	do(array(
		...
	))
	
	// Sai
	do(array(
		...
		))

Như đã nêu ở phần đầu của mục dấu ngoặc của mảng, cú pháp dòng đơn cũng hợp lệ.

	// Đúng
	do(array(...))
	
	// Thay thế cho việc bao gói các dòng dài
	do($bar, 'Đây là một dòng rất dài',
		array(...));

### Quy ước đặt tên

Kohana sử dụng cách đặt tên gạch dưới - under_score, không sử dụng đặt tên kiểu camelCase.

#### Lớp

	// Lớp Controller, sử dụng tiền tố Controller_
	class Controller_Apple extends Controller {

	// Lớp Model, sử dụng tiền tố Model_
	class Model_Cheese extends Model {

	// Lớp thông thường
	class Peanut {

Khi tạo một thể hiện của một lớp, không sử dụng dấu ngoặc đơn nếu bạn không truyền cái gì đó cho hàm khởi tạo:

	// Đúng:
	$db = new Database;

	// Sai:
	$db = new Database();

#### Hàm và phương thức

Các hàm nên là chữ thường, và sử dụng các dấu gạch dưới để phân tách các từ:

	function drink_beverage($beverage)
	{

#### Biến

Tất cả biến nên là chữ thường và sử dụng dấu gạch dưới, không sử dụng camelCase:

	// Đúng:
	$foo = 'bar';
	$long_example = 'sử dụng dấu gạch dưới';

	// Sai:
	$weDontWantThis = 'hiểu gì không?';

### Thụt đầu dòng

Bạn phải sử dụng ký tự tab để thụt đầu dòng mã của bạn. Việc sử dụng khoảng trắng cho tabbing là nghiêm cấm.

Khoảng trắng dọc (cho nhiều dòng) được thực hiện bằng các khoảng trắng.
Các dấu tab không tốt cho canh lề dọc vì nhiều người khác nhau có độ rộng tab khác nhau.

	$text = 'Đây là một khối văn bản dài mà được bọc. Thông thường, chúng tôi nhằm mục đích '
		  . 'bao gói ở 80 ký tự. Canh hàng đứng là rất quan trọng cho mã có thể đọc được. '
		  . 'Hãy nhớ rằng tất cả thụt ròng được thực hiện với các dấu tab,nhưng canh lề '
		  . 'dọc nên được hoàn thành với các khoảng trống, sau việc thụt dòng với các dấu tab.';

### Nối chuỗi

Không đặt các khoảng trắng xung quanh các toán tử nối:

	// Đúng:
	$str = 'one'.$var.'two';

	// Sai:
	$str = 'one'. $var .'two';
	$str = 'one' . $var . 'two';

### Các câu lệnh một dòng

Các câu lệnh IF một dòng nên chỉ được sử dụng khi thực thi lệnh ngắt dòng bình thường (vd. return hoặc continue):

	// Chấp nhận:
	if ($foo == $bar)
		return $foo;

	if ($foo == $bar)
		continue;

	if ($foo == $bar)
		break;

	if ($foo == $bar)
		throw new Exception('Bạn xử lý kém thế!');

	// Không chấp nhận:
	if ($baz == $bun)
		$baz = $bar + 2;

### Các toán tử so sánh

Vui lòng dùng OR và AND cho việc so sánh:

	// Đúng:
	if (($foo AND $bar) OR ($b AND $c))

	// Sai:
	if (($foo && $bar) || ($b && $c))
	
Vui lòng dùng elseif, không dùng else if:

	// Đúng:
	elseif ($bar)

	// Sai:
	else if($bar)

### Cấu trúc switch

Đối với mỗi trường hợp (case), break và default nên ở trên một dòng riêng biệt.
Khối bên trong một trường hợp hoặc mặc định (default) phải thụt dòng một tab.

	switch ($var)
	{
		case 'bar':
		case 'foo':
			echo 'xin chào';
		break;
		case 1:
			echo 'một';
		break;
		default:
			echo 'tạm biệt';
		break;
	}

### Dấu ngoặc đơn

Nên có một khoảng trắng sau một tên câu lệnh, tiếp theo bởi một dấu ngoặc đơn.
Ký tự ! (bang) phải có một khoảng trắng ở cả hai bên để đảm bảo tối đa việc dễ đọc.
Ngoại trừ trong trường hợp một ký tự ! hoặc một câu lệnh ép kiểu, không nên có khoảng trắng sau một ngoặc đơn mở hoặc trước ngoặc đơn đóng.

	// Đúng:
	if ($foo == $bar)
	if ( ! $foo)

	// Sai:
	if($foo == $bar)
	if(!$foo)
	if ((int) $foo)
	if ( $foo == $bar )
	if (! $foo)

### Tam phân

Tất cả toán tử tam phân nên theo định dạng chuẩn sau.
Sử dụng các dấu ngoặc đơn xung quanh các biểu thức, không quanh biến.

	$foo = ($bar == $foo) ? $foo : $bar;
	$foo = $bar ? $foo : $bar;

Tất cả các so sánh và các toán tử phải được thực hiện bên trong một nhóm các dấu ngoặc đơn:

	$foo = ($bar > 5) ? ($bar + $foo) : strlen($bar);

Khi tách các tam phân phức tạp (tam phân mà phần đầu vượt quá 80 ký tự) thành nhiều dòng, khoảng trắng nên được sử dụng để xếp hàng các toán tử, cái mà nên ở trước các dòng kế tiếp:

	$foo = ($bar == $foo)
		 ? $foo
		 : $bar;

### Ép kiểu

Việc ép kiểu nên được thực hiện vơi các khoảng trắng ở mỗi bên của đổi kiểu:

	// Đúng:
	$foo = (string) $bar;
	if ( (string) $bar)

	// Sai:
	$foo = (string)$bar;

Khi có thể, vui lòng sử dụng ép kiểu thay cho các toán tử tam phân:

	// Đúng:
	$foo = (bool) $bar;

	// Sai:
	$foo = ($bar == TRUE) ? TRUE : FALSE;

Khi ép kiểu thành kiểu nguyên hoặc kiểu boolean, hãy sử dụng định dạng ngắn:

	// Đúng:
	$foo = (int) $bar;
	$foo = (bool) $bar;

	// Sai:
	$foo = (integer) $bar;
	$foo = (boolean) $bar;

### Hằng

Luôn sử dụng chữ viết hoa cho các hằng:

	// Đúng:
	define('MY_CONSTANT', 'my_value');
	$a = TRUE;
	$b = NULL;

	// Sai:
	define('MyConstant', 'my_value');
	$a = True;
	$b = null;

Đặt so sánh dòng ở cuối câu lệnh kiểm tra:

	// Đúng:
	if ($foo !== FALSE)

	// Sai:
	if (FALSE !== $foo)

Đây là một sự lựa chọn hơi gây tranh cãi, vì vậy tôi sẽ giải thích lý do.
Nếu chúng ta để viết ví dụ trước bằng tiếng Anh, ví dụ chính xác sẽ đọc:

	if variable $foo is not exactly FALSE - nếu biến $foo không chính xác là FALSE

Và ví dụ không đúng sẽ đọc:

	if FALSE is not exactly variable $foo - nếu FALSE không chính xác là biến $foo

Vì chúng ta đang đọc trái sang phải, nó chỉ đơn giản là không có ý nghĩa để đặt hằng ở trước.

### Chú thích

#### Chú thích một dòng

Sử dụng //, tốt hơn là ở trên các dòng mã mà bạn sẽ chú thích cho nó.
Để một khoảng trắng sau nó và bắt đầu với một chữ viết hoa.
Không bao giờ sử dụng dấu #.

	// Đúng

	//Sai
	// sai
	# Sai

### Biểu thức chính quy

Khi viết mã các biểu thức chính quy hãy sử dụng PCRE hơn là POSIX.
PCRE được coi là mạnh hơn và nhanh hơn.

	// Đúng:
	if (preg_match('/abc/i', $str))

	// Sai:
	if (eregi('abc', $str))

Sử dụng dấu nháy đơn quanh các biểu thức chính quy của bạn hơn là dấu nháy kép.
Các chuỗi trích dẫn trong nháy đơn có nhiều thuận lợi hơn vì sự đơn giản của nó.
Không giống như các chuỗi trích dẫn trong nháy kép chúng không hỗ trợ biến nội hoặc tích hợp dãy gạch chéo ngược như \n hoặc \t, v.v.

	// Đúng:
	preg_match('/abc/', $str);

	// Sai:
	preg_match("/abc/", $str);

Khi thực hiện tìm kiểm hoặc thay thế một biểu thức chính quy, hay sử dụng ký hiệu $n cho backreferences - (tham chiếu lại).
Việc này thích hơn là dùng \n.

	// Đúng:
	preg_replace('/(\d+) dollar/', '$1 euro', $str);

	// Sai:
	preg_replace('/(\d+) dollar/', '\\1 euro', $str);

Cuối cùng, xin lưu ý rằng ký tự $ cho việc khớp vị trí tật cuối dòng cho phép một ký tự xuống dòng dau đây.
Sử dụng bổ đề (modifier) D để sửa lỗi nếu cần thiết.
[Xem thêm thông tin](http://blog.php-security.org/archives/76-Holes-in-most-preg_match-filters.html).

	$str = "email@example.com\n";

	preg_match('/^.+@.+$/', $str);  // TRUE
	preg_match('/^.+@.+$/D', $str); // FALSE