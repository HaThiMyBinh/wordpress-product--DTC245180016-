# Website Quảng bá Sản phẩm

## 1. Giới thiệu dự án

Dự án xây dựng website quảng bá sản phẩm bằng WordPress trên Ubuntu Server. Hệ thống sử dụng Docker và Docker Compose để triển khai, quản lý và vận hành các dịch vụ.

Ngoài chức năng giới thiệu sản phẩm, dự án còn triển khai HTTPS, bảo mật, giám sát hệ thống và quản lý nhật ký.

## 2. Công nghệ sử dụng

- **Ubuntu Server:** Hệ điều hành triển khai.
- **Docker và Docker Compose:** Đóng gói và quản lý dịch vụ.
- **WordPress:** Xây dựng website.
- **MySQL 8.0:** Lưu trữ dữ liệu.
- **phpMyAdmin:** Quản lý cơ sở dữ liệu.
- **Nginx:** Reverse proxy và HTTPS.
- **Prometheus:** Thu thập số liệu giám sát.
- **Blackbox Exporter:** Kiểm tra khả dụng website qua HTTPS.
- **cAdvisor:** Giám sát tài nguyên container.
- **MySQL Exporter:** Cung cấp metric của MySQL.
- **Grafana:** Hiển thị dashboard.
- **Loki và Promtail:** Thu thập và truy vấn log.

## 3. Chức năng hệ thống

- Hiển thị thông tin và hình ảnh sản phẩm.
- Quản lý nội dung bằng WordPress.
- Lưu trữ dữ liệu trong MySQL.
- Quản lý cơ sở dữ liệu bằng phpMyAdmin.
- Sử dụng Nginx làm reverse proxy HTTPS.
- Giám sát WordPress, container và MySQL.
- Thu thập, lưu trữ và truy vấn log bằng Loki và Promtail.

## 4. Cấu trúc thư mục

```text
wordpress-product/
├── docker-compose.yml
├── .env.example
├── .gitignore
├── README.md
├── blackbox/
├── loki/
├── nginx/
│   ├── conf.d/
│   └── certs/
├── php/
├── prometheus/
├── promtail/
└── monitoring/
    └── mysql-exporter/
        └── .my.cnf
```

File `monitoring/mysql-exporter/.my.cnf` chỉ được tạo trên máy triển khai và không được đưa lên GitHub vì chứa thông tin xác thực.

## 5. Yêu cầu môi trường

- Ubuntu Server.
- Docker.
- Docker Compose.
- Máy tính có thể kết nối đến địa chỉ IP máy chủ.

Kiểm tra phiên bản:

```bash
docker --version
docker-compose --version
```

## 6. Cấu hình môi trường

Tạo file `.env` từ file mẫu:

```bash
cp .env.example .env
nano .env
```

Điền các biến môi trường cần thiết, bao gồm thông tin MySQL và Grafana nếu được sử dụng.

Tạo file cấu hình MySQL Exporter:

```bash
mkdir -p monitoring/mysql-exporter
nano monitoring/mysql-exporter/.my.cnf
```

Nội dung mẫu:

```ini
[client]
user=mysqld_exporter
password=MAT_KHAU_EXPORTER
host=db
port=3306
```

Tài khoản `mysqld_exporter` chỉ nên được cấp các quyền cần thiết để đọc metric MySQL.

**Lưu ý:** Không đưa `.env`, `.my.cnf` hoặc mật khẩu thật lên GitHub.

## 7. Khởi chạy hệ thống

Di chuyển vào thư mục dự án:

```bash
cd ~/wordpress-product
```

Kiểm tra cấu hình:

```bash
docker-compose config
```

Khởi chạy các dịch vụ chính:

```bash
docker-compose up -d
```

Kiểm tra trạng thái container:

```bash
docker-compose ps
```

Xem log:

```bash
docker-compose logs --tail=100
```

Kiểm tra cấu hình Prometheus:

```bash
docker exec wordpressproduct_prometheus \
  promtool check config /etc/prometheus/prometheus.yml
```

## 8. Truy cập website và công cụ quản trị

Thay `192.168.101.128` bằng địa chỉ IP thực tế của Ubuntu Server nếu cần.

| Dịch vụ | Địa chỉ truy cập |
|---|---|
| WordPress | http://192.168.101.128:8092 |
| phpMyAdmin | http://192.168.101.128:8093 |
| WordPress qua Nginx HTTPS | https://192.168.101.128:8443 |
| Prometheus | http://192.168.101.128:9092 |
| Grafana | http://192.168.101.128:3001 |
| Loki API | http://192.168.101.128:3100 |

Chứng chỉ HTTPS hiện là chứng chỉ tự ký nên trình duyệt có thể hiển thị cảnh báo trong môi trường thử nghiệm.

## 9. Bảo mật hệ thống

- Không công khai mật khẩu hoặc thông tin xác thực.
- Không đưa `.env` và `.my.cnf` lên repository.
- Chỉ công khai các cổng cần thiết.
- Sử dụng HTTPS cho truy cập website.
- Sử dụng mạng `backend` ở chế độ internal cho cơ sở dữ liệu.
- MySQL Exporter chỉ được dùng tài khoản có quyền giới hạn.
- Kiểm tra trạng thái container định kỳ.
- Không đưa thư mục backup lên GitHub.

## 10. Giám sát hệ thống

### Prometheus và Grafana

Prometheus thu thập số liệu từ các target:

- Prometheus.
- WordPress qua Blackbox Exporter.
- cAdvisor.
- MySQL Exporter.

Các truy vấn PromQL đã sử dụng:

```promql
up
```

Truy vấn này kiểm tra trạng thái các target. Giá trị `1` cho biết target đang hoạt động.

```promql
mysql_up
```

Truy vấn này kiểm tra trạng thái kết nối của MySQL Exporter. Giá trị `1` cho biết exporter kết nối được đến MySQL.

Grafana sử dụng Prometheus làm nguồn dữ liệu và hiển thị:

- WordPress Website Availability.
- cAdvisor Status.
- Container CPU Usage.
- Container Memory Usage.
- MySQL Exporter Status.

### Loki và Promtail

Promtail thu thập log container và gửi đến Loki. Grafana sử dụng Loki làm nguồn dữ liệu log để truy vấn bằng LogQL.

Các truy vấn LogQL đã sử dụng:

```logql
{service=~".+"}
```

Truy vấn toàn bộ log của các service.

```logql
{service=~".+"} |~ "(?i)error|warn"
```

Lọc các log có nội dung `error` hoặc `warn`.

```logql
{service="loki"}
```

Lọc log riêng của Loki.

Trong môi trường kiểm thử, Grafana, Loki và Promtail có thể chạy dưới một stack monitoring riêng nếu không được khai báo trong `docker-compose.yml` chính của dự án.

### Kiểm tra monitoring

```bash
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
```

Kiểm tra các target trên giao diện:

```text
http://192.168.101.128:9092/targets
```

Các target hợp lệ cần có trạng thái `UP`.

## 11. Lịch sử triển khai

Dự án được triển khai qua các mốc chính:

1. **Website và cơ sở dữ liệu:** Hoàn thiện WordPress, MySQL và phpMyAdmin.
2. **Reverse proxy và HTTPS:** Bổ sung Nginx, chứng chỉ HTTPS và security headers.
3. **Monitoring cơ bản:** Bổ sung Prometheus, Blackbox Exporter và cAdvisor.
4. **Quản lý log:** Bổ sung Loki, Promtail và truy vấn LogQL.
5. **Hardening:** Bổ sung cấu hình bảo mật PHP, Nginx và sửa Prometheus target.
6. **Giám sát MySQL:** Bổ sung MySQL Exporter và metric `mysql_up`.
7. **Tài liệu triển khai:** Cập nhật README, hướng dẫn kiểm tra và truy cập hệ thống.

Mỗi commit cần có thông điệp rõ ràng và chứa các file thay đổi tương ứng.

## 12. Kết luận

Dự án kết hợp website WordPress với MySQL, phpMyAdmin, Nginx HTTPS, Prometheus, MySQL Exporter, Grafana, Loki và Promtail.

Repository lưu trữ mã nguồn và các cấu hình cần thiết, đồng thời cung cấp hướng dẫn triển khai, kiểm tra monitoring, truy vấn metric và truy vấn log.
