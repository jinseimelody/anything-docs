# I. Authentication và Authorization
**Authentication**: hay `xác thực` là xác nhận danh tính của riêng bạn vd (tên người dùng, id, password...)

**Authorization**: hay `ủy quyền` là kiểm tra xem bạn đóng vai trò gì trong hệ thống (admin, customer...) từ đó hạn chế việc truy cập đến những enpoint không được phép

Note: bài viết này chỉ tập trung đề cập đến **Authentication**

# II. Các phương thức xác thực thường gặp

## 1. Session - Cookies
Quy trình xác thực với session xảy ra như sau

1. Client gửi thông tin đăng nhập (username, password) lên server
2. Server kiểm tra thông tin đăng nhập

    `Thông tin đăng nhập không hợp lệ`: trả về lỗi (401 Unauthorize)

    `Ok`: chuyển sang bước kế tiếp
3. Server lưu thông tin người dùng vào memory và trả ra một `session_id` tương ứng để có thể truy cập thông tin này
4. Server gửi `session_id` về lại cho client
7. Client lưu `session_id` được trả về vào cookies

    **Note**: When using cookies, the server asks the client to store a cookie by setting the Set-Cookie HTTP response header

8. Sau khi đăng nhập thì mỗi khi client request tới 1 enpoint trên server nó sẽ kẹp theo `session_id` này trong request header

    **Note**: The cookie is then sent back to the server by the client using the Cookie HTTP request header on each request and thus the server is informed on each request the session ID currently assigned to the client.

# 2. Token
Quy trình xác thực với session xảy ra như sau

1. Client gửi thông tin đăng nhập (username, password) lên server
2. Server kiểm tra thông tin đăng nhập

    Thông tin đăng nhập không hợp lệ: trả về lỗi (401 Unauthorize)

    Ok: chuyển sang bước kế tiếp
3. Server tạo ra `access_token` và `refresh_token` gửi về client
4. client lưu token được cấp thông thường sẽ lưu trong localstorage
5. Sau khi đăng nhập thì mỗi khi gửi request user sẽ gửi kèm access token trong authorization request header
