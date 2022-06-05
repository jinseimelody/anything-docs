# I. Cryptography

Mã hóa là cần thiết để thông tin trao đổi trên internet được an toàn hơn, có 2 kiểu mã hóa:
1. `Symmetric Key`: sủ dụng 1 key duy nhất cho mã hóa và giải mã.
2. `Asymmetric Key`: sử dụng 2 key riêng biệt cho quá trình mã hóa và giải mã (RSA, DSA và PKCS..)
   -  **public key** dùng cho mã hóa dữ liệu
   -  **private key** dùng cho giải mã
3. `Hash Functions`: [tham khảo](https://codelearn.io/sharing/hash-la-gi-va-hash-dung-de-lam-gi) (MD5, SHA-256)

# II. TSL and SSL certificate

1. `SSL, TSL protocol`: giao thức để tăng tính bảo mật cho các giao tiếp qua internet (P/s: hiện tại phần lớn đã chuyển đổi sang TSL).
2. `SSL certificate`: là một file chứa các thông tin server vd: tên, địa chỉ.. đại loại giống CMND và trên hết **cái gì cũng có thể fake được** do đó chưa thể đảm bảo ngay certificate server gửi về là "hàng xịn" nên cần được mang đi kiểm tra (đề cập ở phần sau).

# III. Https protocol

Mã hóa trong https protocol sử dụng mã hóa `Asymmetric Key` với thuật toán [RSA](https://vi.wikipedia.org/wiki/RSA_(m%C3%A3_h%C3%B3a))

Truyền tải dữ liệu bao gồm 3 bước sau:
1. `ssl handshake`: quá trình handshake diễn ra như sau
   - Client: [ send ] `client random` (một chuỗi byte ngẩu nhiên).
   - Server: [ send ] `ssl certificate` (đã bao gồm `public key`), `server random`.
   - Client: [ check ] `ssl certificate` này kiểm tra tại chỗ phát hành xem nó có phải "hành xịn" không bằng cách kiểm tra CA (Certificate authority) sẽ được đề cập tại mục IV.
   - Client: [ send ] `premaster secret` được mã hóa bởi public key có được từ quá trình handshake.
   - Server: [ receive ] sử dụng `private key` giải mã để lấy `premaster secret` tại thời điểm này cả client và server đề có 3 thứ sau.
   - Both: `client random`, `server random`, `premaster secret` ở cả 2 phía dùng để tạo ra `Session key`.
   - Both: client và server lần lượt gửi message "finish" tới nhau message này được mã hóa bởi `Session key`.
2. `receive - send`:
   - trao đổi dữ liệu bình thường nhưng trước khi gửi đi sẽ được mã hóa bởi `Sesstion key`

# IV. Part of a ssl certificate

CA: Certificate Authority hiểu đơn giản là nơi cấp chứng chỉ ssl cho bạn, có 2 loại CA:
- public hay gobal CA dùng trong internet, CA này tồn tại trên mọi thiết bị
- private hay internal CA được dùng trong mạng nội bộ


**Các thông số cần được quan tâm khi đọc một cert**

`Subject Alternative Name`

```
DNS Name = the-digital-life.com
DNS Name = *.digital-life.com
DNS Name = sni.cloudflaressl.com
```
Cho biết chứng chỉ này hợp lệ cho những domain nào, khi người dùng truy cập trang brower sẽ kiểm tra xem certificate và url có đến từ cùng 1 domain hay không.

`Certification Path`
```
DigiCert Baltimore Root
   Cloudflare Inc ECC CA-3
      sni.cloudflaressl.com
```
Để chắc chắn certificate là hợp lệ certificate cần pass việc kiểm tra CA, quá trình kiểm tra sẽ diễn ra theo trình tự từ ngọn đến gốc của `Certification Path`, `sni.cloudflaressl.com` -> `Cloudflare Inc ECC CA-3` -> `DigiCert Baltimore Root`.

# V. How to create a https server

## Prerequiresite

Cài đặt `openssl`
  1. Linux: openssl được cài đặt mặc định
  2. Windows: Cài đặt Git Bash (openssl kèm theo)

## Create CA Cert - SSL Cert

### 1. Tạo private CA
   ```
   <!-- tạo rsa private key 4069 bit mã hóa nó bằng thuật toán aes256 và lưu vào file có tên ca-key.pem -->
   openssl genrsa -aes256 -out ca-key.pem 4069

   <!-- tạo chứng chỉ với định dạng x509 mới với mã hóa sha256 có hiệu lực trong vòng 3650 ngày (10 năm), chỉ định private key là file ca-key.pem và lưu vào file ca.pem -->
   openssl req -new -x509 -sha256 -days 3650 -key ca-key.pem -out ca.pem

   <!-- Kiểm tra thông tin cert vừa tạo bằng lệnh sau -->
   openssl x509 -in ca.pem -text
   ```
   `Note`: 

### 2. Tạo và ký ssl certificate 
   ```
   <!-- tạo private key -->
   openssl genrsa -out cert-key.pem

   <!-- tạo certificate signing request và chỉ định private key-->
   openssl req -new -sha256 -subj -key cert-key.pem -out cert.csr

   <!-- tạo file config -->
   echo "subjectAltName=DNS:*.coetorise.home,IP:192.168.1.10" >> extfile.cnf

   <!-- tạo ssl certificate -->
   openssl x509 -req -sha256 -days 3650 -in cert.csr -CA ca.pem -CAkey ca-key.pem -extfile extfile.cnf -out cert.pem -CAcreateserial
   
   <!-- tạo fullchain cert -->
   cat cert.pem > fullchain.pem
   cat ca.pem >> fullchain.pem
   ```

## Install the CA Cert as a trusted root CA
   ### On windows  
```
   Import-Certificate -FilePath "C:\Certificate\ca.pem" -CertStoreLocation Cert:\LocalMachine\Root
```

Openssl docs
[xem thêm](https://www.openssl.org/docs/man1.1.1/man1/)