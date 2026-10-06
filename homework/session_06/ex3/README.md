# Session 06 - Exercise 3

## Cấu hình tường lửa UFW và chẩn đoán cổng mạng

### 1. Mục tiêu

* Cấu hình UFW trên Ubuntu.
* Mặc định từ chối tất cả kết nối vào (incoming).
* Cho phép tất cả kết nối ra ngoài (outgoing).
* Cho phép kết nối SSH qua port 22.
* Cho phép ứng dụng Web qua port 8080/TCP.
* Kiểm tra trạng thái của UFW.
* Kiểm tra các cổng đang lắng nghe bằng lệnh `ss`.

---

### 2. Môi trường thực hiện

* Hệ điều hành: Ubuntu
* Môi trường: WSL2
* User: `nlinhh`

---

### 3. Kiểm tra trạng thái UFW ban đầu

Lệnh:

```bash
sudo ufw status verbose
```

Kết quả:

```text
Status: inactive
```

Điều này cho biết UFW đã được cài đặt nhưng firewall chưa được kích hoạt.

---

### 4. Cấu hình chính sách mặc định

Đặt chính sách mặc định từ chối kết nối vào:

```bash
sudo ufw default deny incoming
```

Kết quả:

```text
Default incoming policy changed to 'deny'
```

Cho phép các kết nối đi ra:

```bash
sudo ufw default allow outgoing
```

Kết quả:

```text
Default outgoing policy changed to 'allow'
```

Như vậy:

* Incoming: `deny`
* Outgoing: `allow`

---

### 5. Cho phép port SSH 22

Lệnh:

```bash
sudo ufw allow 22/tcp
```

Kết quả:

```text
Rules updated
Rules updated (v6)
```

Port 22 được cho phép để hỗ trợ kết nối SSH.

---

### 6. Cho phép port Web 8080

Lệnh:

```bash
sudo ufw allow 8080/tcp
```

Kết quả:

```text
Rules updated
Rules updated (v6)
```

Port 8080 được cho phép để phục vụ ứng dụng Web chạy trên giao thức TCP.

---

### 7. Kích hoạt UFW

Lệnh:

```bash
sudo ufw enable
```

Kết quả:

```text
Firewall is active and enabled on system startup
```

Điều này cho biết UFW đã được kích hoạt và sẽ tự động khởi động cùng hệ thống.

---

### 8. Kiểm tra cấu hình UFW

Lệnh:

```bash
sudo ufw status verbose
```

Kết quả:

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
8080/tcp                   ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
8080/tcp (v6)              ALLOW IN    Anywhere (v6)
```

Kết quả cho thấy:

* UFW đang ở trạng thái `active`.
* Các kết nối vào mặc định bị từ chối.
* Các kết nối ra ngoài được cho phép.
* Port `22/tcp` được phép cho SSH.
* Port `8080/tcp` được phép cho ứng dụng Web.
* Các rule cũng được áp dụng cho IPv6.

---

### 9. Kiểm tra các cổng đang lắng nghe

Lệnh:

```bash
ss -tlnp
```

Kết quả thực tế:

```text
State        Recv-Q        Send-Q        Local Address:Port        Peer Address:Port
LISTEN       0             1000          10.255.255.254:53        0.0.0.0:*
LISTEN       0             4096          127.0.0.53%lo:53         0.0.0.0:*
LISTEN       0             4096          127.0.0.54:53            0.0.0.0:*
```

Kết quả cho thấy trong môi trường Ubuntu WSL2 hiện tại đang có các dịch vụ DNS lắng nghe trên port `53`.

Không có dịch vụ nào đang lắng nghe trên port `8080`.

Điều này không có nghĩa cấu hình UFW bị lỗi. Rule UFW `8080/tcp` chỉ cho phép kết nối đến port 8080 khi có một ứng dụng thực sự chạy và lắng nghe trên port đó.

---

### 10. Kết luận

Đã hoàn thành các yêu cầu của Exercise 3:

* UFW được cài đặt và kích hoạt.
* Incoming mặc định là `deny`.
* Outgoing mặc định là `allow`.
* Port `22/tcp` được phép.
* Port `8080/tcp` được phép.
* Đã kiểm tra trạng thái UFW bằng `sudo ufw status verbose`.
* Đã kiểm tra các cổng đang lắng nghe bằng `ss -tlnp`.

Firewall đã được cấu hình đúng theo yêu cầu của bài tập.
