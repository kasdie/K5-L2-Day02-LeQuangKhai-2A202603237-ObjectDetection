# Hồ sơ Day02 — Lê Quang Khải, 2A202603237, L2

**Bản chuẩn bị, chưa đủ điều kiện nộp hoàn chỉnh.**

## Đã có

- 50 file trong `dataset/labels/train/`, 652 box; checker của lớp đạt 50/50.
- Commit local `d2ade47` giữ nhãn ban đầu trước khi đọc nội dung gold. Chưa push/khóa nhãn trên hệ thống lớp.
- Gold evaluation thật trong [outputs/annotation_evaluation.json](outputs/annotation_evaluation.json): F1 0,4644, mean IoU 0,8320, `passed=false`.
- [Mini guideline](GUIDELINE_MINI.md), [báo cáo cá nhân](report/individual/2A202603237_LeQuangKhai.md) và 10 ảnh nhãn học viên phục vụ rà lại. Tài liệu có AI hỗ trợ, cần học viên xác nhận.
- File gold, ảnh đối chiếu gold và checkpoint đều giữ riêng ngoài hồ sơ nộp.

## Cần hoàn tất

1. Giảng viên xác nhận thứ tự ID/phạm vi gold đang có dấu hiệu khác README. Bộ nhãn cũng cần rà lỗi thực tế, không chỉ sửa ánh xạ ID.
2. Học viên rà/sửa nhãn, chấm lại bằng gold/schema đã được xác nhận; đạt F1 >= 0,80 và mean IoU >= 0,70. Giữ lịch sử trước/sau.
3. Tạo repo bằng đúng tài khoản của học viên: `K5-L2-DAY02-LeQuangKhai-2A202603237-ObjectDetection`; push nhãn và hồ sơ. Tài khoản connector đang có không phải tài khoản nộp bài nên chưa sử dụng.
4. Mở `notebooks/day2_annotation_to_yolo.ipynb` trên Colab, đặt REPO_URL đúng repo đã push, chạy YOLO26n theo split gốc, bổ sung kết quả thực tế.
5. Đưa 2 folder `eval_predictions/` và `yolo26_runs/` từ kết quả Colab vào `report/individual/assets/`, ghi 4 metrics và link Colab vào báo cáo, trả lời phần phân tích bằng kết quả thật.
6. Kiểm tra repo không chứa gold/token/checkpoint; nộp link repo lên LMS bằng tài khoản cá nhân và giữ xác nhận nộp.

Không dùng số liệu YOLO11n của tool gợi ý để điền thay số liệu YOLO26n của bài. Chưa có xác nhận nộp LMS.
