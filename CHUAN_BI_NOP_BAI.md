# Hồ sơ Day02 — Lê Quang Khải, 2A202603237, L2

## Đã xác nhận bằng kết quả thật

- 50 nhãn YOLO, 679 box, checker đạt 50/50; giữ lịch sử bản đầu 652 box và bản sau sửa.
- Gold QC trước/sau: F1 0,4644 → 0,4895; mean IoU 0,8320 → 0,8335; `passed=false` giữ nguyên. Theo học viên, Coach cho phép F1 thấp nếu làm đủ các bước.
- [Mini guideline](GUIDELINE_MINI.md), [báo cáo và bốn câu phân tích](report/individual/2A202603237_LeQuangKhai.md), 10 ảnh nhãn học viên phục vụ self-QC có AI hỗ trợ.
- **[Colab thực tế đã chạy](https://colab.research.google.com/drive/1cu7LVkcAXXOebHDUgeGvU-n-tX68GjZA?usp=sharing)** trên Tesla T4: YOLO26n, đủ 50 epoch, đúng 50 train/10 eval, không có output lỗi; predict đủ 10 ảnh.
- [Bản sao notebook Colab có output](notebooks/day2_annotation_to_yolo_colab_executed.ipynb), [metrics Colab](report/individual/assets/colab/model_eval_metrics.json), [metrics từng lớp](report/individual/assets/colab/model_eval_per_class.json), stdout train/evaluate/predict.
- Colab: precision 0,409064; recall 0,233936; mAP50 0,270667; mAP50–95 0,199550. Bus có recall thấp nhất: 0,000.
- Phiên Mac trước đó được giữ riêng và ghi đúng nguồn; không thay số liệu Colab bằng số liệu Mac.

## Phần còn lại

1. Tải `day2_colab_outputs.zip` từ Colab về máy. Hiện chưa nhận ZIP; hai folder ảnh/log trong assets vẫn là của phiên Mac. Cần dùng ZIP để thay bằng đúng ảnh/log Colab và giữ riêng kết quả Mac.
2. Học viên đọc, xác nhận và bổ sung phần phân tích của mình theo RULES.md.
3. Nộp link repo GitHub lên LMS bằng tài khoản cá nhân và giữ xác nhận. Việc push GitHub không tự nộp LMS.

[Hướng dẫn tải ZIP và nộp](BAT-DAU-COLAB.md). Không đưa gold train, token hoặc checkpoint `.pt` lên GitHub/Colab; bộ đối chiếu gold giữ riêng trên máy.
