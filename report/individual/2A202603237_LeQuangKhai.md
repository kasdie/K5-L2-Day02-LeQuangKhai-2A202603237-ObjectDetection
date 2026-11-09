# Báo cáo cá nhân — Lê Quang Khải — Ngày 2 Object Detection

**Trạng thái:** đã hoàn thành annotation, self-QC bổ sung, gold QC trước/sau sửa và train/evaluate/predict YOLO26n trên **Google Colab Tesla T4**. Đang bổ sung ZIP ảnh/log gốc của phiên Colab. Chưa ghi nhận nộp LMS. Báo cáo có AI hỗ trợ; học viên cần đọc, xác nhận và bổ sung nhận xét của mình theo RULES.md.

## 1. Thông tin học viên

- Họ tên: **Lê Quang Khải**
- MSSV: **2A202603237**
- Email: 26ai.khailq2@vinuni.edu.vn
- Lớp: L2
- [Repo bài làm](https://github.com/kasdie/K5-L2-Day02-LeQuangKhai-2A202603237-ObjectDetection)
- **[Bản Colab đã chạy và được chia sẻ quyền xem](https://colab.research.google.com/drive/1cu7LVkcAXXOebHDUgeGvU-n-tX68GjZA?usp=sharing)**
- [Bản sao notebook có output được tải từ link Colab](../../notebooks/day2_annotation_to_yolo_colab_executed.ipynb)
- [Phiên notebook Mac trước đó](../../notebooks/day2_annotation_to_yolo_local_executed.ipynb), được giữ làm kết quả riêng.

## 2. Schema và mini guideline

Xem [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Giữ schema README/data.yaml: car=0, truck=1, bus=2, van=3. Guideline ghi quy tắc box, vật thể bị che/cắt khung, xe đỗ và các ca drive_001, drive_024, drive_040, drive_042, drive_046.

Đối chiếu gold có dấu hiệu khác thứ tự ID/phạm vi so với README; chưa tự sửa gold hoặc đổi schema để tăng điểm. Học viên cho biết ngày **10/11/2026**, **Lab Coach cho phép F1 thấp, miễn làm đầy đủ các bước**. Vì vậy tiếp tục pipeline và ghi nguyên số liệu QC chưa đạt ngưỡng script.

## 3. Annotation và self-QC

- Công cụ: Label Lab local, tiếp tục nhãn CVAT, vẽ/chỉnh tay và nhận/sửa gợi ý AI.
- Nhãn dùng train: **50 file YOLO, 679 box**, revision **323**; học viên xác nhận 50/50 ảnh trong tool.
- Checker gốc: **đạt 50/50**, giữ basename, 5 cột, normalized, đúng bốn class ID.
- Train gồm car **501**, truck **19**, bus **52**, van **107** box.
- Commit giữ nhãn ban đầu: `d2ade47` (652 box, tạo local trước khi đọc nội dung gold; không tự coi là bằng chứng đã khóa/nộp LMS trước gold).
- Commit sửa nhãn: `15c0df1` (679 box). drive_024 thêm 12 box, drive_040 thêm 15 box; drive_046 sửa xe con trắng từ truck về car và chỉnh box.
- Mẫu đối chiếu bổ sung có AI hỗ trợ, seed 20261110: drive_020, drive_042, drive_012, drive_037, drive_046, drive_031, drive_043, drive_024, drive_021, drive_048. Xem [10 ảnh nhãn học viên](assets/self_qc/), cập nhật theo revision 323. Học viên cần trực tiếp xác nhận phần rà bằng mắt; không mô tả việc đối chiếu AI là hoạt động người dùng đã làm thay.

## 4. Gold QC và rework

Chạy nguyên `tools/evaluate_annotations.py`, matching cùng lớp tại IoU >= 0,50. Không sửa gold hay ngưỡng.

| Chỉ số | Ban đầu — 652 box | Sau sửa — 679 box |
|---|---:|---:|
| TP | 303 | 326 |
| FP | 349 | 353 |
| FN | 350 | 327 |
| Precision | 0,4647 | 0,4801 |
| Recall | 0,4640 | 0,4992 |
| F1 | 0,4644 | **0,4895** |
| Mean IoU của matches | 0,8320 | **0,8335** |
| passed | false | false |

Xem [JSON trước sửa](../../outputs/annotation_evaluation_before_rework.json) và [JSON hiện tại](../../outputs/annotation_evaluation.json). F1 tăng 0,0251, TP tăng 23 và FN giảm 23. Mean IoU chỉ tính các cặp đã ghép, không chứng minh mọi box đều tốt.

Ngưỡng mặc định là F1 >= 0,80 và mean IoU >= 0,70. `passed` vẫn **false**; tiếp tục theo hướng dẫn Lab Coach mà học viên cung cấp, không gọi đây là đạt ngưỡng tự động.

Quan sát cần thận trọng: gold có xe van ID 1 ở drive_031, xe tải ID 2 ở drive_037/040/046, xe buýt ID 3 ở drive_024/042/048; khác thứ tự README. Xe đỗ drive_001 không có các cặp gold tương ứng. Đây là dấu hiệu cần làm rõ, không đủ để quy mọi FP/FN cho gold; nhãn học viên vẫn có hạn chế và thiếu vật thể.

## 5. Phiên YOLO26n trên Google Colab

### Runtime và tham số

- Nguồn kết quả: [notebook Drive đã chạy](https://colab.research.google.com/drive/1cu7LVkcAXXOebHDUgeGvU-n-tX68GjZA?usp=sharing), được đọc từ bản chia sẻ có quyền xem; bản sao notebook giữ nguyên output thực tế.
- **Tesla T4, CUDA:0, 14913 MiB**, Python **3.13.16**, Torch **2.14.1+cu130**, Ultralytics **8.4.208**.
- Model **yolo26n.pt**, **50 epoch**, imgsz **640**, seed **20260909** theo notebook lớp.
- `batch=-1` (auto), log AutoBatch chọn **46**. Phiên Mac trước dùng batch 4; không gán số liệu hai runtime cho nhau.
- Notebook clone commit **f8edd3b** của repo học viên; xác nhận **50 train + 10 eval**. Eval không tham gia cập nhật trọng số, nhưng được dùng validation/chọn best checkpoint.
- Checkpoint trong phiên Colab: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`; không commit checkpoint.
- Đã kiểm tra đủ **6 ô code có execution count**, không có output lỗi, log hoàn tất epoch 50/50, evaluate và predict 10/10 ảnh.

### Metrics Colab trên 10 ảnh eval

| Chỉ số | Giá trị thực tế |
|---|---:|
| Precision | **0.409064** |
| Recall | **0.233936** |
| mAP50 | **0.270667** |
| mAP50–95 | **0.199550** |

Các số tổng hợp lấy nguyên JSON ở ô evaluate, không lấy từ kết quả Mac. Bảng từng lớp lấy từ stdout cùng ô, làm tròn như Ultralytics in ra:

| Lớp theo data.yaml | Số instance eval | Precision | Recall | mAP50 | mAP50–95 |
|---|---:|---:|---:|---:|---:|
| car | 94 | 0,836 | 0,652 | 0,7400 | 0,5240 |
| truck | 5 | 0,663 | 0,200 | 0,2400 | 0,2060 |
| bus | 2 | 0,000 | **0,000** | 0,0141 | 0,00466 |
| van | 12 | 0,136 | 0,0833 | 0,0889 | 0,0635 |

### Đầu ra và nguồn ảnh

Chưa nhận ZIP gốc từ Colab. Hai folder `assets/eval_predictions/` và `assets/yolo26_runs/` đang có vẫn thuộc **phiên Mac**, không được coi là ảnh/log Colab. Notebook chia sẻ đã xác nhận Colab hoàn tất, có đủ metrics và log dự đoán 10 ảnh; cần tải ZIP để bổ sung các ảnh/CSV gốc của đúng phiên Colab.

- [Metrics Colab đã xác nhận](assets/colab/model_eval_metrics.json) và [metrics từng lớp](assets/colab/model_eval_per_class.json).
- [Log train Colab](assets/colab/training_stdout.txt), [log evaluate](assets/colab/evaluation_stdout.txt), [log predict 10 ảnh](assets/colab/prediction_stdout.txt).
- [Ảnh dự đoán phiên Mac để tham khảo, không thay ảnh Colab](assets/eval_predictions/visuals/).

- [Bằng chứng notebook/runtime](assets/colab/notebook_provenance.json).
- [Log evaluate Colab có bảng từng lớp](assets/colab/evaluation_stdout.txt).

### Trả lời bốn câu hỏi phân tích

1. **Lớp recall thấp nhất:** `bus` (ID 2 theo data.yaml), recall **0,000**. Có thể dùng **drive_051** minh họa: nhãn eval gốc có một instance ID 2, nhưng log predict Colab ở conf=0,25 chỉ có **8 car và 1 van**, không có prediction bus. Eval chỉ có 2 instance ID 2, ở drive_051 và drive_058, nên kết luận nhạy với từng lỗi. Cần đối chiếu ảnh và ID khi diễn giải do quan sát lệch schema đã nêu; không suy từ recall 0 rằng model không tạo prediction tên bus ở bất kỳ ảnh nào. Các ảnh khác trong log vẫn có bus. Ảnh Colab gốc sẽ được bổ sung từ ZIP; hiện kết luận dựa trên nhãn eval và log Colab đã lưu, không lấy ảnh Mac làm ảnh Colab.
2. **Nếu mAP50 cao nhưng mAP50–95 thấp:** box có thể đúng tương đối ở IoU 0,50 nhưng chưa sát hoặc lệch vị trí/kích thước khi xét các ngưỡng IoU cao hơn. Cần rà box lỏng, biên vật thể bị che/cắt và chất lượng hình học. Trong phiên thực tế mAP50=0,2707, mAP50–95=0,1996 đều còn thấp; không gọi mAP50 hiện tại là cao. Cũng cần xem thiếu vật thể và sai lớp trước khi quy nguyên nhân chỉ cho box.
3. **Lỗi lớp liên quan guideline:** bản nhãn đầu có xe con trắng gán truck ở drive_046; học viên đã sửa về car. Quy tắc cần nhất quán: SUV/taxi/bán tải theo README thuộc car, tải chở hàng thuộc truck, bus khác van/minivan. Không chọn van làm lớp mặc định cho xe lớn/khó nhận biết, không tách hàng hóa trên xe thành một xe riêng. Train có 501 car nhưng chỉ 19 truck; các lớp ngoài car có recall thấp. Đây là những yếu tố cần kiểm cùng độ phủ và chất lượng nhãn, chưa chứng minh quan hệ nhân quả. Quan sát lệch ID gold/README cũng cần ghi như hạn chế, không đổi nhãn chỉ để tăng điểm.
4. **Vì sao không dùng 10 ảnh eval để train:** giữ chúng ngoài tập cập nhật trọng số để đánh giá trên ảnh không dùng học trọng số; đưa vào train gây rò rỉ dữ liệu và làm số đo quá lạc quan. Notebook dùng chúng làm validation/chọn best nên không phải test set độc lập. Giữ nguyên split theo đề.

### Kết quả Mac trước đó — để đối chiếu riêng

Mac Apple M5/MPS, Python 3.14.10, Torch 2.15.0, Ultralytics 8.4.208, 50 epoch, batch 4, workers 0, amp=False. Precision 0.453994, recall 0.232626, mAP50 0.277476, mAP50–95 0.206158. Notebook local đã chạy được giữ riêng; số liệu chính ở mục 5 phía trên là Colab. Runtime, batch và phiên bản Torch khác nhau nên không yêu cầu hai phiên cho cùng kết quả.

## 6. Khó khăn và cách xử lý

| Bước | Quan sát | Cách xử lý / kết quả |
|---|---|---|
| Annotation | Tiếp tục nhãn CVAT trên Mac | Label Lab local, lưu/backup và xuất đủ 50 nhãn |
| Gợi ý nhãn | Cần hỗ trợ từ nhãn đã có | YOLO11n fine-tune trên 12 ảnh ban đầu để đề xuất; học viên nhận/sửa, không dùng thay YOLO26n của pipeline cuối |
| Gold QC | F1 thấp và dấu hiệu khác ID/phạm vi | Giữ số liệu thật, sửa 3 ảnh, F1 tăng 0,4644 → 0,4895. Tiếp tục theo thông tin hướng dẫn Coach |
| Ô đầu Colab | Bản Drive còn giá trị mẫu REPO_URL, clone thất bại rồi gọi auth.user_token không hợp lệ | Sửa REPO_URL đúng repo công khai; phiên Colab cuối đã clone thành công và không còn output lỗi |
| Train/eval | Model yếu ở các lớp ngoài car | Báo cáo metrics thật, xem mất cân bằng/độ phủ/nhãn và ghi hạn chế schema; không sửa eval hoặc chọn split khác |
| Hồ sơ kết quả | Kết quả Mac và Colab khác nhau | Giữ hai phiên tách bạch, dùng số liệu Colab và ghi nguồn notebook/ZIP |
| Nộp bài | Cần thao tác LMS bằng tài khoản học viên | Repo và link Colab đã có; chưa ghi nhận xác nhận nộp LMS |

## 7. Tự đánh giá theo rubric

Đã có 50 nhãn đúng cấu trúc, guideline, gold QC trước/sau và vòng sửa nhãn, phiên YOLO26n Colab chạy đủ 50 epoch, evaluate/predict đúng 10 eval, metrics từng lớp và phần phân tích có AI hỗ trợ. Cần bổ sung ZIP để có đủ hai folder ảnh/log gốc của Colab. F1 chưa đạt ngưỡng script nhưng được báo cáo đúng theo hướng dẫn Coach do học viên cung cấp. Học viên cần xác nhận nội dung và nộp link repo lên LMS; không tự gán điểm hoặc gọi bước chưa có bằng chứng là đã hoàn thành.

## 8. Công cụ AI và tài liệu tham khảo

- Codex/AI hỗ trợ lập trình Label Lab, chuyển CVAT, kiểm tra file, chạy evaluator/local notebook, đối chiếu dữ liệu, đọc notebook Colab chia sẻ, tổng hợp số liệu và soạn phân tích này. Học viên cần đọc lại, xác nhận và bổ sung nhận xét của mình theo RULES.md.
- Học viên gán/chỉnh nhãn, xác nhận 50/50 trong tool, sửa drive_024/040/046, thực hiện phiên Colab và chia sẻ link đã chạy.
- YOLO11n hỗ trợ annotation từ 12 ảnh đầu; YOLO26n là model cuối của bài, không trộn kết quả hai model.
- [README](../../README.md), [GUIDE](../../GUIDE.md), [RULES](../../RULES.md), [SUBMISSION](../../SUBMISSION.md); [Ultralytics](https://docs.ultralytics.com/), [PyTorch](https://pytorch.org/).
- Không đưa token, gold train hoặc checkpoint `.pt` vào bài nộp. Tài liệu đối chiếu gold giữ riêng trên máy.
