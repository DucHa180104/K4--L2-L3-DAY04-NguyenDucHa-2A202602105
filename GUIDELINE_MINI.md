# Mini guideline - nhóm: Cá nhân  |  người gán: Nguyễn Đức Hà  |  ngày: 2026-09-16

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Ước lượng tâm đối xứng của khung chậu theo cấu trúc vai và sống lưng để đặt điểm, gán `v = 1` nếu bị áo trùm che khuất. | Quần áo rộng hoặc dài che khuất cấu trúc xương trực tiếp, cần dựa vào hình dáng cơ thể tổng thể để định vị đồng nhất. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Đặt chấm tại vị trí dự đoán tâm vành tai và gán cờ `v = 1` (occluded). | Tai không lộ rõ biên giới nhưng vẫn nằm trong khung hình và xác định được vùng tương đối trên đầu. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp nằm trong khung hình gán bình thường; các khớp nằm ngoài biên độ ảnh (chân, đầu gối) bắt buộc cho ra ngoài mép và gắn cờ `v = 0`. | Tuân thủ nguyên tắc không chấm điểm bừa bãi cho phần cơ thể nằm ngoài vùng quan sát thực tế của camera. |
| Cổ tay nằm sau tay lái / sau thân mình | Ước lượng vị trí theo góc gập của khuỷu tay và hướng di chuyển của bàn tay, đặt chấm và gán `v = 1`. | Đảm bảo bộ 17 điểm không bị thiếu hụt, giúp model học được quỹ đạo chi dù bị che khuất. |
| Hai người chồng lên nhau | Phân tách rõ ràng từng ID người theo bounding box từ đầu đến chân, gán điểm độc lập cho từng người kể cả khi bị che khuất một phần bởi người đứng trước. | Tránh tình trạng gộp nhầm nhãn hoặc thiếu người trong các khung hình đông đúc. |
| Người nhỏ đến mức nào thì không gán nữa | Chỉ bỏ qua khi diện tích người quá nhỏ (chiều cao bounding box nhỏ hơn 30 pixels hoặc bị mờ nhòe hoàn toàn không nhìn rõ hình thù). | Giảm nhiễu cho tập dữ liệu học tập vì các đối tượng quá nhỏ không đủ độ chi tiết để trích xuất 17 khớp xương. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_12.jpg`, người thứ `1`, khớp `left_ankle`

- Mơ hồ ở chỗ nào: Phần cổ chân bị khuất một phần bởi vật cản phía trước và sát mép khung hình, phân vân giữa việc bỏ qua hay đánh dấu che khuất.
- Bạn quyết thế nào: Giữ lại điểm và đặt chấm ở vị trí ước lượng. Bản export sau rework hiện lưu `v = 2`; theo luật của lớp, điểm này nên là `v = 1` vì cổ chân bị che nhưng vẫn nằm trong khung.
- Vì sao: Khớp vẫn nằm trong biên độ khung hình, việc xóa hoặc cho ra ngoài (`v = 0`) sẽ làm mất điểm trọng yếu của bộ khung xương chân.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ học được thói quen bỏ sót hoặc gán sai cờ biên, dẫn đến đứt gãy thông tin cấu trúc chi dưới khi gặp vật cản tương tự.

### Ca 2 - ảnh `train_15.jpg`, người thứ `2`, khớp `right_wrist`

- Mơ hồ ở chỗ nào: Cổ tay bị khuất hoàn toàn phía sau thân người do góc chụp nghiêng, không thấy rõ điểm bám thực tế.
- Bạn quyết thế nào: Dựa vào hướng đi của cẳng tay và khớp khuỷu tay (`right_elbow`) để suy luận điểm đặt hợp lý, gán cờ `v = 1`.
- Vì sao: Đảm bảo nguyên tắc đủ 17 điểm cho mọi skeleton, không tự ý cắt bỏ điểm cốt lõi của tay.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ dự đoán sai lệch vị trí các điểm khớp bị che khuất sâu, gây sụt giảm độ chính xác OKS ở các tư thế phức tạp.

### Ca 3 - ảnh `train_19.jpg`, người thứ `2`, khớp `right_wrist`

- Mơ hồ ở chỗ nào: Điểm chấm bị lệch nhịp so với tiêu chuẩn giải phẫu chung do cổ tay chuyển động nhanh gây mờ nhòe (motion blur).
- Bạn quyết thế nào: Tôi đã kéo chỉnh lại tọa độ dựa theo đường viền bàn tay, nhưng evaluator sau rework vẫn xếp điểm này vào lỗi `truot_han`; đây là lỗi còn lại chưa xử lý dứt điểm.
- Vì sao: Tránh lỗi trượt khớp nặng làm ảnh hưởng trực tiếp đến hệ số dung sai OKS khi đánh giá.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ bị nhiễu nhãn tọa độ, dẫn đến việc dự đoán nhầm vị trí khớp sang các vùng lân cận không chính xác.

## 4. Sau khi so visibility report với bạn cùng nhóm

Tôi làm bài cá nhân và chưa nhận được thư mục nhãn của một bạn khác, nên chưa chạy được
chế độ `--compare`. Vì vậy, chưa có số liệu chênh `%v=1` hoặc luật mới được thống nhất từ
hoạt động kiểm chéo. Tôi không tự tạo tên người review hoặc số liệu so sánh.

