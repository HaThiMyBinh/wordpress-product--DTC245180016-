# Website Quảng bá Sản phẩm

## 1. Giới thiệu dự án

Dự án xây dựng website quảng bá sản phẩm bằng WordPress trên Ubuntu Server. Hệ thống sử dụng Docker và Docker Compose để triển khai các dịch vụ, giúp việc cài đặt, quản lý và vận hành thuận tiện hơn.

Ngoài chức năng giới thiệu sản phẩm, dự án hướng đến việc tăng cường bảo mật, giám sát hoạt động và quản lý nhật ký hệ thống.

## 2. Công nghệ sử dụng

- **Ubuntu Server:** Hệ điều hành triển khai.
- **Docker:** Đóng gói và chạy các dịch vụ trong container.
- **Docker Compose:** Quản lý các dịch vụ của hệ thống.
- **WordPress:** Xây dựng và quản lý nội dung website.
- **MySQL 8.0:** Lưu trữ dữ liệu website.
- **phpMyAdmin:** Quản lý cơ sở dữ liệu thông qua giao diện web.
- **Nginx:** Reverse proxy và tiếp nhận kết nối HTTPS.
- **Prometheus:** Thu thập và lưu trữ số liệu giám sát.
- **Grafana:** Hiển thị dashboard giám sát.
- **Loki và Promtail:** Thu thập, lưu trữ và truy vấn nhật ký hệ thống.

## 3. Chức năng hệ thống

- Hiển thị thông tin và hình ảnh sản phẩm.
- Quản lý nội dung website bằng WordPress.
- Lưu trữ dữ liệu trong MySQL.
- Quản lý cơ sở dữ liệu bằng phpMyAdmin.
- Cấu hình Nginx và HTTPS để tăng cường bảo mật truy cập.
- Giám sát hệ thống bằng Prometheus và Grafana.
- Thu thập, quản lý và truy vấn log bằng Loki và Promtail.

## 4. Cấu trúc thư mục

```text
wordpress-product/
├── docker-compose.yml
├── .env.example
├── .gitignore
├── README.md
├── nginx/
│   ├── conf.d/
│   └── certs/
├── prometheus/
├── grafana/
├── loki/
├── promtail/
└── mysql/
```

Các thư mục cấu hình cần được bổ sung theo cấu hình triển khai thực tế của dự án.

## 5. Yêu cầu môi trường

- Ubuntu Server.
- Docker.
- Docker Compose.
- Máy tính có thể kết nối đến địa chỉ IP của máy chủ.

Kiểm tra phiên bản Docker:

```bash
docker --version
docker-compose --version
```

## 6. Cấu hình môi trường

Tạo file `.env` tại thư mục gốc dự án từ file cấu hình mẫu `.env.example`.

```bash
cp .env.example .env
nano .env
```

Điền các biến môi trường cần thiết, bao gồm mật khẩu MySQL và mật khẩu quản trị Grafana nếu được sử dụng trong cấu hình.

**Lưu ý:** Không sử dụng mật khẩu mẫu trong môi trường thực tế. Không tải file `.env` chứa mật khẩu thật lên GitHub.

## 7. Khởi chạy hệ thống

Di chuyển vào thư mục dự án:

```bash
cd ~/wordpress-product
```

Kiểm tra cấu hình Docker Compose:

```bash
docker-compose config
```

Chạy các dịch vụ:

```bash
docker-compose up -d
```

Kiểm tra trạng thái container:

```bash
docker-compose ps
```

Xem nhật ký dịch vụ khi cần kiểm tra lỗi:

```bash
docker-compose logs --tail=100
```

## 8. Truy cập website và công cụ quản trị

Thay `IP_SERVER` bằng địa chỉ IP thực tế của Ubuntu Server.

| Dịch vụ | Địa chỉ truy cập |
|---|---|
| WordPress | `http://IP_SERVER:8092` |
| phpMyAdmin | `http://IP_SERVER:8093` |
| Prometheus | Theo cổng được cấu hình |
| Grafana | Theo cổng được cấu hình |
| Nginx HTTPS | Theo cổng HTTPS được cấu hình |

Các cổng của Prometheus, Grafana và Nginx cần được đối chiếu với cấu hình thực tế trước khi sử dụng.

## 9. Bảo mật hệ thống

- Không công khai mật khẩu hoặc thông tin xác thực.
- Không đưa file `.env` chứa dữ liệu nhạy cảm lên repository.
- Chỉ công khai những cổng dịch vụ cần thiết.
- Sử dụng HTTPS khi cấu hình Nginx hoàn tất.
- Kiểm tra quyền truy cập và trạng thái các container định kỳ.

## 10. Giám sát hệ thống

### Prometheus và Grafana

Prometheus được sử dụng để thu thập các số liệu giám sát theo cấu hình. Grafana cung cấp giao diện dashboard giúp theo dõi tình trạng hệ thống.

### Loki và Promtail

Promtail được sử dụng để thu thập log và gửi đến Loki. Người quản trị có thể sử dụng LogQL để truy vấn log phục vụ kiểm tra và xử lý sự cố.

Các nguồn dữ liệu, dashboard và truy vấn cụ thể phụ thuộc vào cấu hình đã triển khai.

## 11. Lịch sử triển khai

Dự án được chia thành ba mốc công việc chính:

1. **Commit 1 – Website và bảo mật:** Hoàn thiện cấu hình WordPress, MySQL, phpMyAdmin, Nginx và HTTPS.
2. **Commit 2 – Giám sát:** Bổ sung cấu hình Prometheus và Grafana.
3. **Commit 3 – Quản lý log:** Bổ sung cấu hình Loki, Promtail và các truy vấn LogQL.

Mỗi commit cần chứa các file thay đổi tương ứng và có thông điệp mô tả rõ nội dung công việc.

## 12. Kết luận

Dự án kết hợp website WordPress với cơ sở dữ liệu MySQL, reverse proxy, HTTPS, giám sát và quản lý log. Repository lưu trữ mã nguồn cùng cấu hình triển khai để hỗ trợ cài đặt, kiểm tra và phát triển hệ thống.
