Cài đặt Symfony
====================

### Thư mục Project

Trước khi cài đặt symfony, bạn cần tạo thư mục để chứa toàn bộ file của dự án:

    $ mkdir -p /home/sfproject
    $ cd /home/sfproject

Ở Windows:

    c:\> mkdir c:\dev\sfproject
    c:\> cd c:\dev\sfproject

>**NOTE**
>Với người dùng Windows, không nên đặt
>project trong các thư mục có tên chứa dấu cách, như thư mục
>`Documents and Settings`, hay `My Documents`.

-

>**TIP**
>Nếu bạn tạo project trong thư mục web root, bạn sẽ không phải cấu hình web server.  Tuy nhiên, với môi trường production, chúng tôi khuyên bạn cấu hình như mô tả trong mục cấu hình web server.

### Cài đặt Symfony

Tạo thư mục nằm trong project, chứa thư viện symfony framework :

    $ mkdir -p lib/vendor

Do symfony framework có vài phiên bản khác nhau, bạn cần đọc thông tin mô tả ở [trang cài đặt](http://www.symfony-project.org/installation) để chọn phiên bản phù hợp.

Chuyển đến trang cài đặt của phiên bản bạn đã chọn, ví dụ
[symfony 1.2](http://www.symfony-project.org/installation/1_2).

Dưới mục "**Download as an Archive**", bạn sẽ thấy file nén ở định dạng
`.tgz` hoặc `.zip`. Tải file nén về, đặt nó vào trong thư mục `lib/vendor/` và giải nén:

    $ cd lib/vendor
    $ tar zxpf symfony-1.2.2.tgz
    $ mv symfony-1.2.2 symfony
    $ rm symfony-1.2.2.tgz

Đổi tên thư mục thành `symfony`
`c:\dev\sfproject\lib\vendor\symfony`.

>**TIP**
>Nếu bạn sử dụng Subversion, nên sử dụng `svn:externals`
>để nhúng symfony vào project trong thư mục `lib/vendor/`,
>những sửa lỗi sẽ được tự động cập nhật:
>
>     http://svn.symfony-project.com/branches/1.2/

Kiểm tra xem symfony đã cài đặt đúng chưa bằng lệnh `symfony` để hiển thị phiên bản của symfony (chữ `V` viết hoa):

    $ cd ../..
    $ php lib/vendor/symfony/data/bin/symfony -V

Ở Windows:

    c:\> cd ..\..
    c:\> php lib\vendor\symfony\data\bin\symfony -V

>**TIP**
>Nếu bạn tò mò về những lệnh có thể thực hiện, gõ
>`symfony` để hiện danh sách các tác vụ và lựa chọn:
>
>     $ php lib/vendor/symfony/data/bin/symfony
>
>Ở Windows:
>
>     c:\> php lib\vendor\symfony\data\bin\symfony
>
>Lệnh của symfony rất tiện dụng. Nó cung cấp rất nhiều công cụ
>phục vụ cho công việc hằng ngày của bạn như
>xóa cache, tạo sẵn code, ...

### Đường dẫn symfony

Bạn có thể kiểm tra phiên bạn của symfony bằng cách gõ:

    $ php symfony -V

Option `-V` cũng hiển thị đường dẫn của thư mục cài đặt symfony,
đường dẫn này được lưu trong file `config/ProjectConfiguration.class.php`:

    [php]
    // config/ProjectConfiguration.class.php
    require_once '/Users/fabien/work/symfony/dev/1.2/lib/autoload/sfCoreAutoload.class.php';

Để thuận tiện, bạn nên đổi đường dẫn tuyệt đối sang đường dẫn tương đối:

    [php]
    // config/ProjectConfiguration.class.php
    require_once dirname(__FILE__).'/../lib/vendor/symfony/lib/autoload/sfCoreAutoload.class.php';

Với cách này, bạn có thể di chuyển project đến bất kì đâu, nó vẫn làm việc.
