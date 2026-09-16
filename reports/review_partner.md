# Review bài bạn cùng nhóm

Người gán: Phạm Xuân Duy   Người kiểm: Phạm Hữu Hải   Ngày: 2026-09-16

Bài được kiểm: `ban_cung_nhom/dataset.duypx/dataset/labels/train` (20 file, 28 skeleton).

Đã chạy:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels ban_cung_nhom/dataset.duypx/dataset/labels/train
python3 tools/visualize_pose.py --images dataset/images/train --labels ban_cung_nhom/dataset.duypx/dataset/labels/train --out /tmp/vis_review --names
python3 tools/visibility_report.py --labels dataset/labels/train --compare ban_cung_nhom/dataset.duypx/dataset/labels/train --markdown reports/visibility_compare.md
```

Kết quả `check_pose_labels.py`: **ĐẠT định dạng**, 0 lỗi, 2 cảnh báo (cùng ở `train_16.txt` người 1).
Cờ visibility: v=2 347 | v=1 108 | v=0 21.

## Reviewer checklist

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | Mọi skeleton đủ 17 điểm. `train_13` không gán người nhỏ ở nền sát mép trái - giáo viên xác nhận không cần gán |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☑ | Checker cảnh báo `train_16` người 1 (vai/hông ngược chiều so với mắt). Soi ảnh: cầu thủ đang bật nhảy, thân vặn, mắt không dùng làm mốc được. Gán theo thân là đúng - **không phải lỗi** |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | `train_16` hai người chồng lên nhau nhưng xương của ai nằm đúng người đó |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☑ | Không thấy khớp bị che nào bị xoá |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☑ | 21 khớp v=0, không có cảnh báo "nằm gọn giữa ảnh" |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | Không có điểm v=2 nào ngoài khung hoặc ở (0,0) |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☐ | Không kiểm được - chỉ nhận thư mục nhãn YOLO, không có file JSON export |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | Checker đọc được cả 20 file, không lỗi số cột |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ | `reports/visibility_compare.md` |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☐ | Không kiểm được - chưa nhận `GUIDELINE_MINI.md` của bạn |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | 0 lỗi |

## Lỗi tìm được

Các dòng lệch nhẹ lấy từ `evaluate_pose_annotations.py`
chạy trên bài của bạn sau khi gold mở (OKS trung bình 0.923, OKS@0.75 0.966, 0 lỗi đảo trái/phải).

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_04` | 1 | `left_hip`, `right_hip` | Lệch nhẹ (76-87 px, ~1.1-1.2 lần dung sai) | Đặt lại hai chấm hông theo mốc nếp gấp đùi - thân |
| `train_03` | 2 | `left_hip`, `right_hip` | Lệch nhẹ (50-51 px) | Đặt lại hai chấm hông theo mốc nếp gấp đùi - thân |
| `train_12` | 1 | `left_ankle` | Lệch nhẹ (84 px, 1.4 lần dung sai) | Kéo chấm về đúng mắt cá |
| `train_20` | 1 | `right_wrist` | Lệch nhẹ (39 px, 1.7 lần dung sai) | Kéo chấm về giữa cổ tay |
| `train_04` | 2 | `left_wrist` | Lệch nhẹ (51 px, 1.5 lần dung sai) | Kéo chấm về giữa cổ tay |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: **đặt hông lệch** (2 ảnh, 4 chấm: `train_03`, `train_04`). Hông cũng là một trong hai khớp lệch
  `%v=1` nhiều nhất giữa hai bài (`left_hip` 25% của tôi so với 7% của bạn; khớp kia là `left_wrist` 36% so với 21%).
- Nó là lỗi **guideline chưa rõ**: hai người chưa thống nhất hông của người mặc quần áo dài được
  xác định từ mốc nào và gắn cờ gì.
