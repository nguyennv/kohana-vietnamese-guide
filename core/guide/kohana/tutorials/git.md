# Tạo một ứng dụng mới

[!!] Các ví dụ sau đây giả định rằng máy chủ web đã được thiết lập, và bạn sẽ tạo ra một ứng dụng mới tại <http://localhost/gitorial/>.

Sử dụng console của bạn, thay đổi tới thư mục rỗng `gitorial` và chạy `git init`.
Lệnh này sẽ tạo một cấu trúc trần cho một kho git mới.

Tiếp theo, chúng ta sẽ tạo một [mô-đun con](http://www.kernel.org/pub/software/scm/git/docs/git-submodule.html) cho thư mục `system`.
Vào <http://github.com/kohana/core> và sao chép "Clone URL":

![Github Clone URL](http://img.skitch.com/20091019-rud5mmqbf776jwua6hx9nm1n.png)

Bây giờ sử dụng URL để tạo mô-đun con cho `system`:

    git submodule add git://github.com/kohana/core.git system

[!!] Lệnh này sẽ tạo một liên kết tới phiên bản phát triển hiện thời của bản phát hành ổn định tiếp theo. Phiên bản phát triển gân như luôn luôn an toàn để sử dụng, có cùng API như bản tải xuống ổn định hiện thời với các sửa lỗi được áp dụng.

Bây giờ thêm bất cứ cái gì vào mô-đun con mà bạn cần. Ví dụ, nếu bạn cần mô-đun [Database]:

    git submodule add git://github.com/kohana/database.git modules/database

Sau khi các mô-đun con được thêm, chúng phải được khởi tạo:

    git submodule init

Bây giờ các mô-đun con đã được thêm, bạn có thể gửi chúng:

    git commit -m 'Added initial submodules'

Tiếp theo, hãy tạo cấu trúc thư mục ứng dụng. Đây là yêu cầu tối thiểu:

    mkdir -p application/classes/{controller,model}
    mkdir -p application/{config,views}
    mkdir -m 0777 -p application/{cache,logs}

Nếu bạn chạy `find application` bạn sẽ thấy:

    application
    application/cache
    application/config
    application/classes
    application/classes/controller
    application/classes/model
    application/logs
    application/views

Chúng tôi không muốn để gits theo dõi lưu vết hoặc các tập tin bộ nhớ đệm, do đó thêm một tập tin .gitignore tới mỗi thư mục.
Việc này sẽ bỏ qua tất cả tập tin không bị ẩn:

    echo '[^.]*' > application/{logs,cache}/.gitignore

[!!] Git bỏ qua các the mục rỗng, do đó việc thêm một tập tin `.gitignore` đảm bảo rằng git sẽ theo dõi thư mục, nhưng không phải các tập tin bên trong nó.

Bây giờ chúng ta cần các tập tin `index.php` và `bootstrap.php`:

    wget https://github.com/kohana/kohana/raw/3.1/master/index.php --no-check-certificate
    wget https://github.com/kohana/kohana/raw/3.1/master/application/bootstrap.php --no-check-certificate -O application/bootstrap.php

Cũng gửi các thay đổi này:

    git add application
    git commit -m 'Added initial directory structure'

Đó là tất cả để có nó. Bây giờ bạn có một ứng dụng mà đang sử dụng Git cho việc quản lý phiên bản.

## Thêm các mô-đun con
Để them một mô-đun con mới hãy hoàn chỉnh các bước sau:

1. chạy mã sau - git submodule thêm đường dẫn kho cho mỗi mô-đun con mới vd:

        git submodule add git://github.com/shadowhand/sprig.git modules/sprig

2. sau đó khởi tạo và cập nhật các mô-đun con:

        git submodule init
        git submodule update

## Cập nhật các mô-đun con

Tại một số điệm bạn có lẽ cũng sẽ muốn nâng cấp các mô-đun con của bạn.
Để cập nhật tất cả mô-đun con của bạn tới phiên bản `HEAD` mới nhất:

    git submodule foreach 'git checkout 3.1/master && git pull origin 3.1/master'

Để cập nhật một mô-đun con duy nhất, ví dụ, `system`:

    cd system
    git checkout 3.1/master
    git pull origin 3.1/master
    cd ..
    git add system
    git commit -m 'Updated system to latest version'

Nếu bạn muốn cập nhật một mô-đun con duy nhất tới một commit cụ thể:

    cd modules/database
    git pull origin 3.1/master
    git checkout fbfdea919028b951c23c3d99d2bc1f5bbeda0c0b
    cd ../..
    git add database
    git commit -m 'Updated database module'

Lưu ý rằng bạn cũng có thể kiểm tra các commit tại một điểm phát hành chính thức được dán thẻ, ví dụ:

    git checkout 3.1.0

Đơn giản chỉ cần chạy `git tag` mà không có đối số để nhận một danh sách các thẻ.

## Loại bỏ các mô-đun con
Để loại bỏ một mô-đun con mà không cần thiết hãy hoàn thành các bước sau:

1. mở .gitmodules và loại bỏ tất cả tham chiếu tới mô-đun con. Nó sẽ giống như thế này:

        [submodule "modules/auth"]
        path = modules/auth
        url = git://github.com/kohana/auth.git

2. mở .git/config và loại bỏ tham chiếu tới submodule\\

        [submodule "modules/auth"]
        url = git://github.com/kohana/auth.git

3. chạy git rm --cached path/to/submodule, v.d.

        git rm --cached modules/auth

**Lưu ý:** Không đặt dấu gạch chéo ở cuối đường dẫn. Nếu bạn đặt một dấu gạch chéo ở cuối đường dẫn của câu lệnh, nó sẽ thất bại.

## Cập nhật Kho URL từ xa

Trong sự phát triển của một dự án, nguồn gốc của mô-đun con có thể thay đổi vì lý do nào (bạn đã tạo ra rẽ nhánh của riêng bạn, máy chủ thay đổi URL, tên kho lưu trữ hoặc đường dẫn thay đổi, vv ..) và bạn sẽ phải cập nhậtnhững thay đổi.
Để làm như vậy, bạn sẽ cần phải thực hiện các bước sau đây:

1. chỉnh sửa tập tin. gitmodules, và thay đổi URL cho các mô-đun con mà đã thay đổi.

2. trong gốc của cây nguồn hãy chạy:

		git submodule sync

3. chạy `git init` để cập nhật cấu hình kho của dự án với các URL mới:

		git submodule init

Và nó được thực hiện, bây giờ bạn có thể tiếp tục đẩy và kéo các mô-đun của bạn mà không có vấn đề gì.

Nguồn: http://jtrancas.wordpress.com/2011/02/06/git-submodule-location/