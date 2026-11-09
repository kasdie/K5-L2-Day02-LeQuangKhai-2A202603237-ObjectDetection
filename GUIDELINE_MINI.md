# Mini guideline — Lê Quang Khải, 2A202603237

Bản đối chiếu sau self-QC ngày 10/11/2026, có AI hỗ trợ soạn thảo. Schema dưới đây giữ theo README của đề. Gold được cung cấp có dấu hiệu khác thứ tự ID; chưa tự đổi schema. Theo học viên cung cấp ngày 10/11/2026, Lab Coach cho phép tiếp tục pipeline khi F1 thấp nếu làm đủ bước. Các quyết định dưới đây là quy tắc cần áp dụng, không khẳng định mọi box hiện tại đã tuân thủ.

## Định nghĩa lớp

| ID | Lớp | Định nghĩa và điểm phân biệt theo đề |
|---:|---|---|
| 0 | `car` | Xe con, sedan, SUV, taxi và xe bán tải. SUV hoặc taxi không chuyển thành van chỉ vì thân xe lớn. |
| 1 | `truck` | Xe tải chở hàng, đầu kéo, xe chuyên dụng có đặc điểm xe tải. Xe bán tải thuộc `car` theo schema của đề. |
| 2 | `bus` | Xe buýt, xe khách và minibus chở khách có đặc điểm nhận dạng phù hợp. Phân biệt với thân van nhỏ. |
| 3 | `van` | Van/minivan chở người hoặc hàng; không dùng làm lớp mặc định cho mọi xe lớn hoặc khó nhận biết. |

## Quy tắc chung

- Mỗi xe thuộc schema có một box theo phần vật thể nhìn thấy; box sát biên, không gộp hai xe hoặc lấn nhiều nền.
- Xe tải có cả cabin và phần thân/thùng nhìn thấy cần một box của toàn xe; không coi hàng hóa trên xe là một xe riêng.
- Khi bị che, chỉ dùng bằng chứng nhìn thấy để phân lớp, không đoán phần khuất. Box bao phần nhìn thấy theo GUIDE của lớp.
- Xe bị cắt khung vẫn gán nếu đủ nhận biết lớp; box dừng tại biên ảnh.
- Xe rất nhỏ/mờ: phóng to để xem. Nếu chưa xác định được, ghi tên ảnh và hỏi giảng viên thay vì ép vào một lớp cho đủ.
- Xe đang đỗ vẫn gán theo README. Sau gold QC cần hỏi lại phạm vi này vì có xe đỗ trong `drive_001` không thấy nhãn tương ứng trong gold.
- Không gán mô tô/xe máy, phản chiếu hoặc hình xe trên quảng cáo.
- ID xuất YOLO luôn theo schema đã được xác nhận; nhãn có đúng 5 cột, không kèm confidence của gợi ý AI.

## Các ca thực tế cần đối chiếu

| Ảnh | Tình huống quan sát | Quyết định theo README / việc cần làm | Lý do |
|---|---|---|---|
| `drive_001` | Dãy xe đỗ phía trái có trong nhãn học viên nhưng không có cặp tương ứng trong gold. | Giữ quy tắc gán xe đỗ theo README; hỏi giảng viên về phạm vi gold trước khi xóa nhãn. | README phân lớp theo loại xe, không theo trạng thái chuyển động. |
| `drive_024` | Hai xe buýt lớn có nhãn học viên ID 2; gold dùng ID 3. Nhiều xe con trong ảnh còn chưa được gán. | Xe buýt thuộc `bus=2` theo README; xác nhận ID gold và rà lại tính đầy đủ của cả ảnh. | Lệch ID và thiếu xe là hai vấn đề cần kiểm tra riêng. |
| `drive_040` | Xe tải chở hàng màu đỏ trong gold mang ID 2; xe buýt lớn mang ID 3. Bản nhãn đầu có 2 box; sau sửa hiện có 17 box. | Theo README: tải là `truck=1`, buýt là `bus=2`; cần xác nhận schema và rà lại xe bỏ sót. | Không đổi schema chỉ để khớp điểm gold khi hướng dẫn còn mâu thuẫn. |
| `drive_042` | Xe tải chở nhiều hàng có các box học viên chồng lên phần hàng/cabin. | Rà lại thành một box bao phần xe nhìn thấy, phân lớp `truck` theo schema được xác nhận. | Hàng hóa không phải xe riêng; cần tránh box tách sai một vật thể. |
| `drive_046` | Xe con trắng ở mép dưới đang mang nhãn học viên `truck`; xe cứu hộ phía trên có đặc điểm xe tải. | Học viên đã sửa xe con trắng về `car` trong revision 323; giữ xe cứu hộ ở lớp riêng phù hợp schema. | Hình dạng xe quyết định lớp; màu trắng không phải đặc trưng phân lớp. |

## Trạng thái

Đã giữ bản đầu 652 box, học viên sửa drive_024/040/046 lên 679 box và lưu commit riêng; gold gốc giữ nguyên. F1 tăng 0,4644 → 0,4895. Học viên cần xác nhận guideline và phần nhận xét của mình; vấn đề schema/phạm vi vẫn ghi rõ trong báo cáo. Pipeline YOLO26n đã tiếp tục theo hướng dẫn Coach do học viên cung cấp.
