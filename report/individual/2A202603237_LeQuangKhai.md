# Báo cáo cá nhân — Lê Quang Khải — Ngày 2 Object Detection

**Trạng thái: bản nháp có kết quả annotation QC thực tế; chưa hoàn thành gate, train/eval và nộp LMS.** AI hỗ trợ tổng hợp và kiểm tra; học viên cần đọc lại, xác nhận nội dung và bổ sung phần phân tích của mình trước khi nộp.

## 1. Thông tin học viên

- Họ tên: Lê Quang Khải
- MSSV: 2A202603237
- Email: 26ai.khailq2@vinuni.edu.vn
- Lớp: L2
- Tên repo theo chuẩn: `K5-L2-DAY02-LeQuangKhai-2A202603237-ObjectDetection`
- Link repo bài làm: chờ học viên cung cấp tài khoản/repo GitHub đúng. Tài khoản connector hiện có không phải tài khoản nộp bài của học viên.
- Link Colab đã chạy: chưa có; chưa chạy pipeline chính thức vì annotation QC chưa đạt và cần làm rõ schema gold.

## 2. Schema và mini guideline

Xem [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Theo README: `car=0`, `truck=1`, `bus=2`, `van=3`.

Các ca đã đối chiếu gồm xe đỗ trong `drive_001`, xe buýt trong `drive_024`, tải/buýt trong `drive_040`, box hàng/cabin trong `drive_042`, và xe con trắng bị gán truck trong `drive_046`. Có dấu hiệu thứ tự ID của gold khác README; cần giảng viên xác nhận trước khi quyết định rework schema. Không sửa gold để vượt gate.

## 3. Annotation và self-QC

- Bộ nhãn xuất từ Label Lab local: 50/50 ảnh được học viên xác nhận trong tool, 652 box, revision 310 tại lúc xuất.
- Số file YOLO: **50**, cùng basename với ảnh train; checker gốc của lớp **đạt 50/50**.
- Class counts hiện tại: car 478, truck 19, bus 50, van 105.
- Commit giữ bản nhãn ban đầu tại máy, trước khi đọc nội dung gold: `d2ade47` (chưa push lên GitHub; không coi đây là mốc khóa/nộp chính thức trên LMS).
- Kiểm tra bổ sung có AI hỗ trợ trên mẫu ngẫu nhiên seed 20261110: `drive_020`, `drive_042`, `drive_012`, `drive_037`, `drive_046`, `drive_031`, `drive_043`, `drive_024`, `drive_021`, `drive_048`. Ảnh minh họa nhãn học viên ở [assets/self_qc/](assets/self_qc/). Danh sách này ghi hoạt động kiểm tra AI bổ sung; học viên cần trực tiếp rà và xác nhận trước khi tự nhận hoàn thành self-QC bằng mắt.
- Các vấn đề thực tế: xe con trắng gán truck ở `drive_046`; nhiều xe chưa có box ở `drive_024`/`drive_040`; box tách/chồng lên hàng và cabin ở `drive_042`; một số SUV/xe con đang bị gán van. Các lỗi này vẫn cần rà lại kể cả sau khi giải quyết lệch schema gold.

## 4. Gold QC và rework

Gold do học viên cung cấp trong file `gold_train_labels.rar`, được giải nén ngoài repo bài nộp. Chạy nguyên script `tools/evaluate_annotations.py`, matching cùng lớp tại IoU >= 0,50; không đổi ngưỡng hoặc đổi nhãn gold.

Kết quả thực tế trong [outputs/annotation_evaluation.json](../../outputs/annotation_evaluation.json):

| Chỉ số | Bản trước rework | Sau rework |
|---|---:|---|
| TP | 303 | Chưa chạy |
| FP | 349 | Chưa chạy |
| FN | 350 | Chưa chạy |
| Precision | 0,4647 | Chưa chạy |
| Recall | 0,4640 | Chưa chạy |
| F1 | 0,4644 | Chưa chạy |
| Mean IoU của matches | 0,8320 | Chưa chạy |
| passed | false | Chưa chạy |

Gate yêu cầu đồng thời F1 >= 0,80 và mean IoU >= 0,70. Hiện mean IoU của các cặp khớp đạt nhưng F1 chưa đạt. Không suy ra mọi box đều tốt từ mean IoU, vì chỉ số này chỉ tính các cặp đã ghép được.

Vấn đề cần giảng viên xác nhận: gold có xe van ID 1 (ví dụ `drive_031`), xe tải ID 2 (`drive_037`, `drive_040`, `drive_046`), xe buýt ID 3 (`drive_024`, `drive_042`, `drive_048`), trong khi README quy định truck=1, bus=2, van=3. Một số xe đỗ trong `drive_001` có nhãn học viên nhưng không thấy cặp gold. Đây là quan sát từ đối chiếu ảnh, chưa phải kết luận gold chính thức sai.

Chưa rework nhãn trong hồ sơ này. Cần xác nhận schema/phạm vi, rà lại cả hình học và tính đầy đủ, rồi chấm lại và giữ commit trước/sau. Không dùng bản ánh xạ giả định hoặc sao chép gold làm nhãn học viên để báo đạt.

## 5. Kết quả YOLO

**Chưa chạy YOLO26n chính thức.** Chờ gold QC đạt và repo GitHub đúng của học viên để chạy `notebooks/day2_annotation_to_yolo.ipynb` trên Colab.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---|
| Precision | Chưa có kết quả |
| Recall | Chưa có kết quả |
| mAP50 | Chưa có kết quả |
| mAP50–95 | Chưa có kết quả |

Chưa có runtime Colab, best checkpoint của pipeline chính thức, `assets/eval_predictions/` hoặc `assets/yolo26_runs/`. Model YOLO11n dùng tạo gợi ý trong tool là model hỗ trợ annotation, không được dùng thay cho kết quả YOLO26n chính thức của bài.

### Câu hỏi phân tích

1. **Lớp recall thấp nhất và ảnh minh họa:** chưa thể kết luận vì chưa chạy đánh giá model trên 10 ảnh eval. Cần lấy per-class recall từ phiên chạy thật, rồi dẫn ảnh cụ thể.
2. **Nếu mAP50 cao nhưng mAP50–95 thấp:** cần kiểm độ sát/vị trí/kích thước box. Dự đoán có thể qua ngưỡng IoU 0,50 nhưng không ổn ở các ngưỡng cao hơn. Đây là giải thích khái niệm, chưa phải kết luận về một model đã chạy của bài này.
3. **Lỗi class liên quan guideline:** kiểm tra nhãn ban đầu cho thấy `car`/`truck` ở `drive_046` và khả năng `car`/`van` ở nhiều ảnh cần rà. Sự lệch ID gold cần được giải quyết trước khi quy mọi mismatch cho học viên. Sau train phải bổ sung phân tích từ prediction/confusion matrix thật.
4. **Vì sao không dùng eval để train:** 10 ảnh eval giữ vai trò validation và đánh giá theo split của bài, gồm chọn best checkpoint. Dùng chúng để cập nhật trọng số sẽ làm mất ý nghĩa đối chiếu. Vì eval đã tham gia chọn checkpoint nên cũng không được gọi là test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Quan sát | Cách xử lý | Kết quả/còn thiếu |
|---|---|---|---|
| Gán nhãn | Cần tiếp tục nhãn CVAT và vẽ trên Mac | Dùng Label Lab local, giữ snapshot, tự lưu và xuất YOLO | Đã xuất 50 file, 652 box |
| Hỗ trợ gán nhãn | Muốn gợi ý từ nhãn đã có | Fine-tune YOLO11n trên 12 ảnh ban đầu được học viên xác nhận; gợi ý tách riêng, học viên nhận/sửa | Đã dùng để hỗ trợ, cần công khai trong báo cáo |
| Gold QC 10/11/2026 | F1 0,4644, mean IoU 0,8320; dấu hiệu lệch schema/phạm vi | Giữ bản nhãn và gold nguyên trạng, lập tài liệu đối chiếu riêng, hỏi giảng viên | Chưa đạt gate; chưa train chính thức |
| GitHub/LMS | Tài khoản GitHub connector không phải tài khoản học viên | Chờ tài khoản/repo đúng của học viên | Chưa push, chưa nộp LMS |

## 7. Tự đánh giá theo rubric

Đã có 50 file đúng cấu trúc và gold evaluation JSON. Chất lượng đầy đủ/phân lớp còn cần rework; mini guideline và báo cáo là bản có AI hỗ trợ, cần học viên xác nhận. Chưa có kết quả pipeline chính thức và chưa nộp LMS. Không tự nhận hoàn thành hoặc tự gán điểm cho những phần chưa có kết quả.

## 8. Công cụ AI và tài liệu tham khảo

- Codex/AI hỗ trợ cài đặt công cụ, lập trình Label Lab, chuyển nhãn CVAT, kiểm tra file, chạy evaluator, tổng hợp số liệu, đối chiếu ảnh và soạn bản nháp tài liệu này. Học viên cần xác nhận và bổ sung phân tích của mình theo RULES.md.
- Label Lab: vẽ tay, lưu local, nhận/chỉnh gợi ý và xuất YOLO. Trạng thái 50/50 trong tool là xác nhận của học viên.
- Ultralytics YOLO11n + PyTorch MPS: model hỗ trợ gợi ý annotation, học từ 12 ảnh ban đầu; không phải model đánh giá cuối của bài.
- Tài liệu/schema gốc: [README](../../README.md), [GUIDE](../../GUIDE.md), [RULES](../../RULES.md), [SUBMISSION](../../SUBMISSION.md).
- Thư viện ngoài: [Ultralytics](https://docs.ultralytics.com/) và [PyTorch](https://pytorch.org/). Môi trường tool đã cài Ultralytics 8.4.208, Torch 2.15.0, Python 3.14.10 trên Apple M5; đây là runtime tool, chưa phải runtime Colab cuối.
