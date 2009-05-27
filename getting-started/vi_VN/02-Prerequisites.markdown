Yêu cầu
=============

Trước khi cài đặt symfony, bạn cần kiểu tra máy của mình để chắc rằng mọi thứ được cài đặt và cấu hình đúng. Hãy dành thời gian để đọc kĩ chương này và làm theo tất cả các yêu cầu để kiểm tra các thiết lập trên máy bạn, nó sẽ giúp bạn không phải mất thời gian với những lỗi phát sinh sau này.

Phần mềm
---------

Đầu tiên, bạn cần kiểm tra xem máy bạn có môi trường phát triển web chưa. Bạn cần một web server (ví dụ Apache), một hệ quản trị CSDL (MySQL, PostgreSQL, SQLite, ..), và PHP 5.2.4 hoặc mới hơn.

Giao diện dòng lệnh
----------------------

Framework symfony chứa sẵn công cụ dòng lệnh để tự động làm nhiều việc cho bạn. Nếu bạn là người dùng hệ điều hành họ Unix, bạn sẽ cảm thấy quen thuộc. Nếu bạn sử dụng Windows, bạn sẽ phải gõ một vài lệnh ở cửa sổ `cmd`.

>**Note**
>Unix shell commands có thể đưa vào môi trường Windows.
>Nếu bạn thích sử dụng những lệnh như `tar`, `gzip`, hay `grep` ở Windows, bạn
>có thể cài đặt [Cygwin](http://cygwin.com/).  Tài liệu trên trang chủ khá ít,
>bạn có thể đọc hướng dẫn cài đặt
>[tại đây](http://www.soe.ucsc.edu/~you/notes/cygwin-install.html).
>Bạn cũng có thể thử dùng Microsoft's
>[Windows Services for Unix](http://technet.microsoft.com/en-gb/interopmigration/bb380242.aspx).

Cấu hình PHP
-----------------

Cấu hình PHP khác nhau tùy vào hệ điều hành, thậm chí giữa các bản phân phối Linux. Bạn cần kiểm tra để chắc rằng cấu hình PHP đạt các yêu cầu tối thiểu của symfony.

Đầu tiên, hãy chắc rằng bạn có PHP 5.2.4 hoặc mới hơn bằng cách sử dụng function
`phpinfo()` hoặc chạy lệnh `php -v`. Bạn nên kiểm tra cả 2 cách do ở một vài cấu hình, bạn có thể có 2 phiên bản PHP được cài đặt: một cho dòng lệnh, và một cho web.

Sau đó, download script để kiểm tra cấu hình tại đường dẫn sau:

    http://sf-to.org/1.2/check.php

Lưu script này vào thư mục web root.

Chạy script từ dòng lệnh để kiểm tra cấu hình:

    $ php check_configuration.php

Nếu có vấn đề gì với cấu hình PHP hiện tại, đoạn script sẽ đưa ra gợi ý cách sửa.

Bạn cũng nên chạy script này từ trình duyệt và sửa các lỗi phát sinh. Đó là do PHP có thể có các file cấu hình `php.ini` riêng cho 2 môi trường, với các thiết lập khác nhau.

>**NOTE**
>Đừng quên xóa file sau khi kiểm tra xong.
