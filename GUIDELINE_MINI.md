# Mini guideline

- **Nhóm:** ______
- **Người gán:** Nguyễn Văn Tuyển
- **Ngày:** 16/09/2026

*Điền file này trong lúc gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.*

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file .SVG chung.
- Mọi người trong ảnh đều có đủ 17 điểm. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo cơ thể người, không theo bức ảnh.
- Bị che, còn trong khung -> v = 1, vẫn đặt chấm ở vị trí ước lượng.
- Ra ngoài mép ảnh -> v = 0, không đặt chấm.
- Không dùng Hidden (h) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| :--- | :--- | :--- |
| Hông của người mặc quần áo dài | Chấm ước lượng ngang thắt lưng (mấu chuyển xương đùi), đặt v = 1 | Vải áo/quần dài che mất xương, nhưng người vẫn ở trong khung hình nên không được bỏ sót v = 0. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Chấm vào vị trí lỗ tai giải phẫu, gắn v = 1 | Vẫn đoán được vị trí dựa vào trục đầu/mắt, giúp mô hình học được tỷ lệ khuôn mặt khi bị phụ kiện che. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Toàn bộ các khớp chân lọt ra ngoài mép ảnh đặt v = 0 và không chấm | Khớp nằm ngoài khung ảnh hoàn toàn, không thể phỏng đoán vùng pixel bên ngoài. |
| Cổ tay nằm sau tay lái / sau thân mình | Ước lượng điểm nối giữa cẳng tay và bàn tay, đặt v = 1 | Khớp chỉ bị vật cản che khuất tạm thời (Occluded), người vẫn trong ảnh. |
| Hai người chồng lên nhau | Gán đủ bộ 17 điểm cho từng người; phần thân người bị người kia đè lên thì chấm ước lượng và để v = 1 | Tránh nhầm lẫn nối xương giữa hai người và tránh xóa nhầm khớp của người đứng sau. |
| Người nhỏ đến mức nào thì không gán nữa | Bounding box có chiều cao dưới 30 pixel hoặc mờ nhòe không phân biệt được đầu gối/khuỷu tay thì bỏ qua | Quá ít pixel khiến việc ước lượng sai lệch lớn, làm nhiễu tập huấn luyện. |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

**Ca 1 - ảnh train_02.txt, người thứ 1, khớp left_shoulder / right_shoulder (và hông)**
- **Mơ hồ ở chỗ nào:** Người đứng quay lưng hoặc nghiêng, dễ nhầm bên trái/phải theo góc nhìn của mắt mình (màn hình) thay vì bên của cơ thể người.
- **Bạn quyết thế nào:** Đảo lại đúng quy ước giải phẫu: tay trái/chân trái của chính người trong ảnh, không lấy theo bên trái của người xem.
- **Vì sao:** Tool cảnh báo vai và hông nằm ngược chiều so với mắt; khi visualize bị chéo đường xương (xanh/cam lộn xộn).
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Model bị học vẹo xương, không phân biệt được tư thế quay trước hay quay sau lưng.

**Ca 2 - ảnh train_04.txt, người thứ 2, khớp đầu gối / cổ chân**
- **Mơ hồ ở chỗ nào:** Người ngồi hoặc bị vật cản che khuất chân nhưng toàn thân vẫn nằm trọn trong khung hình; phân vân giữa việc bỏ hẳn (v = 0) hay chấm mờ (v = 1).
- **Bạn quyết thế nào:** Chấm ước lượng khớp chân bị che và chuyển sang v = 1.
- **Vì sao:** Tool báo lỗi có 4 khớp v = 0 bất thường giữa ảnh. Quy tắc: còn trong ảnh mà bị che thì bắt buộc là v = 1.
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Model bị mất điểm OKS, học thói quen sai lầm là cứ bị che thì coi như biến mất khỏi cơ thể.

**Ca 3 - ảnh train_10.txt, người thứ 1, khớp cổ tay / khuỷu tay**
- **Mơ hồ ở chỗ nào:** Khớp tay khuất sau lưng áo, không thấy bề mặt da/quần áo cử động, lỡ tay để v = 0.
- **Bạn quyết thế nào:** Dựa theo hướng của bắp tay để chấm điểm khuỷu/cổ tay ước lượng và đặt cờ v = 1.
- **Vì sao:** Cả người nằm gọn giữa ảnh thì các khớp giải phẫu vẫn tồn tại bên trong khung hình, không được để ngoài biên (v = 0).
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Model không học được khả năng suy đoán các bộ phận bị che khuất (occlusion reasoning).

## 4. Sau khi so visibility report với bạn cùng nhóm

- **Khớp lệch %v=1 nhiều nhất:** left_ear (bạn 59% / họ 0%)
- **Nguyên nhân là guideline chưa rõ hay một trong hai bên gán sai:** Bên đối chiếu chưa nộp nhãn (thư mục đối chiếu có 0 skeleton, %v=1 bằng 0%), hoặc bên đối chiếu đang xóa nhầm các điểm bị tóc che (v = 0) thay vì giữ chấm ước lượng (v = 1).
- **Luật mới bổ sung vào mục 2 sau khi thống nhất:** Với tai bị tóc/mũ che nhưng đầu vẫn ở trong khung, bắt buộc phải chấm phỏng đoán dựa theo trục mắt và gán v = 1, nghiêm cấm xóa điểm hoặc để v = 0.
