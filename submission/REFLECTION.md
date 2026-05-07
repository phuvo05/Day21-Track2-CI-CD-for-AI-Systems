# REFLECTION - Lab CI/CD cho AI Systems

**Họ và tên:** Võ Thiên Phú | **MSSV:** 2A202600336

## Bộ siêu tham số đã chọn và lý do

| Tham số | Giá trị | Lý do |
|---|---|---|
| n_estimators | 300 | Tăng số cây từ 100 lên 300 giúp giảm overfitting và tăng ổn định của mô hình ensemble. |
| max_depth | 20 | Giới hạn độ sâu cây ở mức 20 giúp kiểm soát độ phức tạp, tránh overfitting so với max_depth=None. |
| min_samples_split | 2 | Giữ nguyên giá trị mặc định 2 để cây có thể phân chia sớm, phù hợp với dữ liệu không quá lớn (5996 mẫu). |

Các thí nghiệm đã thử với nhiều bộ siêu tham số khác nhau. Bộ trên cho kết quả accuracy tốt nhất trên tập eval, đồng thời thời gian huấn luyện vẫn nằm trong giới hạn cho phép của GitHub Actions.

## Khó khăn và cách giải quyết

1. **Lỗi `curl: Could not resolve host: health`**: Trên PowerShell, alias `curl` = `Invoke-WebRequest` không xử lý biến `$VM_IP` đúng cách. Giải quyết: dùng `curl.exe` (curl thật) thay vì `curl`.

2. **GitHub Actions không được kích hoạt**: Commit dữ liệu đã push nhưng workflow MLOps Pipeline không tự chạy (HTTP 403 khi thử trigger thủ công do thiếu quyền admin). Giải quyết: GitHub Actions đã được kích hoạt tự động sau vài phút, pipeline chạy thành công cả 4 jobs.

3. **DVC chưa được cài**: Cài `dvc` và `dvc-gs` qua pip để push data lên GCS.

4. **Accuracy Bước 3 giảm nhẹ** (0.7420 so với 0.7560 ở Bước 2): Dữ liệu mới thêm vào (2998 mẫu từ train_phase2) có thể có phân bố khác với dữ liệu ban đầu, gây nhiễu nhẹ. Tuy nhiên, kết quả vẫn đạt ngưỡng eval gate (>= 0.70) nên model mới vẫn được deploy thành công lên VM.
