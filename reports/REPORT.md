# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: ______   Nhóm: HaiPH + duypx   Ngày: 2026-09-16

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 328 / 125 / 23 |
| Thời gian trung bình mỗi ảnh | |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` - 54%
2. `right_ear` - 46%
3. `left_wrist` - 36%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.920 | 0.924 |
| OKS@0.50 | 0.931 | 0.931 |
| OKS@0.75 | 0.931 | 0.931 |
| Lỗi `dao_trai_phai` | 1 | 1 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 2 | 0 |
| Lỗi `thieu_nguoi` | 1 | 1 |
| Lỗi `truot_han` | 3 | 3 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

Số thứ tự người theo bản export cuối cùng (CVAT đổi thứ tự người ở `train_01`, `train_04`, `train_14` giữa các lần export).

- `train_01` người 1 (bên trái): `left_wrist` từ `v=0` -> `v=1`, đặt chấm ước lượng
- `train_01` người 2 (bên phải): `right_wrist` từ `v=0` -> `v=1`, đặt chấm ước lượng
- `train_04` người 1 (bên trái, bị cắt ở đáy ảnh): kéo `left_hip`, `right_hip` vào trong khung (từ y = 458 / 457 px lên 452 px), giữ `v=2` - sửa lỗi định dạng "v=2 nhưng toạ độ ra ngoài ảnh"
- `train_10` người 1: kéo `left_hip`, `right_hip` lên 16 px / 10 px; đầu gối và cổ chân giữ `v=0` vì hông đã sát đáy ảnh nên gối nằm dưới mép ảnh (lần sửa giữa chừng đặt gối ở y = 379 / 375 px với `v=2` -> checker chặn, đã trả về `v=0`)
- `train_11` người 1: `left_knee`, `right_knee` từ `v=0` -> `v=1`, đặt chấm ước lượng sau bàn; `left_hip`, `right_hip` từ `v=2` -> `v=1` (bị bàn và con mèo che)
- `train_12` người 1: `left_hip` từ `v=2` -> `v=1`
- `train_13` người 1: `left_knee`, `right_knee` từ `v=0` -> `v=2`, đặt chấm ở sát mép đáy ảnh
- `train_14` người 1: `left_knee` từ `v=1` -> `v=2`

**Giữ nguyên sau khi hỏi giáo viên** (công cụ vẫn báo, nhưng giáo viên xác nhận nhãn không cần sửa):

- `train_13`: `thieu_nguoi` - người nhỏ ở nền, sát mép trái ảnh, không gán.
- `train_15` người 1 (người đội mũ bảo hiểm đen, bên trái): `dao_trai_phai` kèm `truot_han` ở `right_ear`, `left_elbow`, `right_elbow` - giữ nguyên cách gán trái/phải.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Công cụ báo một lỗi `dao_trai_phai` ở `train_15` người 1 (OKS 0.394). Ảnh không khó về kích thước - người lớn, rõ nét, không bị cắt - nhưng người đó đứng nghiêng, cúi về phía xe, mũ bảo hiểm che mặt, nên không dùng mắt/mũi để xác định hướng người được; phải dựa vào vai và thân. Tôi đã hỏi giáo viên và được xác nhận nhãn này giữ nguyên, không phải sửa. Ngoài ca này, không có lỗi đảo trái/phải nào khác trong 20 ảnh.

## 3. Kiểm chéo

Bạn cùng nhóm: duypx

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| `left_hip` | 25% | 7% | 18 | Guideline: chưa có mốc chung để định vị hông và chọn `v=2`/`v=1` cho người mặc quần áo; bài bạn có 3 ảnh lệch vị trí hông. Sau rework tôi đổi thêm hông ở `train_11`, `train_12` sang `v=1` (bị vật che) nên lệch tăng từ 11 lên 18 |
| `left_wrist` | 36% | 21% | 14 | Guideline chưa nói cổ tay sau tay lái / sau thân gắn cờ gì. Bài tôi lúc đầu cũng không nhất quán (2 cổ tay ở `train_01` để `v=0`), sau rework đổi sang `v=1` nên lệch tăng từ 11 lên 14 |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- Hông: đặt ở nếp gấp đùi - thân; `v=2` khi đường viền thân và cạp quần cho định vị chắc chắn, `v=1` khi có vật che thêm (tay, túi, xe, bàn, người khác).
- Cổ tay sau tay lái / sau thân: luôn `v=1` + đặt chấm theo hướng cẳng tay, không bao giờ `v=0`.
- Người bị cắt ở đáy ảnh: hông đã sát mép đáy thì đầu gối và cổ chân là `v=0` (ra ngoài khung), không đặt chấm dưới mép ảnh.
- Người nhỏ, mờ ở nền (cỡ người sát mép trái `train_13`): không gán - đã hỏi giáo viên xác nhận.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50 | 0.9785 | 0.9600 | -0.0185 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   `pose_mAP50-95` tăng rất nhẹ, từ 0.6853 lên 0.6908 (+0.0055); `pose_mAP50` giữ nguyên 0.845. Mức
   thay đổi này quá nhỏ để kết luận 20 ảnh của tôi làm model tốt lên: tập test chỉ có 10 ảnh, 13 người,
   nên một khớp đặt lệch hay đúng hơn đã đủ đẩy số lên xuống cỡ này. `yolo26n-pose` vốn đã train trên
   COCO với cùng bộ 17 điểm, nên 20 ảnh gần như không dạy thêm được gì mới về vị trí khớp. Thứ nhích
   lên là độ chặt của khớp (`mAP50-95` dùng ngưỡng OKS cao), trong khi `pose_recall` đứng yên ở 0.8462 =
   11/13 người: model trước và sau đều bỏ sót đúng 2 người trong tập test. Cái bị hỏng nhẹ là box:
   `box_mAP50` giảm 0.0185 và `box_mAP50-95` giảm 0.0078 - fine-tune 80 epoch trên 20 ảnh kéo model
   về phía phân bố của 20 ảnh này, làm khung người trên ảnh test khớp kém đi một chút.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

   Sau fine-tune, `box_mAP50-95` = 0.8041 và `pose_mAP50-95` = 0.6908, chênh 0.1133 (model gốc chênh
   0.1266). Ở ngưỡng lỏng: `box_mAP50` 0.960 so với `pose_mAP50` 0.845, chênh 0.115. Model tìm
   **người** dễ hơn tìm **khớp**. Box chỉ cần 4 cạnh trùng đủ với khung thật (IoU), và một người là một
   vật thể lớn, rõ. Pose phải đặt đúng cả 17 điểm, mỗi điểm nằm trong bán kính dung sai riêng của OKS -
   rất nhỏ với mắt, mũi, tai - và phải đoán cả khớp bị che mà ảnh không cho thấy. Một người có box
   đúng hoàn toàn vẫn có thể có OKS thấp chỉ vì cổ tay, cổ chân bị che hoặc đặt lệch.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

   `test_02`: nhãn phát sẵn chỉ có **1 người** (người đứng bên phải, bóng đen trên nền trời), nhưng model
   tìm ra **2 người**. Người thứ hai có box nhỏ ở góc dưới bên trái với độ tin cậy chỉ 0.31 (ngưỡng dự đoán
   là 0.25) - đúng vị trí **con chim đậu trên bờ tường**. Model gán cả một skeleton người lên con chim.
   Theo bốn loại của slide 43, đây là **trượt hẳn** ở mức cả skeleton: mọi chấm đều rơi vào chỗ không có
   khớp người nào; nói cách khác là một dự đoán thừa người (false positive). Lý do nhiều khả năng: ảnh đen
   trắng, người thật cũng chỉ là một bóng đen không có chi tiết, nên với model một bóng đen nhỏ có đầu
   và thân trên nền sáng trông giống người. Người thật (độ tin cậy 0.90) vẫn được tìm đúng. Đây là ảnh
   duy nhất model đếm lệch số người: 9 ảnh còn lại số người model tìm được khớp đúng với nhãn phát sẵn.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   Thấp nhất là `train_15` với OKS 0.259 - người 1, người đội mũ bảo hiểm đen đứng cúi về phía xe (người
   2 của ảnh này đạt 0.872). Bằng chứng về ai đúng:
   - Gold cũng lệch với tôi đúng ở người này: OKS nhãn của tôi so với gold chỉ 0.394, công cụ chấm báo
     `dao_trai_phai` (đổi toàn bộ cặp trái/phải thì OKS tăng hẳn) kèm `truot_han` ở hai khuỷu tay.
   - Model đã được fine-tune trên chính nhãn của tôi cho ảnh này mà vẫn lệch với tôi, thấp hơn cả gold. Tức
     là cách đọc hướng người mà model học từ COCO mạnh hơn một nhãn đơn lẻ của tôi. Nhiều khả năng model
     đang đọc hướng người giống gold.
   - Ngược lại, ảnh không cho căn cứ chắc chắn: người này gần như quay lưng, cúi người, mũ che hết mặt,
     hai tay khuất sau xe - không có mắt/mũi để xác định hướng. Giáo viên đã xem và chấp nhận giữ nguyên
     nhãn của tôi.

   Kết luận: theo quy ước COCO thì model và gold đúng hơn; nhãn của tôi là một cách đọc được chấp nhận
   của một ca mơ hồ thật. Ở hai ảnh thấp kế tiếp thì khác: `train_06` (0.653) và `train_14` (0.679) có nhãn
   của tôi gần gold (lần lượt 0.922 và trên 0.93), nên ở đó **model sai**, không phải nhãn.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

   Có. Hai ảnh tệ nhất của tôi theo gold cũng là hai ảnh đứng đầu danh sách model lệch với tôi:

   | Ảnh | OKS tôi vs gold | OKS model vs tôi | Số người (tôi / model / gold) |
   | --- | ---: | ---: | --- |
   | `train_15` người 1 | 0.394 | 0.259 | 2 / 2 / 2 |
   | `train_13` người 2 | 0.793 | 0.638 | 2 / 3 / 3 |

   Điều đó nói rằng cái khó nằm ở **bức ảnh**, không phải do tôi gán ẩu. `train_15`: người quay lưng, mũ che
   mặt, tay khuất sau xe - thiếu đúng những manh mối người và model dùng để phân biệt trái/phải. `train_13`:
   phố đông, người chồng lên nhau, có một người nhỏ ở nền - tôi không gán người này (giáo viên cho phép),
   còn model và gold đều tính là 3 người. Những ảnh như vậy nên được ghi thành ca mơ hồ trong guideline
   (đã ghi `train_15` ở ca 2) thay vì chỉ sửa nhãn. Ngược lại, `train_06` và `train_14` chỉ khó với model,
   nên độ khó của model không phải lúc nào cũng trùng độ khó của người gán.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->

`train_11`, người 1 (người ngồi sau bàn picnic), `left_knee`. Bàn và con mèo che hết phần chân nên không nhìn thấy gối. Nhưng hông của người này nằm ở y ≈ 0.7 chiều cao ảnh, và phía dưới còn gần 30% ảnh là mặt bàn - tức là phần chân vẫn nằm trong khung, chỉ bị vật che. Vì vậy chọn `v=1` và đặt chấm ước lượng dưới mặt bàn theo tư thế ngồi, không chọn `v=0` - `v=0` chỉ dành cho khớp đã ra ngoài mép ảnh.
