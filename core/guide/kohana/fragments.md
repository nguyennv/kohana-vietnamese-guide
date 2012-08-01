# Phân mảnh

Phân mảnh là một cách nhanh chóng và đơn giản để tạo bộ nhớ đệm HTML hoặc đầu ra khác.
Phân mảnh không hữu ích cho việc tạo bộ nhớ đệm các đối tượng hoặc các kết quả cơ sở dữ liệu thô, trong trường hợp bạn sẽ sử dụng một phương thức tạo bộ nhớ đệm mạnh mẽ hơn, cái mà có thể đạt được với [mô-đun Cache](../cache).
Phân mảnh sử dụng [Kohana::cache()] và sẽ được đặt trong thư mục cache (mặc định là `application/cache`).

Bạn nên sử dụng Phân mảnh (hoặc bất kỳ giải pháp tạo bộ nhớ đệm nào) khi đọc bộ nhớ đệm nhanh hơn so với việc tái xử lý kết quả.
Việc đọc và phân tích các tập tin từ xa, phân tích một mẫu phức tạp, tính toán vài thứ, v.v.

Phân mảnh thông thường được sử dụng trong các tập tin khung nhìn.

## Cách dùng

Phân mảnh thường được sử dùng bằng cách gọi [Fragment::load()] nếu một câu lệnh `if` đặt tại chỗ bắt đầu của cái mà bạn muốn đưa vào bộ nhớ đệm, và [Fragment::save()] đặt tại chỗ kết thúc.
Chúng sử dụng [đệm đầu ra](http://www.php.net/manual/en/function.ob-start.php) để bắt dữ liệu đầu ra giữa hai lần gọi hàm.

Bạn có thể chỉ định vòng đời (tính theo giây) của Phân mảnh bằng cách sử dụng tham số thứ hai của [Fragment::load()].
Vòng đời mặc định là 30 giây.
Bạn có thể sử dụng trình trợ giúp [Date] để làm cho thời gian dễ đọc hơn.

Phân mảnh sẽ lưu trữ một bộ nhớ đệm khác nhau cho mỗi ngôn ngữ (sử dụng [I18n]) nếu bạn truyền `true` vào tham số thư ba tới [Fragment::load()];

Bạn có thể buộc xoá một Phân mảnh bằng cách sử dụng [Fragment::delete()], hoặc chỉ định một vòng đời là 0.

~~~
// Tạo bộ nhớ đệm trong 5 phút, và tạo bộ nhớ đệm cho mỗi ngôn ngữ
if ( ! Fragment::load('foobar', Date::MINUTE * 5, true))
{
    // Bất kỳ thứ gì mà được echo ở đây sẽ được lưu lại
    Fragment::save();
}
~~~

## Ví dụ: Tính toán số Pi

Trong ví dụ này chúng ta sẽ tính toán số pi tới 1000 chỗ, và tạo bộ nhớ đệm cho kết quả bằng cách sử dụng một phân mảnh.
Lần đầu bạn chạy ví dụ này nó có thể lấy vài giây, nhưng các lần tải sao đó sẽ nhanh hơn nhiều, cho đến khi vòng đời phân mảnh hết.

~~~
if ( ! Fragment::load('pi1000', Date::HOUR * 4))
{   
    // Thay đổi giới hạn lồng hàm
    ini_set('xdebug.max_nesting_level',1000);
    
    // Nguồn: http://mgccl.com/2007/01/22/php-calculate-pi-revisited
    function bcfact($n)
    {
      return ($n == 0 || $n== 1) ? 1 : bcmul($n,bcfact($n-1));
    }
    function bcpi($precision)
    {
        $num = 0;$k = 0;
        bcscale($precision+3);
        $limit = ($precision+3)/14;
        while($k < $limit)
        {
            $num = bcadd($num, bcdiv(bcmul(bcadd('13591409',bcmul('545140134', $k)),bcmul(bcpow(-1, $k), bcfact(6*$k))),bcmul(bcmul(bcpow('640320',3*$k+1),bcsqrt('640320')), bcmul(bcfact(3*$k), bcpow(bcfact($k),3)))));
            ++$k;
        }
        return bcdiv(1,(bcmul(12,($num))),$precision);
    }
    
    echo bcpi(1000);
    
	Fragment::save();
}

echo View::factory('profiler/stats');

?>
~~~

## Ví dụ: Các chỉnh sửa Wikipedia gần đây

Trong ví đụ này chúng ta sẽ sử dụng lớp [Feed] để lấy và phân tích một nguồn RSS của các lần chỉnh sửa gần đây của [http://en.wikipedia.org](http://en.wikipedia.org), sau đó sử dụng Phân mảnh để tạo bộ nhớ đệm cho các kết quả.

~~~
$feed = "http://en.wikipedia.org/w/index.php?title=Special:RecentChanges&feed=rss";
$limit = 50;

// Các nguồn dữ liệu đã hiển thị được lưu vào bộ nhớ đệm trong 30 giây (mặc định)
if ( ! Fragment::load('rss:'.$feed)):

	// Phân tích nguồn dữ liệu
	$items = Feed::parse($feed, $limit);
	
	foreach ($items as $item):
	
		// Chuyển đổi $item thành đối tượng
		$item = (object) $item;
		
		echo HTML::anchor($item->link,$item->title);
		
		?>
		<blockquote>
			<p>tác giả: <?php echo $item->creator ?></p>
			<p>ngày: <?php echo $item->pubDate ?></p>
		</blockquote>
		<?php
		
	endforeach;

	Fragment::save();

endif;

echo View::factory('profiler/stats');
~~~

## Ví dụ: Phân mảnh lồng nhau

Bạn có thẻ phân mảnh lồng nhau với các vòng đời khác nhau để cung cấp điều khiển cụ thể hơn.
Ví dụ, giả sử trang của bạn có nhiều nội dung động vì vậy chúng ta muốn tạo bộ nhớ đệm cho nó với một vòng đời là năm phút, nhưng một phần có nhiều thời gian hơn để tạo ra, và chỉ thay đổi mỗi giờ.
Không có lý do gì để tạo nó mỗi 5 phút, vì thế chúng ta sẽ sử dụng một phân mảnh lồng nhau.

[!!] Nếu một phân mảnh lồng nhau có một vòng đời ngắn hơn mảnh cha, nó sẽ chỉ được xử lý khi mảnh cha đã hết hạn.

~~~
// Tạo bộ nhớ đệm trang chủ trong năm phút
if ( ! Fragment::load('homepage', Date::MINUTE * 5)):

	echo "<p>Home page stuff</p>";
	
	// Giả vờ như chúng ta đang thực sự làm một cái gì đó :)
	sleep(2);
	
	// Tạo bộ nhớ đệm đoạn này mỗi giờ khi thấy nó không thay đổi thường xuyên
	if ( ! Fragment::load('homepage-subfragment', Date::HOUR)):
	
		echo "<p>Home page special thingy</p>";
		
		// Cứ cho rằng như thế này mất một thời gian dài
		sleep(5);
		
	Fragment::save(); endif;
	
	echo "<p>More home page stuff</p>";
	
	Fragment::save();

endif;

echo View::factory('profiler/stats');
~~~