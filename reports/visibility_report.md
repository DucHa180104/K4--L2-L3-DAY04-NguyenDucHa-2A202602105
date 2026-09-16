# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 27 skeleton, trung bình 16.11 khớp có v > 0 mỗi người
- Tổng: v=2 400 | v=1 35 | v=0 24

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 26 | 1 | 0 | 4% |
| 1 | left_eye | 24 | 3 | 0 | 11% |
| 2 | right_eye | 23 | 4 | 0 | 15% |
| 3 | left_ear | 22 | 5 | 0 | 19% |
| 4 | right_ear | 24 | 2 | 1 | 7% |
| 5 | left_shoulder | 27 | 0 | 0 | 0% |
| 6 | right_shoulder | 26 | 1 | 0 | 4% |
| 7 | left_elbow | 25 | 1 | 1 | 4% |
| 8 | right_elbow | 25 | 2 | 0 | 7% |
| 9 | left_wrist | 22 | 4 | 1 | 15% |
| 10 | right_wrist | 24 | 3 | 0 | 11% |
| 11 | left_hip | 25 | 2 | 0 | 7% |
| 12 | right_hip | 25 | 2 | 0 | 7% |
| 13 | left_knee | 20 | 4 | 3 | 15% |
| 14 | right_knee | 24 | 1 | 2 | 4% |
| 15 | left_ankle | 18 | 0 | 9 | 0% |
| 16 | right_ankle | 20 | 0 | 7 | 0% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
