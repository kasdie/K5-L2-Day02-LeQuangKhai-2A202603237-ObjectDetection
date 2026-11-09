# Phân tích từ phiên Colab đã chạy

Nguồn: https://colab.research.google.com/drive/1cu7LVkcAXXOebHDUgeGvU-n-tX68GjZA

Phiên Google Colab Tesla T4, Python 3.13.16, Torch 2.14.1+cu130, Ultralytics 8.4.208. YOLO26n, 50 epoch, imgsz=640, seed=20260909, auto-batch chọn 46. 50 ảnh train, đúng 10 ảnh eval, 113 instance eval; notebook có đủ 6 ô code đã chạy và không có output lỗi.

Precision 0,409064; recall 0,233936; mAP50 0,270667; mAP50–95 0,199550. Đây là metrics model trên eval, khác F1 annotation 0,4895.

1. **Lớp recall thấp nhất:** `bus` (ID 2 theo data.yaml), recall **0,000**; các lớp còn lại: car 0,652, truck 0,200, van 0,0833. Tập eval chỉ có 2 instance ID 2 ở drive_051 và drive_058. Có thể dùng **drive_051** minh họa: nhãn eval gốc có 1 instance ID 2, nhưng log predict Colab ở ngưỡng conf=0,25 chỉ ghi 8 car và 1 van, không có prediction bus. Cần đối chiếu ảnh và ID nhãn khi giải thích vì vẫn có quan sát lệch schema; không kết luận model không dự đoán bus ở bất kỳ ảnh nào, vì log các ảnh khác vẫn có bus.
2. **Nếu mAP50 cao nhưng mAP50–95 thấp:** box có thể đúng tương đối ở ngưỡng IoU 0,50 nhưng chưa sát/vị trí/kích thước chưa chính xác để vượt các ngưỡng IoU cao hơn. Cần kiểm box lỏng, lệch và vật thể bị che. Với phiên này, mAP50=0,2707 và mAP50–95=0,1996 đều còn thấp; không mô tả mAP50 thực tế là cao. Cũng cần rà thiếu vật thể và sai lớp trước khi kết luận nguyên nhân chỉ là hình học.
3. **Lỗi lớp và guideline:** bản nhãn ban đầu có xe con trắng bị gán truck ở drive_046; đã sửa về car. Quy tắc cần nhất quán: SUV/taxi/bán tải theo README thuộc car; tải chở hàng thuộc truck; bus khác van/minivan. Không dùng van làm lớp mặc định cho xe lớn/khó phân biệt, không tách hàng trên xe thành xe riêng. Train có 501 car, 19 truck, 52 bus, 107 van; các lớp ngoài car có recall thấp, nên cần xem cả độ phủ, mất cân bằng và chất lượng nhãn. Quan sát lệch ID giữa gold và README là hạn chế cần ghi rõ, chưa đủ để quy mọi lỗi cho gold hoặc cho một nguyên nhân duy nhất.
4. **Không dùng 10 ảnh eval để train:** giữ chúng ngoài dữ liệu cập nhật trọng số để có đối chiếu với ảnh không dùng học trọng số. Đưa eval vào train gây rò rỉ dữ liệu và làm số đo quá lạc quan. Trong notebook, eval cũng là validation dùng chọn best checkpoint, nên chưa phải test set độc lập; giữ nguyên split theo đề.

Bản phân tích này có AI hỗ trợ, học viên cần đọc và xác nhận bằng ảnh/kết quả của mình. Các giá trị từng lớp được làm tròn như bảng Ultralytics in ra; số tổng hợp lấy nguyên JSON ở ô evaluate.
