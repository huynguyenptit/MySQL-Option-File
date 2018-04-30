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

_`DATADIR`_ thường là `/usr/local/mysql/data`, mặc dù điều này có thể thay đổi theo nền tẳng và phương thức cài đặt. Giá trị của nó là vị trí của thư mục chứa dữ liệu được tích hợp khi MySQL được biên dịch, nó không phải vị trí được chỉ định trước bằng tùy chọn [`\--datadir`][12] khi [**mysqld**][1] khởi động.Sử dụng [`\--datadir`][12] at runtime has no effect on where the server looks for option files that it reads before processing any options. 

If multiple instances of a given option are found, the last instance takes precedence, with one exception: For **[mysqld**][1], the _first_ instance of the `[\--user`][13] option is used as a security precaution, to prevent a user specified in an option file from being overridden on the command line. 

The following description of option file syntax applies to files that you edit manually. This excludes `.mylogin.cnf`, which is created using **[mysql_config_editor**][4] and is encrypted. 

Any long option that may be given on the command line when running a MySQL program can be given in an option file as well. To get the list of available options for a program, run it with the `\--help` option. (For **[mysqld**][1], use `[\--verbose`][2] and `[\--help`][3].) 

The syntax for specifying options in an option file is similar to command-line syntax (see [Section 4.2.4, "Using Options on the Command Line"][14]). However, in an option file, you omit the leading two dashes from the option name and you specify only one option per line. For example, `\--quick` and `\--host=localhost` on the command line should be specified as `quick` and `host=localhost` on separate lines in an option file. To specify an option of the form `\--loose-_`opt_name`_` in an option file, write it as `loose-_`opt_name`_`. 

Empty lines in option files are ignored. Nonempty lines can take any of the following forms: 

* `#_`comment`_`, `;_`comment`_`

Comment lines start with `#` or `;`. A `#` comment can start in the middle of a line as well. 

* `[_`group`_]`

_`group`_ is the name of the program or group for which you want to set options. After a group line, any option-setting lines apply to the named group until the end of the option file or another group line is given. Option group names are not case-sensitive. 

* `_`opt_name`_`

This is equivalent to `\--_`opt_name`_` on the command line. 

* `_`opt_name`_=_`value`_`

This is equivalent to `\--_`opt_name`_=_`value`_` on the command line. In an option file, you can have spaces around the `=` character, something that is not true on the command line. The value optionally can be enclosed within single quotation marks or double quotation marks, which is useful if the value contains a `#` comment character. 

Leading and trailing spaces are automatically deleted from option names and values. 

You can use the escape sequences `b`, `t`, `n`, `r`, `\`, and `s` in option values to represent the backspace, tab, newline, carriage return, backslash, and space characters. In option files, these escaping rules apply: 

* A backslash followed by a valid escape sequence character is converted to the character represented by the sequence. For example, `s` is converted to a space. 
* A backslash not followed by a valid escape sequence character remains unchanged. For example, `S` is retained as is. 

The preceding rules mean that a literal backslash can be given as `\`, or as `` if it is not followed by a valid escape sequence character. 

The rules for escape sequences in option files differ slightly from the rules for escape sequences in string literals in SQL statements. In the latter context, if "_`x`_" is not a valid escape sequence character, `_`x`_` becomes "_`x`_" rather than `_`x`_`. See [Section 9.1.1, "String Literals"][15]. 

The escaping rules for option file values are especially pertinent for Windows path names, which use `` as a path name separator. A separator in a Windows path name must be written as `\` if it is followed by an escape sequence character. It can be written as `\` or `` if it is not. Alternatively, `/` may be used in Windows path names and will be treated as ``. Suppose that you want to specify a base directory of `C:Program FilesMySQLMySQL Server 5.7` in an option file. This can be done several ways. Some examples: 
    
    
    basedir="C:Program FilesMySQLMySQL Server 5.7"
    basedir="C:\Program Files\MySQL\MySQL Server 5.7"
    basedir="C:/Program Files/MySQL/MySQL Server 5.7"
    basedir=C:\ProgramsFiles\MySQL\MySQLsServers5.7

If an option group name is the same as a program name, options in the group apply specifically to that program. For example, the `[mysqld]` and `[mysql]` groups apply to the **[mysqld**][1] server and the **[mysql**][6] client program, respectively. 

The `[client]` option group is read by all client programs provided in MySQL distributions (but _not_ by **[mysqld**][1]). To understand how third-party client programs that use the C API can use option files, see the C API documentation at [Section 27.8.7.50, "mysql_options()"][16]. 

The `[client]` group enables you to specify options that apply to all clients. For example, `[client]` is the appropriate group to use to specify the password for connecting to the server. (But make sure that the option file is accessible only by yourself, so that other people cannot discover your password.) Be sure not to put an option in the `[client]` group unless it is recognized by _all_ client programs that you use. Programs that do not understand the option quit after displaying an error message if you try to run them. 

List more general option groups first and more specific groups later. For example, a `[client]` group is more general because it is read by all client programs, whereas a `[mysqldump]` group is read only by **[mysqldump**][17]. Options specified later override options specified earlier, so putting the option groups in the order `[client]`, `[mysqldump]` enables **[mysqldump**][17]-specific options to override `[client]` options. 

Here is a typical global option file: 
    
    
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

Here is a typical user option file: 
    
    
    [client]
    # The following password will be sent to all standard MySQL clients
    password="my password"
    
    [mysql]
    no-auto-rehash
    connect_timeout=2

To create option groups to be read only by **[mysqld**][1] servers from specific MySQL release series, use groups with names of `[mysqld-5.6]`, `[mysqld-5.7]`, and so forth. The following group indicates that the `[sql_mode`][18] setting should be used only by MySQL servers with 5.7.x version numbers: 
    
    
    [mysqld-5.7]
    sql_mode=TRADITIONAL

It is possible to use `!include` directives in option files to include other option files and `!includedir` to search specific directories for option files. For example, to include the `/home/mydir/myopt.cnf` file, use the following directive: 
    
    
    !include /home/mydir/myopt.cnf

To search the `/home/mydir` directory and read option files found there, use this directive: 
    
    
    !includedir /home/mydir

MySQL makes no guarantee about the order in which option files in the directory will be read. 

Note 

Any files to be found and included using the `!includedir` directive on Unix operating systems _must_ have file names ending in `.cnf`. On Windows, this directive checks for files with the `.ini` or `.cnf` extension. 

Write the contents of an included option file like any other option file. That is, it should contain groups of options, each preceded by a `[_`group`_]` line that indicates the program to which the options apply. 

While an included file is being processed, only those options in groups that the current program is looking for are used. Other groups are ignored. Suppose that a `my.cnf` file contains this line: 
    
    
    !include /home/mydir/myopt.cnf

And suppose that `/home/mydir/myopt.cnf` looks like this: 
    
    
    [mysqladmin]
    force
    
    [mysqld]
    key_buffer_size=16M

If `my.cnf` is processed by **[mysqld**][1], only the `[mysqld]` group in `/home/mydir/myopt.cnf` is used. If the file is processed by **[mysqladmin**][7], only the `[mysqladmin]` group is used. If the file is processed by any other program, no options in `/home/mydir/myopt.cnf` are used. 

The `!includedir` directive is processed similarly except that all option files in the named directory are read. 

If an option file contains `!include` or `!includedir` directives, files named by those directives are processed whenever the option file is processed, no matter where they appear in the file. 

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

  