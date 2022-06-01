**[Q]**: Giao thức https và Jwt đang hiểu là điều dùng để tránh việc bị tấn công MitM vậy khi sử dụng https thì có cần thiết sử dụng jwt hay không
**[A]**:
- Trong trường hợp này jwt không cần thiết lắm khi bạn cài đặt một https server bởi vì về giao thức https chắc chắn rằng quá trình truyền tải thông tin giữa client và server luôn được mã hóa từ cả 2 phía [xem thêm](https://stackoverflow.com/questions/45978013/is-jwt-necessary-over-https-communication).
- Trong trường hợp bạn không có 1 https server thì việc sử dụng jwt là cần thiết.

**[Q]**: jwt và oauth khác nhau như thế nào
- Đầu tiên hãy nói một chút về quá trình đăng nhập có 2 cách để thực hiện
  - Session/Cookie đối với các website truyền thống
  - Jwt, Oauth đối với các API