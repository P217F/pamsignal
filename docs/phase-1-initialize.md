# Bước 1 - Initialize (Khởi tạo dự án)

- **Trạng thái**: Hoàn thành
- **Ngày thực hiện**: 26/12/2025
- **Ngày hoàn thành**: 26/12/2025

## 1. Mục tiêu

Thiết lập nền móng cho dự án **PAMSignal**. Đảm bảo mã nguồn được tổ chức khoa học, dễ mở rộng và có quy trình biên dịch tự động hóa, tối ưu hiệu suất.

## 2. Yêu cầu

**Hệ điều hành**

Do dự án tập trung vào linux và cần include thư viện *libsystemd*  nên để có thể chạy project này, bạn cần dùng hệ điều hành Linux, chọn đại một distro như Ubuntu chẳng hạn.

**Gói phụ thuộc**

```bash
sudo apt update
sudo apt install libsystemd-dev pkg-config build-essential cmake
```

Sau khi cài đặt xong thì bạn có thể clone dự án về máy và chạy thử lệnh `make` , output sẽ là file thực thi `pamsignal`

```bash
git clone git@github.com:anhtuank7c/pamsignal.git
cd pamsignal
make
```

## 3. Cấu trúc thư mục

```
pamsignal
    src/            Chứa mã thực thi (.c)
    include/        Chứa các file header (.h) để quản lý interface.
    Makefile        Quản lý quy trình build.
    docs/           Quản lý tài liệu
```

## 4. Tối ưu hóa biên dịch với Makefile

Trình biên dịch C có vài options liên quan tới việc tối ưu hóa mã máy [chi tiết xem tại đây](https://gcc.gnu.org/onlinedocs/gcc-15.1.0/gcc/Optimize-Options.html)

Tôi sử dụng flag tối ưu hóa `-O2` trong quá trình biên dịch.

**Tại sao -O2?**

Đây là mức tối ưu giúp trình biên dịch sắp xếp lại các lệnh máy, loại bỏ code thừa, tăng tốc độ xử lý log nhưng không làm phình kích thước file như `-O3`.

## 5. Kết quả

- [x] Khởi tạo thành công cấu trúc thư mục.

- [x] Makefile hoạt động tốt, nhận diện đúng thư viện `libsystemd`.

- [x] Biên dịch thành công file thực thi đầu tiên (Sanity Check).

