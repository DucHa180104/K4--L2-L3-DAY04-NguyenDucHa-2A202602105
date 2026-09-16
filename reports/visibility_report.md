# Visibility report

- Thư mục nhãn: `C:\Users\ADMIN\OneDrive - Phenikaa University\Desktop\Vin\Day4\K4-DAY04-NguyenDucHa-2A202602105\dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 16.31 khớp có v > 0 mỗi người
- Tổng: v=2 429 | v=1 44 | v=0 20

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 28 | 1 | 0 | 3% |
| 1 | left_eye | 26 | 3 | 0 | 10% |
| 2 | right_eye | 25 | 4 | 0 | 14% |
| 3 | left_ear | 22 | 7 | 0 | 24% |
| 4 | right_ear | 26 | 2 | 1 | 7% |
| 5 | left_shoulder | 28 | 1 | 0 | 3% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 26 | 2 | 1 | 7% |
| 8 | right_elbow | 27 | 2 | 0 | 7% |
| 9 | left_wrist | 23 | 5 | 1 | 17% |
| 10 | right_wrist | 26 | 3 | 0 | 10% |
| 11 | left_hip | 26 | 3 | 0 | 10% |
| 12 | right_hip | 27 | 2 | 0 | 7% |
| 13 | left_knee | 21 | 6 | 2 | 21% |
| 14 | right_knee | 26 | 2 | 1 | 7% |
| 15 | left_ankle | 22 | 0 | 7 | 0% |
| 16 | right_ankle | 22 | 0 | 7 | 0% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
