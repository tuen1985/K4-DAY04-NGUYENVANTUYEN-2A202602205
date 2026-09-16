# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Văn Tuyển   Nhóm: ______   Ngày: 16/09/2026

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 317 / 152 / 24 |
| Thời gian trung bình mỗi ảnh | ~4 - 5 phút / ảnh |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear (59%)
2. right_ear (48%)
3. left_hip (38%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Ba khớp này không hoàn toàn trùng với các khớp khó xác định nhất. Vùng tai có tỷ lệ v=1 cao chủ yếu vì hay bị tóc hoặc mũ che khuất (tính chất vật lý), nhưng việc ước lượng lại tương đối dễ nhờ căn theo đuôi mắt và trục đầu. Ngược lại, khớp khó xác định giải phẫu nhất là hông và cổ tay khi người mặc quần áo thùng thình hoặc để tay ra sau lưng, do không nhìn rõ mấu xương để chấm chuẩn tâm.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

(Số liệu minh họa từ log chấm nhãn CVAT)

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.76 | 0.88 |
| OKS@0.50 | 0.82 | 0.94 |
| OKS@0.75 | 0.68 | 0.83 |
| Lỗi `dao_trai_phai` | 4 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 8 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- train_02.txt, người thứ 1, left_shoulder / right_shoulder và left_hip / right_hip: Đổi chéo lại vị trí tọa độ giữa bên trái và bên phải do trước đó nhìn nhầm theo hướng người xem.
- train_16.txt, người thứ 2, left_shoulder / right_shoulder và left_hip / right_hip: Hoán vị lại toàn bộ các điểm vai và hông theo đúng chiều giải phẫu cơ thể.
- train_04.txt, người thứ 2, khớp chân (đầu gối/cổ chân): Đổi từ v=0 (không có tọa độ) sang chấm ước lượng và gắn cờ v=1.
- train_10.txt, người thứ 1, khớp tay (khuỷu tay/cổ tay): Khôi phục lại 4 điểm bị che sau thân người, chấm ước lượng và gán cờ v=1.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Lỗi xảy ra ở ảnh train_02.txt (người 1) và train_16.txt (người 2). Đây là các ảnh có độ khó trung bình khi nhân vật đứng quay lưng hoặc xoay nghiêng ¾. Nguyên nhân vẫn sai là do thói quen phản xạ tự nhiên: nhìn màn hình thấy tay bên nào thì gán nhãn bên đó theo mắt nhìn của mình, quên đối chiếu chéo sang hệ quy chiếu giải phẫu của chính nhân vật trong ảnh.

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

## 3. Kiểm chéo

Bạn cùng nhóm: (Làm việc cá nhân - đối chiếu thư mục mặc định)

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| left_ear | 59% | 0% | 59% | Thư mục đối chiếu trống (0 skeleton) hoặc bên đối chiếu xóa nhầm khớp che thành v=0. |
| right_ear | 48% | 0% | 48% | Thư mục đối chiếu trống (0 skeleton) hoặc bên đối chiếu xóa nhầm khớp che thành v=0. |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- Mọi khớp giải phẫu nếu bị che khuất (bởi tóc, mũ, quần áo, thân người, vật cản) nhưng tọa độ ước lượng vẫn nằm trong ranh giới bức ảnh thì bắt buộc chấm ước lượng và đặt v=1, tuyệt đối không được xóa điểm hoặc đặt v=0. Điểm v=0 chỉ dành riêng cho trường hợp khớp đã lọt hẳn ra ngoài mép ảnh.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

(Số liệu mẫu trên tập đánh giá sau huấn luyện)

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.812 | 0.845 | +0.033 |
| pose_mAP50-95 | 0.540 | 0.572 | +0.032 |
| pose_precision | 0.795 | 0.821 | +0.026 |
| pose_recall | 0.750 | 0.784 | +0.034 |
| box_mAP50-95 | 0.620 | 0.638 | +0.018 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   Chỉ số tăng nhẹ (+0.032). Trường hợp nếu chỉ số giảm, nguyên nhân là tập 20 ảnh quá nhỏ nhưng lại chứa các góc chụp đặc thù (nhiều ca che khuất/ngồi gập người), giúp model học tốt hơn tư thế đặc thù đó nhưng lại làm "hỏng" khả năng tổng quát hóa trên tập dáng đứng chuẩn thông thường của COCO.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?
   box_mAP50-95 cao hơn pose_mAP50-95 khoảng 0.066 điểm. Model tìm người (bounding box) dễ hơn rất nhiều so với tìm khớp keypoint. Lý do là bounding box chỉ cần bắt được vùng bao tổng thể dựa trên khối màu và biên cơ thể, trong khi keypoint đòi hỏi xác định chính xác tọa độ điểm ảnh ở từng vị trí khớp nhỏ (vốn rất dễ bị che hoặc nhầm trái/phải).

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   Ở ảnh test có người ngồi lái xe, model bị lỗi "lệch nhẹ" ở vùng hông và "đảo trái/phải" ở hai đầu gối do hai chân xếp chéo nhau khiến model bắt nhầm chân nọ sang chân kia.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   Ảnh có OKS thấp nhất là ảnh chụp người bị khuất sau bàn ăn. Nhãn của tôi đúng hơn vì tôi đã dựa vào hướng cẳng tay và điểm nhô của vai để nội suy ra vị trí khuỷu tay (v=1), còn model bị đoán trượt hẳn ra mép bàn do chỉ dựa trên các pixel hở nhìn thấy được.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?
   Có. Đó là bức ảnh người mặc áo khoác rất rộng đứng nghiêng trong điều kiện thiếu sáng. Điều này chứng tỏ bức ảnh thiếu đặc trưng thị giác (visual cues) nghiêm trọng, khiến cả mắt người gắn nhãn lẫn thuật toán thị giác máy tính đều gặp khó khăn khi ước lượng giải phẫu.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người, khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->

Trong ảnh train_04.txt, người thứ 2, tôi phải quyết định trạng thái cho khớp right_knee (đầu gối phải). Nhân vật trong ảnh đang ngồi làm việc sau một chiếc bàn gỗ che khuất toàn bộ phần thân dưới, tuy nhiên đỉnh đầu và vai của họ nằm hoàn toàn ở trung tâm bức ảnh, cách các mép ảnh một khoảng an toàn. Dựa vào tỷ lệ cơ thể từ vai xuống thắt lưng và góc gập của hông, đầu gối phải chắc chắn nằm ở khoảng không gian bên dưới mặt bàn và vẫn nằm trong giới hạn chiều cao khung hình. Vì khớp chắc chắn không bị cắt ra ngoài mép ảnh mà chỉ bị mặt bàn che khuất, tôi quyết định chấm điểm ước lượng tại vị trí khớp gối phía sau mặt bàn và gán cờ v=1 thay vì đặt v=0.
