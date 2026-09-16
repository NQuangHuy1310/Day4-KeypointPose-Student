# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 28 skeleton, trung bình 16.14 khớp có v > 0 mỗi người
- Tổng: v=2 375 | v=1 77 | v=0 24

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 24 | 4 | 0 | 14% |
| 1 | left_eye | 23 | 5 | 0 | 18% |
| 2 | right_eye | 24 | 4 | 0 | 14% |
| 3 | left_ear | 16 | 12 | 0 | 43% |
| 4 | right_ear | 17 | 11 | 0 | 39% |
| 5 | left_shoulder | 27 | 1 | 0 | 4% |
| 6 | right_shoulder | 28 | 0 | 0 | 0% |
| 7 | left_elbow | 25 | 3 | 0 | 11% |
| 8 | right_elbow | 24 | 4 | 0 | 14% |
| 9 | left_wrist | 24 | 4 | 0 | 14% |
| 10 | right_wrist | 23 | 4 | 1 | 14% |
| 11 | left_hip | 24 | 3 | 1 | 11% |
| 12 | right_hip | 25 | 2 | 1 | 7% |
| 13 | left_knee | 18 | 8 | 2 | 29% |
| 14 | right_knee | 20 | 4 | 4 | 14% |
| 15 | left_ankle | 16 | 5 | 7 | 18% |
| 16 | right_ankle | 17 | 3 | 8 | 11% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
