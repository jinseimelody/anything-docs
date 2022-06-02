**I. Cryptography**

Mã hóa là cần thiết để thông tin trao đổi trên internet được an toàn hơn, có 2 kiểu mã hóa:
1. `Symmetric Key`: sủ dụng 1 key duy nhất cho mã hóa và giải mã.
2. `Asymmetric Key`: sử dụng 2 key riêng biệt cho quá trình mã hóa và giải mã (RSA, DSA và PKCS..)
   -  **public key** dùng cho mã hóa dữ liệu
   -  **private key** dùng cho giải mã
3. `Hash Functions`: [tham khảo](https://codelearn.io/sharing/hash-la-gi-va-hash-dung-de-lam-gi) (MD5, SHA-256)

**I. Ssl protocol and ssl certificate**

1. `SSL, TSL protocol`: giao thức để tăng tính bảo mật cho các giao tiếp qua internet (P/s: hiện tại phần lớn đã chuyển đổi sang TSL).
2. `SSL certificate`: là một file chứa các thông tin server vd: tên, địa chỉ.. đại loại giống CMND và trên hết **cái gì cũng có thể fake được** do đó chưa thể đảm bảo ngay certificate server gửi về là "hàng xịn" nên cần được mang đi kiểm tra (đề cập ở phần sau).

**II. Https protocol**

Mã hóa trong https protocol sử dụng mã hóa `Asymmetric Key` với thuật toán [RSA](https://vi.wikipedia.org/wiki/RSA_(m%C3%A3_h%C3%B3a))

Truyền tải dữ liệu bao gồm 3 bước sau:
1. `ssl handshake`: quá trình handshake diễn ra như sau
   - Client: [ send ] `client random` (một chuỗi byte ngẩu nhiên).
   - Server: [ send ] `ssl certificate` (đã bao gồm `public key`), `server random`.
   - Client: [ check ] `ssl certificate` này kiểm tra tại chỗ phát hành xem nó có phải "hành xịn" không (điều đó đồng nghĩa với việc certificate phải được cấp bởi bên cung cấp dịch vụ thứ 3).
   - Client: [ send ] `premaster secret` được mã hóa bởi public key có được từ quá trình handshake.
   - Server: [ receive ] sử dụng `private key` giải mã để lấy `premaster secret` tại thời điểm này cả client và server đề có 3 thứ sau.
   - Both: `client random`, `server random`, `premaster secret` ở cả 2 phía dùng để tạo ra `Session key`.
   - Both: client và server lần lượt gửi message "finish" tới nhau message này được mã hóa bởi `Session key`.
2. `receive - send`:
   - trao đổi dữ liệu bình thường nhưng trước khi gửi đi sẽ được mã hóa bởi `Sesstion key`

**III. How to create a https server**

**Prerequiresite**

Cài đặt `openssl`

- Linux: openssl được cài đặt mặc định
- Windows: Cài đặt Git Bash (openssl kèm theo)

Sử dụng lệnh sau để tạo ra một ssl certificate

```
openssl req -x509 --days 365 --newkey rsa:4096 --keyout key.pem --out cert.pem 

```

Explanation:
- req: tạo mới certificate
- `--days 365` chỉ định số ngày khả dụng khi sử dụng `-x509`, khi không được chỉ định thì mặc định là 30 ngày
- `--newkey rsa:4096` chỉ định tạo mới private key 4096 bit sử dụng chuẩn mã hóa [RSA](https://vi.wikipedia.org/wiki/RSA_(m%C3%A3_h%C3%B3a))
- `--keyout key.pem` ghi private key được tạo ra file mới có tên key.pem
- `--out cert.pem` output ra file với tên `cert.pem`

Openssl docs
[xem thêm](https://www.openssl.org/docs/man1.1.1/man1/)