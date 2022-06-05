# 1. Q/A
## Reset mysql root password
1. tạo file ini với nội dung sau (file này sẽ được chạy khi start mysql) và lưu vào `C:\\mysql-init.txt`
    ```
    ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'new_password';

    ```
    Note: thay `new_password` thành password thành mật khẩu mới mà bạn muốn đặt.

2. truy cập thư mục mysql/bin và chạy lệnh sau
    ```
    mysqld --defaults-file="C:\\ProgramData\\MySQL\\MySQL Server 8.0\\my.ini" --init-file=C:\\mysql-init.txt
    ```