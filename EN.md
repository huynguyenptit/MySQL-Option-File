[Source](https://dev.mysql.com/doc/refman/5.7/en/option-files.html "Permalink to MySQL :: MySQL 5.7 Reference Manual :: 4.2.6 Using Option Files")

# MySQL :: MySQL 5.7 Reference Manual :: 4.2.6 Using Option Files

### 4.2.6 Sử dụng các tệp tin cấu hình

Hầu hết các chương trình MySQL đều có thể đọc các tùy chọn khởi động từ các file tùy chọn (thường được gọi là các file cấu hình). Các file tuỳ chọn cung cấp 1 cách thuận tiện nhất để sử dụng các tùy chọn thường được chỉ định để chúng không cần phải nhập vào mỗi dòng lệnh khi bạn chạy một chương trình nào đó.

Để xác định một chương trình có đọc file tùy chọn hay không, gọi nó bằng lệnh tùy chọn `\--help`. (Áp dụng cho [**mysqld**][1], sử dụng [`\--verbose`][2] và [`\--help`][3].) Nếu chương trình có đọc file tùy chọn,bảng thông báo hỗ trợ cho biết những tập tin nào nó tìm kiếm và những nhóm tùy chọn nó có thể nhận ra.

Chú ý: 

Một chương trình MySQL thường được bắt đầu với việc đọc tùy chọn `\--no-defaults`, không có file tùy chọn nào khác ngoài file `.mylogin.cnf`. 

Nhiều file tùy chọn là những file văn bản thông thường, được tạo bởi bất kì 1 trình chỉnh sửa văn bản nào. Có một ngoại lệ là file `.mylogin.cnf` có chứa các tùy chọn về đường dẫn khi đăng nhập.Đây là một file được mã hóa, tạo bởi tiện ích thuộc [**trình soạn thảo cấu hình mysql**][4]. Xem trong [Section 4.6.6, "**trình soạn thảo cấu hình mysql** — MySQL Configuration Utility"][4]. Một "đường dẫn đăng nhập" là một nhóm các tùy chọn mà chỉ cho phép một số các tùy chọn cố định như: `host`, `user`, `password`, `port` và `socket`. những chương trình phía Client được chỉ định với đường dẫn đăng nhập được đọc từ file `.mylogin.cnf`, sử dụng tùy chọn [`\--login-path`][5]. 

Để chỉ định thay thế tên tệp đường dẫn đăng nhập, đặt biến môi trường `MYSQL_TEST_LOGIN_FILE`. Biến này được sử dụng bởi công cụ kiểm thử **mysql-test-run.pl**, nhưng cũng được công nhận trong [**mysql_config_editor**][4] và trong các client của MySQL như [**mysql**][6], [**mysqladmin**][7], và những client khác. 

MySQL tìm kiếm những file tùy chọn theo thứ tự được mô tả trong cuộc thảo luận sau và đọc bất cứ nội dung nào tồn tại. Nếu một file tùy chọn mà bạn muốn sử dụng nhưng nó không tồn tại, hãy tạo nó bằng 1 phương pháp thích hợp, như vừa thảo luận. 

Trên môi trường Windows, Các chương trình MySQL đọc các tùy chọn khởi động từ những file hiển thị trong những bảng sau, theo 1 thứ tự đã được chỉ định (Các file liệt kê đầu danh sách được đọc trước, các file được đọc theo thứ tự ưu tiên). 

**Bảng 4.1 file tùy chọn được đọc trên môi trường hệ thống windows**

| File Name (Tên file)                                                                       | Purpose (Mục đích)                                                       |  
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |  
| ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.ini`, ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.cnf` | tùy chọn toàn cục                                                |  
| ``%WINDIR%`my.ini`, ``%WINDIR%`my.cnf`                                                     | tùy chọn toàn cục                                                |  
| `C:my.ini`, `C:my.cnf`                                                                     | tùy chọn toàn cục                                                |  
| `_`BASEDIR`_my.ini`, `_`BASEDIR`_my.cnf`                                                   | tùy chọn toàn cục                                                |  
| `defaults-extra-file`                                                                      | file được chỉ định với [`\--defaults-extra-file`][8] nếu có |  
| ``%APPDATA%`MySQL.mylogin.cnf`                                                             | tùy chọn đường dẫn đăng nhập (chỉ dành cho các client)                             |  

Trong bảng trước, `%PROGRAMDATA%` đại diện cho thư mục file hệ thống chứa dữ liệu của ứng dụng cho tất cả các người dùng trên máy chủ. Đường dẫn này mặc định là `C:ProgramData`  trên Microsoft Windows Vista hoặc các phiên bản mới hơn, và `C:Documents and SettingsAll UsersApplication Data` trên các phiên bản cũ của Microsoft Windows. 

`%WINDIR%` đại diện cho vị trí thư mục Windows của bạn.Nó thường là `C:WINDOWS`. Sử dụng lệnh sau để xác định vị trí chính xác của nó từ giá trị của biến môi trường `WINDIR`: 
    
    
    C:> echo %WINDIR%

`%APPDATA%` đại diện cho giá trị của thư mục chứa dữ liệu của các ứng dụng trên môi trường Windows. Sử dụng câu lệnh sau để xác định vị trí chính xác của nó từ giá trị của biến môi trường `APPDATA`: 
    
    
    C:> echo %APPDATA%

_`BASEDIR`_ đại diện cho thư mục gốc khi cài đặt MySQL. Khi MySQL 5.7 cài đặt hoàn tất bằng trình cài đặt MySQL,nó thường là `C:_`PROGRAMDIR`_MySQLMySQL 5.7 Server`, nơi m à_`PROGRAMDIR`_ đại diện thư mục của chương trình (thường là `Program Files` trên các phiên bản tiếng Anh của Windows), Xem thêm tại [Section 2.3.3, "MySQL Installer for Windows"][9]. 

Trên các hệ thống Unix và tương tự Unix, các chương trình MySQL đọc các file khởi động từ các file được biểu diễn trong bảng sau, theo một thứ tự nhất định ( các file đầu danh sách được đọc trước, và các file được đọc theo thứ tự ưu tiên). 

Ghi chú:

Trên các nền tảng Unix, MySQL bỏ qua các file cấu hình mà chúng có thể bị ghi đè ở mọi nơi. Đây giống như là ý tưởng về một biện pháp bảo mật.

**Bảng 4.2 Các file tùy chọn được đọc trên các hệ thống Unix và tương tự Unix**


| File Name               | Purpose                                                       |  
| ----------------------- | ------------------------------------------------------------- |  
| `/etc/my.cnf`           | Global options                                                |  
| `/etc/mysql/my.cnf`     | Global options                                                |  
| `_`SYSCONFDIR`_/my.cnf` | Global options                                                |  
| `$MYSQL_HOME/my.cnf`    | Server-specific options (server only)                         |  
| `defaults-extra-file`   | The file specified with [`\--defaults-extra-file`][8], if any |  
| `~/.my.cnf`             | User-specific options                                         |  
| `~/.mylogin.cnf`        | User-specific login path options (clients only)               |  

Trong bảng trước,dấu `~` đại diện cho thư mục home của user hiện tại ( giá trị của biến `$HOME`). 

_`SYSCONFDIR`_ đại diện cho thư mục được chỉ định với tùy chọn [`SYSCONFDIR`][10] cho **CMake** khi MySQL được xây dựng. Theo mặc định, đây thường là thư mục `etc`nằm trong thư mục cài đặt đã được biên dịch. 

`MYSQL_HOME` là biến môi trường chứa đường dẫn đến thư mục mà trong đó có chứa file `my.cnf` của một máy chủ cụ thể. Nếu `MYSQL_HOME` không được thiết đặt và bạn khởi động server sử dụng chương trình [**mysqld_safe**][11], [**mysqld_safe**][11] đặt nó thành _`BASEDIR`_, thư mục cài đặt ban đầu của MySQL. 

_`DATADIR`_ thường là `/usr/local/mysql/data`, mặc dù điều này có thể thay đổi theo nền tẳng và phương thức cài đặt. Giá trị của nó là vị trí của thư mục chứa dữ liệu được tích hợp khi MySQL được biên dịch, nó không phải vị trí được chỉ định trước bằng tùy chọn [`\--datadir`][12] khi [**mysqld**][1] khởi động.Sử dụng [`\--datadir`][12] tại thời điểm chạy không gây ảnh hưởng gì lên máy chủ khi tìm kiếm những file tùy chọn mà nó đọc trước mọi tiến trình tùy chọn.

Nếu tìm thấy nhiều phiên bản cho một tùy chọn đã có, phiên bản cuối cùng sẽ được ưu tiên, với một ngoại lệ là: đối với **[mysqld**][1], phiên bản _đầu tiên_  của tùy chọn `[\--user`][13] được sử dụng như một giải pháp đảm bảo an toàn bảo mật , để ngăn chặn một người dùng được chỉ định ghi đè lên bằng chế độ command line trong một file tùy chọn. 

Mô tả sau đây về cú pháp trong file tùy chọn được áp dụng cho những file mà bạn có thể chỉnh sửa một cách thủ công. Điều này không bao gồm file `.mylogin.cnf`, được tạo ra bằng **[mysql_config_editor**][4] và nó được mã hóa. 

Bất kỳ tùy chọn dài nào có thể được đưa ra trên dòng lệnh khi chạy một chương trình MySQL cũng có thể được cung cấp từ những file tùy chọn một cách tốt hơn. Để có được danh sách các tùy chọn một cách có sẵn cho các chương trình, chạy nó với tùy chọn `\--help`. (Đối với **[mysqld**][1], sử dụng `[\--verbose`][2] và `[\--help`][3].) 

Cú pháp để chỉ định các tùy chọn trong một tệp tùy chọn cũng tương tự như trong cú pháp của dòng lệnh(xem tại [Section 4.2.4, "Using Options on the Command Line"][14]). Tuy nhiên, trong một file tùy chọn, bạn bỏ qua hai dấu gạch ngang đầu hàng từ tên của tùy chọn và bạn phải chỉ định mỗi tùy chọn trên 1 dòng. Để ví dụ, `\--quick` và `\--host=localhost` trên chế độ dòng lệnh được chỉ định giống như là  `quick` và `host=localhost` trên các dòng phân biệt của file tùy chọn. Để chỉ định một tùy chọn của biểu mẫu `\--loose-_`opt_name`_` trong một file tùy chọn, viết nó như `loose-_`opt_name`_`. 

Các dòng trống trong các tệp tùy chọn thì được bỏ qua. CÁc dòng không trống thì phải thuộc các định dạng sau: 

* `#_`comment`_`, `;_`comment`_`

Dòng chú thích được bắt đầu bằng `#` hoặc `;`. một chú thích `#` cũng có thể được bắt đầu ở giữa dòng.

* `[_`group`_]`

_`group`_ là tên của một chưong trình hay một nhóm mà bạn muốn đặt một tùy chọn cho nó. sau dòng group này, mọi dòng thiết lập tùy chọn đều được áp dụng cho nhóm đã được đặt tên cho tới khi kết thúc file tùy chọn hoặc có một dòng group khác xuất hiện. Tên của nhóm tùy chọn không có phân biệt chữ hoa chữ thường. 

* `_`opt_name`_`

Điều này tương đương với `\--_`opt_name`_` trên chế độ dòng lệnh. 

* `_`opt_name`_=_`value`_`

Điều này tương đương với `\--_`opt_name`_=_`value`_` trên chế độ dòng lệnh. Trong một file tùy chọn, bạn có thể có khoảng trống xung quanh ký tự `=`, nhưng điều này thường không đúng trên chế độ dòng lệnh. Gía trị của tùy chọn có thể được đặt trong dấu nháy đơn hoặc dấu nháy kép, nó sẽ hữu ích hơn nếu giá trị của nó chứa kí tự chú thích `#`. 

các khoảng trống ở đầu và cuối được tự động bỏ đi trong những tên và giá trị tùy chọn.

bạn có thể sử dụng các ký tự đóng như `\b`, `\t`, `\n`, `\r`, `\\`, và `\s` trong các giá trị tùy chọn để đại diện cho backspace, tab, newline, carriage return, backslash, and space characters. trong các file tùy chọn, các quy tắc cho ký tự đặc biệt này là: 

* ký tự backslash theo sau một ký tự chuỗi đánh dấu đặc biệt được chuyển đổi thành một ký tự đại diện được trình bày theo trình tự. Ví dụ như, `\s`được chuyển đổi thành dấu khoảng trắng. 
* Ký tự  backslash không theo sau nó là một ký tự chuỗi thoát hợp lệ thì giá trị của nó sẽ không bị thay đổi. Ví dụ, `S` vẫn được giữ lại như cũ. 

Các quy tắc trước có nghĩa là với một ký tự backslash có thể được đưa ra như là  `\\`, hoặc là `\` nếu nó không có theo sau bởi một ký tự thoát khả dụng. 

Các quy tắc cho những chuỗi thoát trong file tùy chọn hơi khác so với những nguyên tắc cho các chuỗi thoát trong chuỗi ký tự của câu lệnh SQL. Trong bối cảnh thứ 2, nếu "_`x`_" không phải là một ký tự chuỗi thoát hợp lệ, `_`\x`_` trở thành "_`x`_" thay cho `_`\x`_`. Xem tại [Section 9.1.1, "String Literals"][15]. 

Các quy tắc thoát cho các giá trị file tùy chọn đặc biệt thích hợp đối với các tên đường dẫn trên môi trường Windows, sử dụng `\` là dấu tách trên đường dẫn. Dấu phân tách trên đường dẫn trong môi trường Windows là  `\\` nếu nó có một ký tự chuỗi thoát theo sau. Nó cũng có thể được viết là `\\` hoặc  `\` Nếu nó không có ký tự chuỗi thoát theo sau (giải thích theo nghĩa câu trên). một cách khác là , dấu  `/` có thể được sử dụng trong tên đường dẫn của Window và sẽ được coi như là  `\`. Giả sử bạn muốn chỉ định thư mục gốc `C:\Program Files\MySQL\MySQL Server 5.7` trong một file tùy chọn. Điều này có thể được thực hiện theo nhiều cách khác nhau. Một vài ví dụ như: 
    
    
    basedir="C:Program FilesMySQLMySQL Server 5.7"
    basedir="C:\Program Files\MySQL\MySQL Server 5.7"
    basedir="C:/Program Files/MySQL/MySQL Server 5.7"
    basedir=C:\ProgramsFiles\MySQL\MySQLsServers5.7

Nếu tên của nhóm tùy chọn trùng với tên của chương trình, các tùy chọn trong group sẽ áp dụng riêng cho chương trình đó. Ví dụ nhóm `[mysqld]`  và nhóm `[mysql]` áp dụng cho lần lượt máy chủ **[mysqld**][1] và chương trình client **[mysql**][6]. 

Các tùy chọn nhóm `[client]` được đọc bởi các chương trình client, được cung cấp trong phân phối của MySQL (nhưng _không phải_ bởi  **[mysqld**][1]). Để hiểu được làm cách nào các chương trình client bên thứ 3 sử dụng C API lại có thể sử dụng các file tùy chọn, đọc tại C API documentation ở [Section 27.8.7.50, "mysql_options()"][16]. 

Các group `[client]` cho phép bạn chỉ định những tùy chọn nào được áp dụng cho các client.Để ví dụ, `[client]` là một group thích hợp để chỉ định việc sử dụng mât khẩu khi kết nối tới server. (Nhưng hãy chắc chắn là các file tùy chọn chỉ cho phép bạn truy cập, vì thế những người khác không thể tìm ra mật khẩu của bạn) Hãy chắc chắn là không đặt các tùy chọn trong nhóm `[client]`  trừ khi nó được công nhận đối với tất cả các chương trình client mà bạn sử dụng. Các chương trình không hiểu các tùy chọn sẽ thoát sau khi hiển thị thông báo lỗi nếu bạn cố gắng chạy chúng.

Liệt kê thêm những nhóm tùy chọn chung  và sau đó là các nhóm đặc thù. Ví dụ, một nhóm `[client]`  phổ biến hơn bởi vì nó được đọc bởi tất cả các chương trình client, trong khi một nhóm `[mysqldump]` lại chỉ được đọc bởi **[mysqldump**][17]. Các tùy chọn đặc thù sau sẽ ghi đè các tùy chọn đặc thù trước đó,  vì vậy hay đặt các nhóm tùy chọn theo thứ tự  `[client]`, `[mysqldump]` cho phép **[mysqldump**][17]- một tùy chọn cụ thể được ghi đè lên tùy chọn của `[client]`. 

Đây là một tệp tùy chọn chung điển hình: 
    
    
    [client]
    port=3306
    socket=/tmp/mysql.sock
    
    [mysqld]
    port=3306
    socket=/tmp/mysql.sock
    key_buffer_size=16M
    max_allowed_packet=8M
    
    [mysqldump]
    quick

Đây là một file tùy chọn cho người dùng thông thường: 
    
    
    [client]
    # The following password will be sent to all standard MySQL clients
    password="my password"
    
    [mysql]
    no-auto-rehash
    connect_timeout=2

để tạo ra nhóm tùy chọn chỉ được đọc bởi các máy chủ **[mysqld**][1] từ hàng loạt các bản phát hành MySQL cụ thể, sử dụng các group với các tên như `[mysqld-5.6]`, `[mysqld-5.7]`, và tương tự thế. CÁc nhóm sau chỉ ra rằng cài đặt `[sql_mode`][18]  chỉ nên được sử dụng với các máy chủ MySQL từ phiên bản  5.7.x trở lên: 
    
    
    [mysqld-5.7]
    sql_mode=TRADITIONAL

Có thể sử dụng chỉ thị `!include` trong các file tùy chọn để include các file tùy chọn khác và chỉ thị `!includedir` để tìm kiếm các thư mục cụ thể cho các file tùy chọn. Ví dụ, để include file `/home/mydir/myopt.cnf`, sử dungj chỉ thị sau: 
    
    
    !include /home/mydir/myopt.cnf

Để tìm kiếm thư mục  `/home/mydir` và đọc những file tùy chọn được tìm thấy trong đó, sử dụng chỉ thị sau:
    
    
    !includedir /home/mydir

MySQL không đảm bảo về thứ tự các tệp tùy chọn trong thư mục sẽ được đọc 

Ghi chú: 

 Bất cứ file nào được tìm thấy và include vào bằng cách sử dụng chỉ thị `!includedir` trong các hệ điều hành Unix `phải` có tên file kết thục bằng  `.cnf`. Trên Windows, chỉ thị này sẽ kiểm tra các file với phần mở rộng là `.ini` hoặc `.cnf`.

Viết nội dung của các file tùy chọn được include như các file tùy chọn khác. Có nghĩa là, nó nên chứa các nhóm tùy chọn, mỗi nhóm được đứng trước bởi 1 dòng `[_`group`_]`  chỉ định chương trình mà tùy chọn áp dụng.

Trong khi 1 file được thêm đang được xử lý, chỉ những tùy chọn trong các nhóm mà chương trình hiện tại tìm kiếm được sử dụng. Các nhóm khác sẽ bị loại bỏ. Giả sử rằng file  `my.cnf` chứa dòng sau:     
    
    !include /home/mydir/myopt.cnf

Và giả sử là `/home/mydir/myopt.cnf` trông như thế này: 
    
    
    [mysqladmin]
    force
    
    [mysqld]
    key_buffer_size=16M

Nếu  `my.cnf` được xử lý bởi **[mysqld**][1], chỉ nhóm`[mysqld]` trong `/home/mydir/myopt.cnf` được sử dụng. Nếu file này được sử lý bởi **[mysqladmin**][7], chỉ có nhóm `[mysqladmin]` được sử dụng. Nếu file được xử lý bởi bất kỳ chương trình nào khác,không có tùy chọn nào trong `/home/mydir/myopt.cnf` được sử dụng. 

Chỉ thị  `!includedir` được xử lý tương tự trừ khi tất cả các file tùy chọn trong thư mục được gọi, được đọc. 

Nếu 1 file tùy chọn bao gồm các chỉ thị`!include` hoặc `!includedir`các file được gọi bởi các chỉ thị đó được xử lý mỗi khi file tùy chọn được xử lý, không quan tâm đến việc chúng xuất hiện ở đâu trong file

[1]: https://dev.mysql.com/mysqld.html "4.3.1 mysqld — The MySQL Server"
[2]: https://dev.mysql.com/server-options.html#option_mysqld_verbose
[3]: https://dev.mysql.com/server-options.html#option_mysqld_help
[4]: https://dev.mysql.com/mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility"
[5]: https://dev.mysql.com/option-file-options.html#option_general_login-path
[6]: https://dev.mysql.com/mysql.html "4.5.1 mysql — The MySQL Command-Line Tool"
[7]: https://dev.mysql.com/mysqladmin.html "4.5.2 mysqladmin — Client for Administering a MySQL Server"
[8]: https://dev.mysql.com/option-file-options.html#option_general_defaults-extra-file
[9]: https://dev.mysql.com/mysql-installer.html "2.3.3 MySQL Installer for Windows"
[10]: https://dev.mysql.com/source-configuration-options.html#option_cmake_sysconfdir
[11]: https://dev.mysql.com/mysqld-safe.html "4.3.2 mysqld_safe — MySQL Server Startup Script"
[12]: https://dev.mysql.com/server-options.html#option_mysqld_datadir
[13]: https://dev.mysql.com/server-options.html#option_mysqld_user
[14]: https://dev.mysql.com/command-line-options.html "4.2.4 Using Options on the Command Line"
[15]: https://dev.mysql.com/string-literals.html "9.1.1 String Literals"
[16]: https://dev.mysql.com/mysql-options.html "27.8.7.50 mysql_options()"
[17]: https://dev.mysql.com/mysqldump.html "4.5.4 mysqldump — A Database Backup Program"
[18]: https://dev.mysql.com/server-system-variables.html#sysvar_sql_mode

  