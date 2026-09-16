# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 28 skeleton, trung bình 16.18 khớp có v > 0 mỗi người
- Tổng: v=2 328 | v=1 125 | v=0 23

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 6 | 0 | 21% |
| 1 | left_eye | 21 | 7 | 0 | 25% |
| 2 | right_eye | 20 | 8 | 0 | 29% |
| 3 | left_ear | 13 | 15 | 0 | 54% |
| 4 | right_ear | 15 | 13 | 0 | 46% |
| 5 | left_shoulder | 25 | 3 | 0 | 11% |
| 6 | right_shoulder | 26 | 2 | 0 | 7% |
| 7 | left_elbow | 24 | 4 | 0 | 14% |
| 8 | right_elbow | 24 | 4 | 0 | 14% |
| 9 | left_wrist | 18 | 10 | 0 | 36% |
| 10 | right_wrist | 18 | 9 | 1 | 32% |
| 11 | left_hip | 21 | 7 | 0 | 25% |
| 12 | right_hip | 22 | 6 | 0 | 21% |
| 13 | left_knee | 19 | 7 | 2 | 25% |
| 14 | right_knee | 17 | 9 | 2 | 32% |
| 15 | left_ankle | 13 | 6 | 9 | 21% |
| 16 | right_ankle | 10 | 9 | 9 | 32% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
