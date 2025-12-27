# Journal Subscriber

- **Trạng thái**: Đang thực hiện
- **Ngày thực hiện**: 27/12/2025
- **Ngày hoàn thành**: 

## 1. Mục tiêu

- **Event-driven**: thay thế việc quét file log truyền thống bằng việc đăng ký nhận sự kiện từ `libsystemd` giúp PAMSignal phản ứng ngay lập tức khi có log mới mà không lãng phí tài nguyên khi không cần dùng đến.
- **Real-time**: Tận dụng hàm `sd_journal_wait` để đạt được độ trễ `~0s` từ khi hệ thống ghi nhận phiên đăng nhập đến khi PAMSignal bắt đầu xử lý dữ liệu.
- **Structured data**: Khai thác dạng nhị phân của Journal để truy xuất các trường metadata như `MESSAGE`, `_PID` , `_UID` một cách chính xác.

## 2. Tìm hiểu về systemd, journal

Trước đây tôi chỉ có ý tưởng cực kỳ đơn giản là quét file `/var/log/auth.log` rồi sau đó bóc tách chuỗi để lấy thông tin phiên đăng nhập. Việc này tuy có thể chứng minh được ý tưởng "cảnh báo phiên đăng nhập" là khả thi nhưng về mặt hiệu năng, độ ổn định cũng như độ tin cậy không cao.

**Cụ thể:**

- Việc xử lý từng dòng log thì dạng thời gian, pattern message rất khác nhau trên mỗi phiên bản hệ điều hành dẫn tới việc trích xuất thông tin thiếu độ tin cậy. 

- Pulling định kỳ mỗi `500ms` gây lãng phí tài nguyên, trong một ngày không có quá nhiều phiên đăng nhâp thành công vào hệ thống nên việc làm này là không tốt.
- Nếu hacker đã vào được trong hệ thống và có action xóa dấu vết trong file auth.log trong vòng 500ms thì hệ thống hoàn toàn không nhận ra.

Một người bạn trong network của tôi là **Nguyễn Hồng Quân** có chia sẻ là nên tìm hiểu systemd, journal, bạn nói rằng những log file kia chỉ là tính năng mang tính tương thích ngược, hỗ trợ các hệ thống cũ, vậy là tôi bắt tay vào tìm hiểu.

### 2.1 systemd là gì?