# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Đức Hà

Nhóm: Cá nhân

Ngày: 2026-09-16

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| `v=2 / v=1 / v=0` | 429 / 44 / 20 |
| Thời gian trung bình mỗi ảnh | Khoảng 3 phút |

Ba khớp có tỉ lệ `v=1` cao nhất theo visibility report chạy trên bản export rework:

1. `left_ear`: 7/29 người, khoảng 24%.
2. `left_knee`: 6/29 người, khoảng 21%.
3. `left_wrist`: 5/29 người, khoảng 17%.

Các khớp trên thường có nguy cơ bị che bởi tóc, góc quay đầu hoặc tư thế tay. Tuy nhiên, bảng đếm visibility chỉ cho biết tần suất dùng `v=1`, không tự chứng minh tọa độ đã được đặt đúng. Kết quả so với gold vẫn cần được dùng để phát hiện các trường hợp đặt sai trạng thái hoặc sai vị trí.

## 2. Chấm với gold

Tôi đã thực hiện rework trong CVAT và export lại bằng COCO Keypoints 1.0. Bản export mới đạt định dạng, có 20 ảnh và 29 skeleton.

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9295 | 0.9270 |
| OKS@0.50 | 0.9310 | 1.0000 |
| OKS@0.75 | 0.8966 | 0.9655 |
| Người trong gold / ghép được | 29 / 27 | 29 / 29 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `thieu_nguoi` | 2 | 0 |
| Lỗi `xoa_khop_bi_che` | 1 | 0 |
| Lỗi `truot_han` | 1 | 1 |
| Lỗi `lech_nhe` | 11 | 12 |

Kết quả sau rework vẫn đạt mức **Xuất sắc**, không còn thiếu người, thừa người, đảo trái/phải, nhầm người hoặc lỗi xóa khớp bị che. Tôi đã thực hiện các thay đổi sau:

- `train_13.jpg`: bổ sung hai skeleton còn thiếu. Sau rework, cả ba người đều ghép được với gold, với OKS lần lượt là 0.8125, 0.9132 và 0.9680.
- `train_11.jpg`: đặt lại bốn khớp phần thân dưới từng dùng `v=0`; checker không còn cảnh báo người nằm trong ảnh nhưng có nhiều khớp outside. Hai mắt cá hiện được ghi `v=2` dù bị mèo và mặt bàn che, nên `v=1` sẽ nhất quán hơn với guideline nếu tiếp tục chỉnh.
- `train_12.jpg`, người số 1, `left_ankle`: bổ sung lại tọa độ trong khung, nên evaluator không còn báo `xoa_khop_bi_che`. Bản export hiện ghi cờ `v=2`; do chân bị thùng hàng che, `v=1` sẽ phù hợp hơn nếu chỉnh tiếp theo đúng guideline.

Các lỗi còn lại là một lỗi `truot_han` tại `train_19.jpg`, người số 2, `right_wrist`, cùng 12 trường hợp lệch nhẹ. Lỗi trượt cổ tay cần được ghi nhận nhưng không làm bài rơi khỏi cổng đạt; các lỗi lệch nhẹ là nhóm ít hại nhất.

Ngoài ra, evaluator báo 46 trường hợp cờ khác gold và 81 trường hợp gold để `v=0` trong khi tôi có gán. Theo rubric, hai nhóm này là thông tin chẩn đoán và không bị trừ OKS.

**Lỗi đảo trái/phải:** Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.

## 3. Kiểm chéo

Tại thời điểm hoàn thiện báo cáo, tôi chưa có bài của bạn cùng nhóm và chưa thực hiện kiểm chéo. `outputs/visibility_report.json` cũng đang có trường `comparison: null`, vì vậy chưa có số liệu để điền chênh lệch `%v=1`, tên người review hoặc lỗi do reviewer tìm thấy.

Tôi chưa bổ sung luật mới từ hoạt động kiểm chéo. Phần này cần được cập nhật nếu có reviewer và có kết quả so sánh thật; tôi không tự tạo số liệu hoặc tên người review.

## 4. Model

| Chỉ số | `yolo26n-pose` gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. **`pose_mAP50-95` thay đổi bao nhiêu?**
   `pose_mAP50-95` tăng từ 0.6853 lên 0.6908, tức tăng 0.0055. Đây là mức tăng nhỏ trên tập test hiện có. Chỉ từ con số này chưa thể kết luận chắc chắn 20 ảnh đã dạy model một đặc trưng cụ thể nào; cần thêm dữ liệu và nhiều lần chạy để kiểm tra độ ổn định.

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu?**
   Sau fine-tune, `box_mAP50-95` là 0.8041 và `pose_mAP50-95` là 0.6908, chênh 0.1133. Trên tập test này, model phát hiện vùng người dễ hơn định vị chính xác các keypoint vì keypoint yêu cầu tọa độ chi tiết hơn bounding box.

3. **Một ảnh test model đoán sai và loại lỗi:**
   Trong lần chạy Colab, tôi chỉ lưu các chỉ số tổng hợp trong `outputs/eval_model.json` mà chưa lưu ảnh dự đoán từ mục 5 của notebook. Vì vậy, tôi chưa có bằng chứng để nêu chính xác một ảnh test và loại lỗi tương ứng; đây là phần evidence còn thiếu của bài.

4. **Ảnh có OKS thấp nhất giữa nhãn của tôi và model:**
   Ghi chép hiện có nêu `train_15.jpg` có OKS model so với nhãn khoảng 0.559. Tuy nhiên, notebook hiện không lưu output per-image để kiểm chứng lại con số này. Khi so nhãn với gold, người số 1 trong `train_15.jpg` đạt OKS 0.708 và được xếp vào nhóm lệch nhẹ; vì vậy chưa thể kết luận tuyệt đối model hay nhãn đúng nếu không xem lại ảnh trực quan.

5. **Ảnh tôi gán tệ nhất có đồng thời là ảnh model đoán tệ nhất không?**
   Sau rework, skeleton có OKS thấp nhất so với gold là người số 1 trong `train_15.jpg`, với OKS khoảng 0.708. Ghi chép về so sánh model với nhãn cũng nêu `train_15.jpg` có OKS thấp nhất, khoảng 0.559. Hai kết quả cùng chỉ về một ảnh khó, nhưng output per-image của notebook chưa được lưu nên tôi không kết luận xa hơn về nguyên nhân.

## 5. Một rule evidence tôi đã dùng

Ở `train_12.jpg`, người số 1, `left_ankle` ban đầu được ghi là `v=0`, dù khớp vẫn nằm trong ảnh và bị thùng hàng che. Trong vòng rework, tôi đã bổ sung lại tọa độ, nhờ đó evaluator không còn báo lỗi xóa khớp bị che. Quy tắc rút ra là: khi khớp vẫn nằm trong khung hình và có thể suy ra từ phần cơ thể liền kề, cần đặt điểm ước lượng với `v=1`; chỉ dùng `v=0` khi khớp thật sự ra ngoài khung. Bản export hiện đang ghi `v=2` cho điểm này, nên nếu chỉnh tiếp thì cần đổi về `v=1` để bám sát guideline.
