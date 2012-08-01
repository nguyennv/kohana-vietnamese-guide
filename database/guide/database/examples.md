# Ví dụ

Ở đây là vài ví dụ "thế giới thực" của việc sử dụng thư viện cơ sở dữ liệu để xây dựng các truy vấn của bạn và sử dụng các kết quả.

## Ví dụ về Câu lệnh đã chuẩn bị

TODO: 4-6 ví dụ về câu lệnh đã chuẩn bị phức tạp khác nhau, bao gồm một ví dụ tốt bind().

## Phân trang và tìm kiếm/lọc

Trong ví dụ này, chúng ta lặp thông qua một mảng của các trường đầu vào whitelisted và đối với mỗi giá trị không rỗng được phép và thêm nó tới các truy vấn tìm kiếm.
Chúng ta làm một bản sao của truy vấn và sau đó thực thi truy vấn đó để đếm tổng số kết quả.
Số lượng này sau đó được truyền tới lớp [Pagination](../pagination) để xác định offset tìm kiếm.
Vì dòng cuối cùng tìm kiếm với items_per_page của Pagination và các giá trị offset để trả về một trang của các kết quả dựa trên trang hiện thời người dùng ở trên.

	$query = DB::select()->from('users');
	
	//chỉ tìm kiếm cho các trường này
	$form_inputs = array('first_name', 'last_name', 'email');
	foreach ($form_inputs as $name) 
	{
		$value = Arr::get($_GET, $name, FALSE);
		if ($value !== FALSE AND $value != '')
		{
			$query->where($name, 'like', '%'.$value.'%');
		}
	}
	
	//sao chép truy vấn & thực thi nó
	$pagination_query = clone $query;
	$count = $pagination_query->select('COUNT("*") AS mycount')->execute()->get('mycount');
	
	//truyền tổng số đếm các mục tới Pagination
	$config = Kohana::$config->load('pagination');
	$pagination = Pagination::factory(array(
		'total_items' => $count,
		'current_page'   => array('source' => 'route', 'key' => 'page'), 
		'items_per_page' => 20,
		'view'           => 'pagination/pretty',
		'auto_hide'      => TRUE,
	));
	$page_links = $pagination->render();
	
	//tìm kiếm các kết quả bắt đầu tại offset được tính toán bởi lớp Pagination
	$query->order_by('last_name', 'asc')
		->order_by('first_name', 'asc')
		->limit($pagination->items_per_page)
		->offset($pagination->offset);
	$results = $query->execute()->as_array();

## Having

TODO: Ví dụ tại đây

[!!]  Chúng ta có thể sử dụng thêm các ví dụ trên trang này. 