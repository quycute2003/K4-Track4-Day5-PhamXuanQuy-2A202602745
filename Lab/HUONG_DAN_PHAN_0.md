# Phần 0 — Cài đặt và hiểu bài lab

## Mở notebook trên Windows

Chạy từ thư mục gốc repo trong PowerShell. Dùng trực tiếp Python của `.venv` để không phụ thuộc chính sách chạy script activation của Windows:

```powershell
# Chỉ cần khi tạo lại môi trường
py -3.11 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt

# Mở Jupyter
.\.venv\Scripts\python.exe -m jupyter notebook Lab/kalman_fusion_lab_STUDENT.ipynb
```

Trong notebook, chọn kernel của môi trường lab rồi chạy cell code ở **Phần 0 · Thiết lập**. Kết quả mong đợi là `Setup OK - numpy ...` và không có lỗi. Chạy các cell tiếp theo theo thứ tự từ trên xuống.

## Bài toán cần hiểu

Robot có các phép đo vị trí bị nhiễu. Kalman filter dự đoán trạng thái từ mô hình chuyển động, cập nhật bằng phép đo và theo dõi độ bất định. Hợp nhất nhiều cảm biến giúp tận dụng độ tin cậy khác nhau của từng nguồn. Innovation (phần dư) là chênh lệch giữa phép đo và dự đoán; NIS chuẩn hóa phần dư theo độ bất định để kiểm tra mức độ bất thường.

Trong ví dụ 1 chiều, NIS có kỳ vọng khoảng **1**; phép đo vị trí 2 chiều có kỳ vọng khoảng **2**. Giá trị trung bình cao cho thấy sai lệch lớn hơn mô hình dự kiến, nhưng cần xem thêm median, residual và diễn biến theo thời gian để phân biệt lỗi.

## Lộ trình tiếp theo

| Phần | Việc cần làm |
|---|---|
| 1–4 | Chạy code có sẵn, đọc đồ thị: trung bình trượt, hợp nhất Gaussian, Kalman 1D và NIS. |
| 5.1 | Viết `make_F(dt)` (4×4) và `make_H()` (2×4) cho trạng thái `[x, y, vx, vy]`. |
| 5.2 | Viết `predict(self, F, Q)` và `update(self, z, H, R)` của lớp Kalman. |
| 6.1 | Xử lý phép đo theo timestamp, predict khi thời gian tăng rồi update và lưu log. |
| 7.1 | Viết `gated_update(kf, z, H, R, p=0.99)`; từ chối phép đo khi NIS vượt ngưỡng `chi2.ppf(p, df=len(z))`. |
| 9 | Đặt `STUDENT_ID`, đọc số liệu, chẩn đoán, chọn `bias` / `inflate_R` / `gate`, viết báo cáo gồm bằng chứng, cách sửa, độ tin cậy cuối và hạn chế. |
| 8 | EKF tùy chọn, phần thưởng. |

Repo hiện tại đã là fork `quycute2003/K4-Track4-Day5-PhamXuanQuy-2A202602745`. Khi đến Phần 9, mã số theo tên repo là `2A202602745`; cần dùng đúng mã của bạn để sinh dữ liệu.

Notebook gọi cảm biến ở Phần 6 là LiDAR/radar/camera và dùng GPS/UWB ở nhiệm vụ Phần 9. Một số chữ ký hàm trong hướng dẫn tổng quan khác notebook; hãy lấy yêu cầu và check cell trong notebook làm chuẩn.

Các bài có chấm vẫn cần được hoàn thành và chạy kiểm tra. Sau khi xong toàn bộ bài, lưu file `.ipynb` có output để nộp. Chạy thành công Phần 0 chỉ xác nhận môi trường đã sẵn sàng.
