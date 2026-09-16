# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.97 khớp có v > 0 mỗi người
- Tổng: v=2 331 | v=1 132 | v=0 30

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 7 | 0 | 24% |
| 1 | left_eye | 18 | 11 | 0 | 38% |
| 2 | right_eye | 21 | 8 | 0 | 28% |
| 3 | left_ear | 10 | 19 | 0 | 66% |
| 4 | right_ear | 15 | 14 | 0 | 48% |
| 5 | left_shoulder | 26 | 3 | 0 | 10% |
| 6 | right_shoulder | 27 | 2 | 0 | 7% |
| 7 | left_elbow | 23 | 6 | 0 | 21% |
| 8 | right_elbow | 25 | 4 | 0 | 14% |
| 9 | left_wrist | 18 | 11 | 0 | 38% |
| 10 | right_wrist | 21 | 7 | 1 | 24% |
| 11 | left_hip | 17 | 12 | 0 | 41% |
| 12 | right_hip | 24 | 4 | 1 | 14% |
| 13 | left_knee | 17 | 7 | 5 | 24% |
| 14 | right_knee | 20 | 4 | 5 | 14% |
| 15 | left_ankle | 13 | 7 | 9 | 24% |
| 16 | right_ankle | 14 | 6 | 9 | 21% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
