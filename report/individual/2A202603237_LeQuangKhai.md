# Báo cáo cá nhân — Lê Quang Khải — Ngày 2 Object Detection

**Trạng thái:** đã có nhãn, self-QC bổ sung, gold evaluation trước/sau, train YOLO26n và đánh giá thực tế trên Mac. Đã chuẩn bị notebook Colab; chưa ghi nhận phiên chạy Google Colab hoặc nộp LMS. AI hỗ trợ tổng hợp; học viên cần đọc lại, xác nhận và bổ sung phân tích của mình trước khi nộp.

## 1. Thông tin học viên

- Họ tên: **Lê Quang Khải**
- MSSV: **2A202603237**
- Email: 26ai.khailq2@vinuni.edu.vn
- Lớp: L2
- Repo bài làm: https://github.com/kasdie/K5-L2-Day02-LeQuangKhai-2A202603237-ObjectDetection
- [Mở notebook trên Colab](https://colab.research.google.com/github/kasdie/K5-L2-Day02-LeQuangKhai-2A202603237-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb). Đây là link mở mã từ GitHub, chưa phải link bản Drive đã chạy của học viên.
- [Notebook có output thực tế từ Mac](../../notebooks/day2_annotation_to_yolo_local_executed.ipynb).

## 2. Schema và mini guideline

Xem [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Giữ schema README/data.yaml: car=0, truck=1, bus=2, van=3. Guideline ghi quy tắc box, xe bị che/cắt khung, xe đỗ và các ca thực tế ở drive_001, drive_024, drive_040, drive_042, drive_046.

Gold có dấu hiệu dùng thứ tự ID hoặc phạm vi khác README; quan sát này được giữ như hạn chế, không tự sửa gold hoặc đổi schema để tăng điểm. Theo học viên trao đổi ngày **10/11/2026**, **Lab Coach cho phép F1 thấp, miễn hoàn thành đầy đủ các bước**. Vì vậy tiếp tục train/eval và báo cáo nguyên các chỉ số QC chưa đạt ngưỡng tự động.

## 3. Annotation và self-QC

- Công cụ: Label Lab local, tiếp tục nhãn CVAT, vẽ/chỉnh tay và nhận/sửa gợi ý AI.
- Bản mới nhất dùng train: **50 file YOLO, 679 box**, revision **323**; 50/50 ảnh được học viên xác nhận trong tool.
- Checker gốc: **đạt 50/50**, giữ basename, 5 cột, normalized, đúng bốn class ID.
- Phân bố nhãn train: car **501**, truck **19**, bus **52**, van **107**.
- Commit giữ bản nhãn ban đầu trước khi đọc nội dung gold: `d2ade47` (652 box, commit local tại thời điểm tạo, chưa phải bằng chứng đã khóa/nộp LMS trước gold).
- Commit cập nhật bộ nhãn mới và notebook Colab: `15c0df1` (679 box). Giữ lịch sử trước/sau, không thay bản đã lưu ban đầu.
- Học viên đã chỉnh drive_024, drive_040, drive_046. So với bản đầu, drive_024 thêm 12 box, drive_040 thêm 15 box, drive_046 sửa xe con trắng từ truck về car và chỉnh box.
- Mẫu kiểm tra bổ sung bằng AI, lấy ngẫu nhiên seed 20261110: drive_020, drive_042, drive_012, drive_037, drive_046, drive_031, drive_043, drive_024, drive_021, drive_048. Xem [10 ảnh nhãn học viên](assets/self_qc/), đã cập nhật theo revision 323. Học viên cần trực tiếp rà và xác nhận phần self-QC bằng mắt; danh sách này ghi đúng hoạt động đối chiếu AI bổ sung.

## 4. Gold QC và rework

Chạy nguyên `tools/evaluate_annotations.py`, ghép cùng lớp tại IoU >= 0,50; không đổi gold hoặc ngưỡng chấm.

| Chỉ số | Bản ban đầu, 652 box | Sau sửa, 679 box |
|---|---:|---:|
| TP | 303 | 326 |
| FP | 349 | 353 |
| FN | 350 | 327 |
| Precision | 0,4647 | 0,4801 |
| Recall | 0,4640 | 0,4992 |
| F1 | 0,4644 | **0,4895** |
| Mean IoU của matches | 0,8320 | **0,8335** |
| passed | false | false |

Kết quả gốc ở [annotation_evaluation_before_rework.json](../../outputs/annotation_evaluation_before_rework.json), bản hiện tại ở [annotation_evaluation.json](../../outputs/annotation_evaluation.json). F1 tăng 0,0251; TP tăng 23 và FN giảm 23. Mean IoU chỉ tính các cặp đã ghép, không chứng minh mọi box đều tốt.

Ngưỡng mặc định của script vẫn là F1 >= 0,80 và mean IoU >= 0,70. `passed` giữ **false**; tiếp tục pipeline theo hướng dẫn Lab Coach mà học viên cung cấp, không gọi kết quả này là đạt ngưỡng tự động.

Ví dụ cần thận trọng khi diễn giải: gold có xe van ID 1 ở drive_031, xe tải ID 2 ở drive_037/040/046 và xe buýt ID 3 ở drive_024/042/048; khác thứ tự ghi trong README. Dãy xe đỗ drive_001 không có các cặp gold tương ứng. Đây là quan sát cần xác nhận, không phải lý do quy mọi FP/FN cho gold; nhãn học viên vẫn có lỗi và thiếu vật thể.

## 5. Kết quả YOLO26n

### Phiên chạy đã hoàn tất

- Runtime: **local Mac Apple M5, MPS**, Python 3.14.10, Ultralytics 8.4.208, Torch 2.15.0.
- Checkpoint khởi tạo: **yolo26n.pt**; **50 epoch**, imgsz=640, seed=20260909.
- Điều chỉnh so với notebook Colab: dùng repo local có sẵn, batch=4 thay vì auto batch=-1, device=MPS, workers=0, amp=False. Lý do: phù hợp runtime Mac và kiểm soát bộ nhớ. Không thay đổi split, schema hoặc số epoch.
- `data-local.yaml` dùng đường dẫn tuyệt đối trong outputs; `data.yaml` gốc giữ nguyên. Train chỉ dùng **50 ảnh train**; val/chọn best/evaluate dùng đúng **10 ảnh eval**.
- Best checkpoint trong phiên chạy: `outputs/yolo26_runs/vehicle_annotation/weights/best.pt`, giữ trên máy và không commit.
- Notebook chạy đủ 6 ô code, có output thật, không có ô lỗi: [notebook local đã chạy](../../notebooks/day2_annotation_to_yolo_local_executed.ipynb).

| Chỉ số trên 10 ảnh eval | Giá trị thực tế |
|---|---:|
| Precision | **0.453994** |
| Recall | **0.232626** |
| mAP50 | **0.277476** |
| mAP50–95 | **0.206158** |

Nguồn: [model_eval_metrics.json](assets/model_eval_metrics.json). Đây là metrics model, khác F1/IoU của bộ nhãn ở mục 4.

### Các đầu ra

- [10 ảnh dự đoán eval](assets/eval_predictions/visuals/).
- [results.csv theo epoch](assets/yolo26_runs/vehicle_annotation/results.csv).
- [Confusion matrix](assets/yolo26_runs/vehicle_eval/confusion_matrix.png), [confusion matrix chuẩn hóa](assets/yolo26_runs/vehicle_eval/confusion_matrix_normalized.png), [PR curve](assets/yolo26_runs/vehicle_eval/BoxPR_curve.png).
- [Metrics từng lớp](assets/model_eval_per_class.json), trích từ bảng stdout trong ô evaluate; giá trị từng lớp được làm tròn như Ultralytics in ra.
- Hai thư mục bắt buộc `eval_predictions/` và `yolo26_runs/` đã đặt ở assets; không chứa checkpoint `.pt`.

### Trả lời bốn câu hỏi của notebook

1. **Lớp recall thấp nhất:** `bus` (ID 2 theo data.yaml), recall **0,000** trong bảng evaluate; car 0,647, truck 0,200, van 0,0833. Eval chỉ có **2 instance ID 2** ở drive_051 và drive_058 nên kết luận rất nhạy với từng lỗi. [drive_051](assets/eval_predictions/visuals/drive_051.jpg) minh họa cảnh tối/mờ và box chồng nhau ở xe đỏ phía trước. [drive_053](assets/eval_predictions/visuals/drive_053.jpg) có box `bus` ở xe phía trên bên phải nhưng xe buýt xanh lớn phía dưới chưa có box bus bao phần nhìn thấy. Cần kiểm tra lớp theo đúng ID và quy tắc gold; không suy từ recall 0 rằng model không hề tạo prediction mang tên bus ở bất kỳ ảnh nào.
2. **Nếu mAP50 cao nhưng mAP50–95 thấp:** cần kiểm vị trí, kích thước và độ sát box. Trên phiên thực tế, mAP50=0.2775 và mAP50–95=0.2062, cả hai còn thấp; không gọi đây là trường hợp mAP50 cao. Ngoài độ sát box, còn phải rà thiếu vật thể, class và phạm vi annotation. Các ảnh prediction là bằng chứng bổ sung, không dùng riêng chênh lệch mAP để kết luận nguyên nhân.
3. **Lỗi class liên quan guideline:** bản nhãn ban đầu có xe con trắng bị gán truck ở drive_046; đã sửa về car. Cần nhất quán SUV/taxi thuộc car, van khác bus và không tách hàng hóa trên xe thành xe riêng. Tập train mất cân bằng (car 501/679 box, truck chỉ 19); bảng eval cho thấy các lớp ngoài car kém hơn. Đây là các yếu tố cần kiểm, chưa đủ để quy quan hệ nhân quả. Quan sát lệch schema gold/README đã ghi ở mục 4 cũng cần được làm rõ khi diễn giải nhầm lớp.
4. **Vì sao không dùng 10 ảnh eval để train:** chúng giữ vai trò validation/chọn best checkpoint và đánh giá theo split của bài, không tham gia cập nhật trọng số. Vì eval đã dùng để chọn best nên không gọi là test set độc lập. Không chuyển eval vào train hoặc đổi split để làm đẹp điểm.

### Phần Colab còn cần học viên thao tác

Phiên trên hoàn thành trên Mac, **chưa phải phiên chạy Google Colab**. Mở [link Colab](https://colab.research.google.com/github/kasdie/K5-L2-Day02-LeQuangKhai-2A202603237-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb), Sao chép vào Drive, chọn GPU nếu được cấp, chạy tất cả, rồi bổ sung link bản Drive đã chạy vào báo cáo. Các metrics Colab nếu khác runtime local phải được ghi riêng, không gán số liệu local cho Colab.

## 6. Khó khăn và cách xử lý

| Bước | Quan sát | Cách xử lý / kết quả |
|---|---|---|
| Annotation | Cần tiếp tục nhãn CVAT và tự lưu trên Mac | Label Lab local, snapshot/backup và xuất 50 nhãn |
| Hỗ trợ gán nhãn | Muốn gợi ý từ nhãn đã có | YOLO11n học từ 12 ảnh ban đầu để đề xuất; học viên nhận/sửa. Không dùng model này thay YOLO26n của bài |
| Gold QC | F1 thấp và có dấu hiệu lệch ID/phạm vi gold | Giữ số liệu thật, sửa 3 ảnh, F1 tăng 0,4644 → 0,4895. Tiếp tục theo hướng dẫn Coach do học viên cung cấp |
| Notebook local | Môi trường thiếu PyYAML ở lần khởi động đầu | Cài bổ sung trong môi trường riêng, chạy lại thành công; notebook cuối không còn ô lỗi |
| Đóng gói output | Lệnh zip cũ tạo ZIP trong cùng cây đang nén | Sửa cell đóng gói: chỉ thêm hai thư mục kết quả và metrics, loại `.pt`, không tự nén chính ZIP |
| GitHub | Connector chỉ có quyền đọc repo kasdie | Dùng đăng nhập Git sẵn trên Mac, đẩy thường vào đúng repo kasdie; giữ upstream giảng viên riêng |
| Colab/LMS | Cần thao tác bằng tài khoản học viên | Link mở notebook đã sẵn; chưa ghi nhận bản Drive đã chạy hoặc xác nhận nộp LMS |

## 7. Tự đánh giá theo rubric

Đã có 50 nhãn đúng cấu trúc, mini guideline, gold evaluation trước/sau, một vòng sửa nhãn, phiên YOLO26n local đầy đủ, metrics và ảnh/log eval, cùng phần phân tích có AI hỗ trợ. F1 chưa đạt ngưỡng script, chất lượng nhãn còn hạn chế và được báo cáo thật theo hướng dẫn Coach. Cần học viên xác nhận nội dung báo cáo, hoàn thành link/phiên Colab theo yêu cầu lớp và tự nộp LMS. Không tự gán điểm hoặc gọi bước chưa làm là đã hoàn thành.

## 8. Công cụ AI và tài liệu tham khảo

- Codex/AI hỗ trợ lập trình Label Lab, chuyển CVAT, kiểm tra file, chạy evaluator, đối chiếu ảnh, chạy notebook local, tổng hợp kết quả và soạn bản nháp phân tích này. Học viên cần đọc lại, xác nhận và bổ sung nhận xét của mình theo RULES.md.
- Label Lab: vẽ tay, tự lưu, nhận/chỉnh gợi ý AI và xuất YOLO. Trạng thái 50/50 là xác nhận của học viên trong tool.
- YOLO11n + PyTorch MPS: hỗ trợ annotation từ 12 ảnh ban đầu. YOLO26n mới là model của pipeline cuối ở mục 5.
- Tài liệu gốc: [README](../../README.md), [GUIDE](../../GUIDE.md), [RULES](../../RULES.md), [SUBMISSION](../../SUBMISSION.md).
- [Ultralytics](https://docs.ultralytics.com/) và [PyTorch](https://pytorch.org/). Không đưa token, gold train hoặc checkpoint vào bài nộp.
