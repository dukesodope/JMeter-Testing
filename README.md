Trần Trung Đức - BIT220204 - 22SE1

# Kiểm thử phi chức năng với JMeter
## Giới thiệu
Dự án này sử dụng Apache JMeter để đánh giá hiệu suất của một trang web thương mại điện tử thông qua kiểm thử tải (Load Testing) và kiểm thử khả năng chịu tải (Stress Testing).

## Hướng dẫn sử dụng
### 1. Cài đặt
- Yêu cầu: 
    - Apache JMeter
    - Java 8 hoặc mới hơn.
## Cài đặt Apache JMeter
### Bước 1: Tải và cài đặt Java
JMeter yêu cầu Java để chạy. Kiểm tra phiên bản Java:
```
java -version
```
Nếu chưa có, tài và cài đặt từ trang web chính của Java: https://www.java.com

### Bước 2: Tải và cài đặt Apache JMeter
- Truy cập website của JMeter: https://jmeter.apache.org/download_jmeter.cgi
- Tải phiên bản mới nhất.
- Giải nén và chạy JMeter bằng lệnh:
```
cd apache-jmeter/bin
./jmeter.sh   # Mac/Linux
jmeter.bat    # Windows
```

## Mô tả bài tập
### 1. Kiểm thử hiệu suất
- Mô phỏng 50 người dùng đồng thời truy cập trang web trong vòng 5 phút.
- Gửi yêu cầu HTTP đến trang chủ của website.
- Đo thời gian phản hồi và xác định bottleneck nếu có.
### 2. Kiểm thử tải
- Chạy kiểm thử với số lượng người dùng tăng dần từ 10 → 50.
- Ghi nhận thời gian phản hồi trung bình và tỷ lệ lỗi (Error Rate).
### 3. Kiểm thử khả năng chịu tải
- Tiếp tục tăng số người dùng lên 100 và xác định giới hạn chịu tải của hệ thống.
- Quan sát thời điểm hệ thống bắt đầu giảm hiệu suất đáng kể.

## Kết quả 
### 1. Kiểm thử hiệu suất
### **Thời Gian Phản Hồi**

| Số request | Trung bình (ms) | Min (ms) | Max (ms) | Std. Dev. (ms) |
|------------|-----------------|----------|----------|----------------|
| 700 | 529 | 269 | 2928 | 390.8 |

### **Throughput & Dữ Liệu**

| Throughput (req/s) | Dữ liệu nhận (KB/s) | Dữ liệu gửi (KB/s) |
|--------------------|----------------------|----------------------|
| 1.72 req/s | 0.82 KB/s | 0.2 KB/s |

### **Tỷ Lệ Lỗi**

| Tỷ lệ lỗi (%) |
|--------------|
| 0.143% |

---
### 2. Kiểm thử chịu tải
### **Thời Gian Phản Hồi**

| Số request | Trung bình (ms) | Min (ms) | Max (ms) | Std. Dev. (ms) |
|------------|-----------------|----------|----------|----------------|
| 1700 | 505 | 264 | 3905 | 364.19 |

### **Throughput & Dữ Liệu**

| Throughput (req/s) | Dữ liệu nhận (KB/s) | Dữ liệu gửi (KB/s) |
|--------------------|----------------------|----------------------|
| 3.34 req/s | 1.6 KB/s | 0.38 KB/s |

### **Tỷ Lệ Lỗi**

| Tỷ lệ lỗi (%) |
|--------------|
| 0.118% |

---

### **📌 Kết Luận & Đề Xuất Cải Thiện**
✅ **Hệ thống hoạt động ổn định khi tăng tải từ 700 → 1700 request.**  
✅ **Throughput và khả năng xử lý dữ liệu tăng theo tải.**  
🚨 **Thời gian phản hồi tối đa tăng cao (~3.9s), cần tối ưu để tránh delay lớn.**  

**🔧 Giải pháp:**
- **Tối ưu truy vấn cơ sở dữ liệu hoặc API** để giảm độ trễ.
- **Tăng tài nguyên máy chủ (CPU, RAM) hoặc load balancing** nếu số request tiếp tục tăng.
- **Dùng caching (Redis, Memcached)** để giảm tải request trùng lặp.

---
### 3. Kiểm thử tải
### Tổng Quan Dữ Liệu
Bảng dưới đây tổng hợp các chỉ số hiệu suất chính khi kiểm thử với số lượng người dùng khác nhau.

| Users | Requests | Avg Response Time (ms) | Min Response Time (ms) | Max Response Time (ms) | Std Dev (ms) | Throughput (req/s) | Error Rate (%) |
|-------|----------|-----------------------|-----------------------|-----------------------|--------------|--------------------|---------------|
| 10    | 2        | 493.0                 | 272                   | 2350                  | 359.00        | 4.56663            | 0.000         |
| 30    | 2        | 669.0                 | 272                   | 2928                  | 535.04        | 1.11938            | 0.000         |
| 50    | 2        | 570.0                 | 269                   | 2928                  | 428.97        | 1.68838            | 0.222         |
| 100   | 2        | 505.0                 | 264                   | 3905                  | 364.19        | 3.34448            | 0.118         |

---

### Phân Tích Hiệu Suất

✅ **Thời Gian Phản Hồi:**
- Thời gian phản hồi trung bình **tăng** khi tải tăng từ 10 → 30 users (493ms → 669ms).
- Khi tăng lên 50 users, thời gian giảm nhẹ (570ms), có thể do hệ thống tối ưu hóa tài nguyên.
- Với 100 users, thời gian phản hồi giảm xuống 505ms, nhưng **thời gian phản hồi tối đa** tăng lên 3905ms, cho thấy dấu hiệu quá tải cục bộ.

✅ **Throughput:**
- Throughput cao nhất ở mức 10 users (4.57 req/s), sau đó **giảm mạnh** ở 30 users (1.12 req/s) và chỉ tăng nhẹ khi lên 50 users.
- Khi tải đạt 100 users, throughput tăng lên 3.34 req/s, nhưng không đạt mức tối ưu.

✅ **Tỷ Lệ Lỗi:**
- Không có lỗi ở mức 10 và 30 users.
- Khi tải đạt 50 users, tỷ lệ lỗi bắt đầu xuất hiện (0.222%).
- Với 100 users, tỷ lệ lỗi giảm nhẹ (0.118%), nhưng hệ thống có dấu hiệu chậm dần.

---

### **📌 Kết Luận & Đề Xuất Cải Thiện**
🚀 **Hệ thống hoạt động ổn định đến khoảng 30 users, nhưng xuất hiện độ trễ cao khi tải tăng lên 50+ users.**  
⚠️ **Throughput giảm mạnh ở 30 users, cho thấy hệ thống có thể đang bị nghẽn tài nguyên.**  

🔧 **Giải pháp cải thiện:**
- **Tối ưu hóa truy vấn cơ sở dữ liệu hoặc API** để giảm thời gian phản hồi.
- **Cấu hình load balancing** để phân tán tải đồng đều.
- **Tăng tài nguyên máy chủ** (CPU, RAM) khi tải tăng trên 50 users.
- **Áp dụng caching (Redis, Memcached)** để giảm request trùng lặp.

## ❓ Câu hỏi thảo luận:
### 1. Tại sao kiểm thử phi chức năng lại quan trọng trong phần mềm?
- Đảm bảo hệ thống hoạt động ổn định, nhanh, và có thể mở rộng.
- Xác định bottleneck (điểm nghẽn hiệu suất) trước khi triển khai thực tế.
- Giúp tối ưu hạ tầng server và tránh downtime khi có nhiều người dùng truy cập.
### 2. Các thông số quan trọng cần theo dõi trong kiểm thử hiệu suất là gì?
- Response Time (thời gian phản hồi) – Mức độ nhanh/chậm của hệ thống.
- Throughput (lưu lượng xử lý) – Hệ thống có thể xử lý bao nhiêu request/giây.
- Error Rate (tỷ lệ lỗi) – Hệ thống có trả về lỗi khi tải cao không?
- CPU & Memory Usage – Nếu có quyền truy cập server, cần theo dõi tài nguyên hệ thống.
### 3.Nếu hệ thống không đáp ứng yêu cầu hiệu suất, bạn sẽ đề xuất giải pháp gì?
- Tối ưu code backend: Cải thiện truy vấn database, caching, tối ưu API.
- Sử dụng Load Balancer: Chia tải giữa nhiều server.
- Nâng cấp phần cứng: CPU, RAM, Storage.
- Sử dụng CDN: Giúp giảm tải server khi có nhiều request tĩnh.
- Tối ưu frontend: Nén ảnh, tối ưu CSS/JS để giảm tải.
---
### Link ChatGPT: https://chatgpt.com/share/67a8c1a6-9eb8-8013-b975-f4b55ab5293e



