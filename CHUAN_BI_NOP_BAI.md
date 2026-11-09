# Hồ sơ Day02 — Lê Quang Khải, 2A202603237, L2

## Đã có kết quả thực tế

- 50 nhãn YOLO, 679 box, checker đạt 50/50; repo kasdie đã có nhãn để Colab clone.
- Lịch sử nhãn ban đầu 652 box và bản đã sửa 679 box.
- Gold QC trước/sau: F1 0,4644 → 0,4895; mean IoU 0,8320 → 0,8335. `passed=false` được giữ nguyên. Tiếp tục theo hướng dẫn Lab Coach do học viên cung cấp: F1 thấp được chấp nhận nếu làm đủ bước.
- [Mini guideline](GUIDELINE_MINI.md), [báo cáo](report/individual/2A202603237_LeQuangKhai.md) và 10 ảnh self-QC bổ sung có AI hỗ trợ.
- [Notebook YOLO26n local có output](notebooks/day2_annotation_to_yolo_local_executed.ipynb): chạy đủ 50 epoch, giữ đúng 50 train/10 eval.
- Hai folder [eval_predictions](report/individual/assets/eval_predictions/) và [yolo26_runs](report/individual/assets/yolo26_runs/), metrics/ảnh/CSV đã có trong assets; không chứa checkpoint `.pt`.
- Precision 0,453994; recall 0,232626; mAP50 0,277476; mAP50–95 0,206158 trên phiên Mac.

## Phần cần tài khoản học viên

1. Đọc, xác nhận và bổ sung phần phân tích của mình trong guideline/báo cáo theo RULES.md.
2. [Mở Colab, tạo bản Drive và chạy](BAT-DAU-COLAB.md). Notebook đã đặt đúng URL repo. Lưu link bản đã chạy và ZIP output; cập nhật riêng kết quả/runtime Colab nếu có.
3. Nộp link repo lên LMS bằng tài khoản học viên và giữ xác nhận. Chưa có xác nhận nộp LMS.

Không đưa gold train, archive gold, token hoặc checkpoint lên GitHub/Colab. File gold và bộ đối chiếu riêng vẫn nằm ngoài repo bài nộp. Hạn nộp/gia hạn theo thông báo chính thức của Coach/LMS.
