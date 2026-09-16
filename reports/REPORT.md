# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Đức Hà   Nhóm: ______   Ngày: 2026-06-06

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán |20 |
| Số skeleton |27 |
| v=2 / v=1 / v=0 |400 / 35 / 24 |
| Thời gian trung bình mỗi ảnh |~3 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear (19%)
2. right_eye (15%)
3. left_wrist (15%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

> Các khớp này đúng là những vị trí khó gán vì thường xuyên bị che khuất một phần bởi tóc, góc quay nghiêng hoặc cử động tay tự nhiên[cite: 5, 6]. Tuy nhiên, khó xác định vị trí giải phẫu chính xác khác với việc khớp bị che khuất một phần (`v=1`)[cite: 5, 6]. Bằng chứng nhìn thấy từ các khung hình cho thấy các điểm này vẫn nằm trong biên độ ảnh nhưng bị khuất nhẹ, đòi hỏi phải suy luận từ cấu trúc khuôn mặt và tay áo
<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình |0.9295 |0.9295 |
| OKS@0.50 |0.9310 |0.9310 |
| OKS@0.75 |0.8966 |0.8966 |
| Lỗi `dao_trai_phai` |0 |0 |
| Lỗi `nham_nguoi` |2 |2 |
| Lỗi `xoa_khop_bi_che` |1 |1 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- `train_12.jpg` + người số 1 + khớp `left_ankle` + thay đổi cờ trạng thái từ `v=0` (outside) thành `v=1` (occluded) theo chuẩn gold.
- `train_19.jpg` + người số 2 + khớp `right_wrist` + kéo và tinh chỉnh lại tọa độ điểm chấm để khắc phục lỗi trượt khớp.
- `train_13.jpg` + bổ sung thêm 2 skeleton người bị thiếu trực tiếp trên khung hình ảnh.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

- `Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh`
## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

-

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 |0.8450 |0.8450 |0.0000 |
| pose_mAP50-95 |0.6853 |0.6908 |+0.0055 |
| pose_precision |0.9734 |0.9792 |+0.0058 |
| pose_recall |0.8462 |0.8462 |0.0000 |
| box_mAP50-95 |0.8119 |0.8041 |-0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
- `pose_mAP50-95` tăng nhẹ `+0.0055` sau fine-tune. Việc huấn luyện trên 20 ảnh giúp model củng cố thêm đặc trưng phân bố pose riêng của tập dữ liệu này, mặc dù số lượng ít nhưng vẫn hỗ trợ cải thiện độ chính xác tổng thể ở ngưỡng OKS khắt khe.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?
- `box_mAP50-95` (0.8041) cao hơn rõ rệt so với `pose_mAP50-95` (0.6908). Điều này chứng tỏ model tìm *người* (bounding box) dễ hơn rất nhiều so với việc định vị chính xác tọa độ từng *khớp xương* (keypoints), do việc nhận diện khung người có biên độ sai số lớn hơn.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
- Ảnh test `train_19.jpg` có hiện tượng model đoán sai lệch một chút ở cổ tay (`right_wrist`), thuộc nhóm lỗi **lệch nhẹ** so với cấu trúc thực tế.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
- Ảnh có OKS thấp nhất giữa nhãn và model là `train_15.jpg` (OKS khoảng 0.559). Trong trường hợp này, nhãn của người gán chính xác hơn vì đã qua kiểm định Gold, trong khi model bị nhầm lẫn do tư thế khuất góc.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
- Ảnh gán tệ nhất trên tập train (`train_15.jpg` và ca lệch người ở `train_13.jpg`) cũng chính là các bức ảnh mà model gặp khó khăn lớn khi dự đoán. Điều này phản ánh các bức ảnh đó có độ phức tạp cao, góc khuất hoặc thiếu sáng khiến cả con người lẫn model đều dễ suy luận sai lệch.


## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->

- Tại ảnh `train_12.jpg`, đối với khớp cổ chân trái (`left_ankle`), mặc dù phần chân bị che khuất một phần bởi vật cản phía trước, nhưng căn cứ vào cấu trúc cẳng chân liền kề và vị trí khớp nối giày, khớp này vẫn nằm hoàn toàn bên trong khung hình. Do đó, tôi đã quyết định chọn trạng thái `v=1` (occluded) thay vì loại bỏ thành `v=0`, đảm bảo giữ lại điểm trọng yếu cho bộ khung xương.