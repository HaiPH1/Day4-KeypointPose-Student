# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 28 skeleton, trung bình 16.18 khớp có v > 0 mỗi người
- Tổng: v=2 328 | v=1 125 | v=0 23

So sánh với `ban_cung_nhom/dataset.duypx/dataset/labels/train` (28 skeleton).
Cột **lệch** là hiệu số phần trăm v=1 - chỗ nào lệch nhiều nhất là chỗ guideline chưa nói rõ.

| # | Khớp | %v=1 (bạn) | %v=1 (đối chiếu) | lệch |
| ---: | --- | ---: | ---: | ---: |
| 11 | left_hip | 25% | 7% | 18 |
| 9 | left_wrist | 36% | 21% | 14 |
| 12 | right_hip | 21% | 11% | 11 |
| 1 | left_eye | 25% | 32% | 7 |
| 4 | right_ear | 46% | 39% | 7 |
| 13 | left_knee | 25% | 32% | 7 |
| 16 | right_ankle | 32% | 25% | 7 |
| 6 | right_shoulder | 7% | 0% | 7 |
| 8 | right_elbow | 14% | 11% | 4 |
| 14 | right_knee | 32% | 29% | 4 |
| 5 | left_shoulder | 11% | 7% | 4 |
| 0 | nose | 21% | 21% | 0 |
| 2 | right_eye | 29% | 29% | 0 |
| 3 | left_ear | 54% | 54% | 0 |
| 7 | left_elbow | 14% | 14% | 0 |
| 10 | right_wrist | 32% | 32% | 0 |
| 15 | left_ankle | 21% | 21% | 0 |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
