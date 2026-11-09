# Chạy Colab — Lê Quang Khải, 2A202603237

> **Phiên của học viên đã chạy xong:** [Colab Tesla T4](https://colab.research.google.com/drive/1cu7LVkcAXXOebHDUgeGvU-n-tX68GjZA?usp=sharing). Notebook có đủ output train 50 epoch, evaluate và predict 10 ảnh. Bước hiện tại là tải `day2_colab_outputs.zip` trong `/content/Day2-Lab/outputs/` về máy để cập nhật ảnh/log Colab vào repo, sau đó nộp link repo lên LMS. Các bước bên dưới được giữ để tham khảo hoặc chạy lại khi cần.

Repo đã có 50 file nhãn mới nhất, 679 box và notebook đã đặt sẵn `REPO_URL` đúng repo kasdie. Theo học viên cung cấp, Lab Coach cho phép tiếp tục dù F1 thấp; giữ nguyên kết quả chấm thật.

## Tạo bản Colab của bạn

1. Mở [notebook trên Google Colab](https://colab.research.google.com/github/kasdie/K5-L2-Day02-LeQuangKhai-2A202603237-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb).
2. Đăng nhập tài khoản Google của bạn, chọn **Sao chép vào Drive / Copy to Drive**. Colab tạo một bản riêng; địa chỉ thường có dạng `https://colab.research.google.com/drive/...`.
3. Chọn **Thời gian chạy → Thay đổi loại thời gian chạy → GPU T4** nếu được cấp, rồi lưu. Nếu không có GPU, vẫn có thể chạy CPU nhưng lâu hơn; không cần mua gói Colab để nộp bài trừ khi lớp có yêu cầu riêng.
4. Chọn **Thời gian chạy → Chạy tất cả**. Notebook cài thư viện, clone đúng repo, xác nhận **50 train + 10 eval**, train YOLO26n 50 epoch rồi evaluate/predict. Không upload gold train lên Colab.
5. Chờ tất cả ô hoàn tất, không có lỗi. Cuối notebook sẽ hiện 10 ảnh prediction. Nếu Colab thiếu GPU hoặc ngắt phiên, ghi lỗi thực tế thay vì tự điền số liệu.

## Lưu kết quả để nộp

6. Trong thanh **Tệp / Files** bên trái Colab, vào `/content/Day2-Lab/outputs/`, tải `day2_colab_outputs.zip` về. ZIP chứa hai folder `eval_predictions/`, `yolo26_runs/` và `model_eval_metrics.json`; cell đóng gói đã loại checkpoint `.pt` và không tự nén chính ZIP.
7. Bấm **Chia sẻ / Share**, cấp quyền xem phù hợp yêu cầu lớp và sao chép link bản Drive đã chạy. Link mở mã GitHub ở bước 1 chưa phải link bản Drive này.
8. Gửi link bản Drive và ZIP cho Codex để cập nhật repo/báo cáo, hoặc tự đặt hai folder vào `report/individual/assets/`, cập nhật 4 chỉ số và runtime Colab trong báo cáo rồi commit/push. Nếu kết quả khác Mac, giữ rõ hai phiên; không gọi metrics local là metrics Colab.
9. Mở [LMS AI Thực chiến](https://aithucchien.vinuni.edu.vn), đăng nhập tài khoản cá nhân, vào bài Day02 và nộp **link repo GitHub**:

   https://github.com/kasdie/K5-L2-Day02-LeQuangKhai-2A202603237-ObjectDetection

10. Kiểm tra trạng thái đã nộp và giữ xác nhận. Việc push GitHub không tự nộp LMS.

## Kết quả đã có trên Mac

Đã hoàn thành một phiên local thật: YOLO26n 50 epoch, 50 train/10 eval, precision 0,453994, recall 0,232626, mAP50 0,277476, mAP50–95 0,206158. Xem [notebook có output](notebooks/day2_annotation_to_yolo_local_executed.ipynb) và [báo cáo](report/individual/2A202603237_LeQuangKhai.md). Đây là bằng chứng chạy local, không phải bằng chứng đã chạy trên Google Colab.
