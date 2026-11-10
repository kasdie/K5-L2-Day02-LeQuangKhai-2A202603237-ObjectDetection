# Hồ sơ Day02 — Lê Quang Khải, 2A202603237, L2

## Đã hoàn thiện trên GitHub

- 50 file YOLO, 679 box; checker gốc đạt 50/50. Giữ lịch sử nhãn ban đầu 652 box và bản sau sửa.
- Gold evaluation trước/sau: F1 0,4644 → 0,4895; mean IoU 0,8320 → 0,8335. `passed=false` giữ nguyên, tiếp tục theo hướng dẫn Coach do học viên cung cấp về F1 thấp.
- [Mini guideline](GUIDELINE_MINI.md), [báo cáo và bốn câu phân tích](report/individual/2A202603237_LeQuangKhai.md), 10 ảnh nhãn học viên phục vụ self-QC có AI hỗ trợ.
- **[Colab thực tế đã chạy](https://colab.research.google.com/drive/1cu7LVkcAXXOebHDUgeGvU-n-tX68GjZA?usp=sharing)**: YOLO26n, Tesla T4, 50 epoch, đúng 50 train/10 eval, evaluate/predict đủ 10 ảnh.
- [Bản sao notebook Colab có output](notebooks/day2_annotation_to_yolo_colab_executed.ipynb), metrics và log từng lớp.
- Đã nhận ZIP `day2_colab_outputs.zip`, đối chiếu 4 metrics khớp notebook và kiểm tra CSV đủ 50 epoch.
- Đúng hai folder kết quả Colab nằm tại [assets/eval_predictions/](report/individual/assets/eval_predictions/) và [assets/yolo26_runs/](report/individual/assets/yolo26_runs/). Có đủ 10 JPG eval, results.csv và các biểu đồ; không có checkpoint `.pt` trong bài nộp.
- Metrics Colab: precision 0,409064; recall 0,233936; mAP50 0,270667; mAP50–95 0,199550. Bus có recall thấp nhất: 0,000.
- Kết quả Mac trước đó giữ riêng tại [assets/local_mac/](report/individual/assets/local_mac/), không trộn với ảnh/log Colab.
- Gold train, archive gold, token và checkpoint được giữ ngoài repo bài nộp.

## Học viên thực hiện bước nộp cuối

1. Đọc, xác nhận và bổ sung phần phân tích của mình trong báo cáo/guideline theo RULES.md.
2. Nộp link repo GitHub trên LMS bằng tài khoản cá nhân:

   https://github.com/kasdie/K5-L2-Day02-LeQuangKhai-2A202603237-ObjectDetection

3. Kiểm tra trạng thái đã nộp và giữ xác nhận. **Chưa ghi nhận xác nhận nộp LMS**; push GitHub không tự nộp LMS.

[Hướng dẫn Colab và nộp bài](BAT-DAU-COLAB.md). Hạn nộp/gia hạn theo thông báo chính thức của Coach/LMS.
