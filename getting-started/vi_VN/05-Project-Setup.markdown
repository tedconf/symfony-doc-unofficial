Tạo Project
=============

Ở symfony, các **application** chia sẻ cùng một data model và được nhóm lại trong một
**project**. Với đa số project, chúng ta sẽ có 2 application khác nhau: frontend và backend.

### Tạo Project

Ở thư mục `sfproject/`, chạy tác vụ symfony `generate:project` để thực hiện việc tạo symfony project:

    $ php lib/vendor/symfony/data/bin/symfony generate:project PROJECT_NAME

Ở Windows:

    c:\> php lib\vendor\symfony\data\bin\symfony generate:project PROJECT_NAME

Tác vụ `generate:project` tạo ra cấu trúc thư mục và file mặc định cần thiết cho một project symfony:

 | Thư mục     | Mô tả
 | ----------- | ----------------------------------
 | `apps/`     | Chứa các application của project
 | `cache/`    | Chứa các file cache của framework
 | `config/`   | Chứa các file cấu hình
 | `lib/`      | Chứa các thư viện dùng trong project
 | `log/`      | Chứa file log của framework
 | `plugins/`  | Chứa các plugin đã cài đặt
 | `test/`     | Chứa các file unit và functional test
 | `web/`      | Thư mục web root (xem bên dưới)

>**NOTE**
>Tại sao symfony tạo ra quá nhiều file như vậy? Một trong những lợi ích chính của việc sử dụng full-stack framework là chuẩn hóa việc phát triển. Nhờ có cấu trúc
>file và thư mục mặc định của symfony, bất kì lập trình viên nào có kiến thức về
>symfony cũng có thể maintenance project symfony.
>Trong vài phút, anh ta đã có thể nắm bắt được code, sửa lỗi,
>và thêm tính năng mới.

Tác vụ `generate:project` cũng tạo một shortcut `symfony` ở thư mục gốc của project để giảm số kí tự bạn phải gõ khi chạy một lệnh.

Do đó, từ bây giờ, thay vì sử dụng đầy đủ đường dẫn đến symfony, bạn chỉ cần dùng shortcut `symfony`.

### Tạo Application

Bây giờ, tạo ứng dụng frontend bằng cách chạy tác vụ `generate:app`:

    $ php symfony generate:app --escaping-strategy=on
      ➥ --csrf-secret=UniqueSecret frontend

>**TIP**
>Do file shortcut symfony được thực thi, người dùng Unix có thể thay thế cụm
>'`php symfony`' bằng '`./symfony`' từ bây giờ.
>
>Ở Windows bạn có thể copy file '`symfony.bat`' tới project của bạn và sử dụng
>'`symfony`' thay vì '`php symfony`':
>
>     c:\> copy lib\vendor\symfony\data\bin\symfony.bat .

Dựa vào tên ứng dụng được cung cấp dưới dạng một *tham số*, tác vụ `generate:app` tạo cấu trúc thư mục mặc định cần thiết trong thư mục `apps/frontend/`:

 | Thư mục      | Mô tả
 | ------------ | -------------------------------------
 | `config/`    | Chứa file cấu hình của application
 | `lib/`       | Chứa thư viện dùng trong application
 | `modules/`   | Mã nguồn application (MVC)
 | `templates/` | File template toàn cục

>**TIP**
>Khi gọi tác vụ `generate:app`, bạn cũng đã cung cấp 2 *options* bảo mật:
>
>  * `--escaping-strategy`: Cho phép output escaping để tránh tấn công XSS
>  * `--csrf-secret`: Cho phép session tokens trong forms để tránh tấn công CSRF
>
>Bằng cách cung cấp 2 tham số này cho tác vụ, bạn đã bảo vệ ứng dụng của mình
>khỏi những tấn công phổ biến trên
>web. Đúng vậy, symfony sẽ tự động bảo mật cho bạn.
>
>Nếu bạn chưa biết về
>[XSS](http://en.wikipedia.org/wiki/Cross-site_scripting) hay
>[CSRF](http://en.wikipedia.org/wiki/CSRF), hãy dành chút thời gian để tìm hiểu về chúng.

### Cấu trúc thư mục đúng

Trước khi thử truy cập project bạn vừa tạo, bạn cần thiết lập quyền khi cho thư mục
`cache/` và `log/`, để web server có thể ghi vào đó:

    $ chmod 777 cache/ log/

>**SIDEBAR**
>Thủ thuật cho người sử dụng công cụ SCM
>
>symfony chỉ ghi lên 2 thư mục
>`cache/` và `log/`. Nội dung của những thư mục này có thể được bỏ qua
>bởi SCM (bằng cách sửa property `svn:ignore` nếu bạn dùng Subversion).

### Cấu hình cơ sở dữ liệu

Một trong những điều đầu tiên bạn muốn làm là cấu hình để kết nối đến cơ sở dữ liệu cho project của bạn. symfony framework hỗ trợ tất cả các sơ sở dữ liệu hỗ trợ [PDO]((http://www.php.net/PDO)) (MySQL, PostgreSQL,
SQLite, Oracle, MSSQL, ...). Symfony chứa sẵn 2 ORM: Propel và Doctrine. Propel là mặc định, nhưng chuyển sang Doctrine thực sự dễ dàng (xem mục tiếp theo).

Cấu hình cơ sở dữ liệu đơn giản là sử dụng tác vụ `configure:database`:

    $ php symfony configure:database "mysql:host=localhost;dbname=dbname" root mYsEcret

Tác vụ `configure:database` nhận 3 tham số: [~PDO DSN~](http://www.php.net/manual/en/pdo.drivers.php), username, và password để truy cập database. Nếu bạn không cần password để truy cập database ở server develop, bạn có thể bỏ qua tham số thứ 3.

### Chuyển sang Doctrine

Nếu bạn quyết định sử dụng Doctrine thay cho Propel, đầu tiên bạn cần enable
`sfDoctrinePlugin` và disable `sfPropelPlugin`. Điều này có thể thực hiện bằng cách thay đổi đoạn code dưới đây trong file `config/ProjectConfiguration.class.php`:

    [php]
    public function setup()
    {
      $this->enableAllPluginsExcept(array('sfPropelPlugin', 'sfCompat10Plugin'));
    }

Sau khi lưu lại thay đổi, chạy những lệnh sau:

    $ php symfony plugin:publish-assets
    $ php symfony cc
    $ rm web/sfPropelPlugin
    $ rm config/propel.ini
    $ rm config/schema.yml
    $ rm config/databases.yml

Sau đó, chạy lệnh để cấu hình database cho Doctrine:

    $ php symfony configure:database --name=doctrine --class=sfDoctrineDatabase "mysql:host=localhost;dbname=jobeet" root mYsEcret
