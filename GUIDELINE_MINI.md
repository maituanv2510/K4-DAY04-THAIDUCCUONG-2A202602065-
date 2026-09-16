# Mini guideline - nhóm: ______  |  người gán: ______  |  ngày: 2026-09-16

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Đặt tại vị trí hông ước lượng theo vai, thân và chân; nếu còn trong ảnh thì dùng `v=1` khi bị che. | Hông thường không có bề mặt nhìn thấy được nhưng vẫn là điểm giải phẫu cần giữ. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Giữ chấm tại vị trí tai ước lượng, còn trong ảnh thì `v=1`; chỉ dùng `v=0` khi tai nằm ngoài biên ảnh. | Tóc hoặc mũ là che khuất, không phải Outside. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp chân nằm ngoài biên ảnh là `v=0` và không đặt chấm; các khớp còn trong ảnh vẫn phải được gán. | `v=0` chỉ dành cho điểm ngoài khung, không dùng để đánh dấu điểm khó nhìn. |
| Cổ tay nằm sau tay lái / sau thân mình | Giữ vị trí cổ tay ước lượng phía sau vật che, còn trong ảnh thì `v=1`. | Đây là Occluded; xoá điểm sẽ làm mất thông tin huấn luyện. |
| Hai người chồng lên nhau | Hoàn thành từng người một; lấy vai, hông và chuỗi tay/chân liên tục của đúng cơ thể, không nối sang người bên cạnh. | Tránh nhầm người và skeleton kéo sang cơ thể khác. |
| Người nhỏ đến mức nào thì không gán nữa | Không bỏ người chỉ vì nhỏ; bộ dữ liệu yêu cầu đủ 17 điểm cho mọi người. Điểm không xác định được thì đặt theo ước lượng và gắn cờ phù hợp. | Không xoá skeleton hoặc keypoint khỏi file. |

Ảnh mẫu CVAT cần bổ sung thủ công cho từng dòng trước khi nộp: `[chưa bổ sung screenshot CVAT]`.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_04`, người thứ `1`, khớp `left_knee`/`right_knee`

- Mơ hồ ở chỗ nào: Hai đầu gối nằm sát hoặc vượt mép dưới ảnh nên dễ bị để lại tọa độ cũ.
- Bạn quyết thế nào: Đổi visibility thành `v=0` cho điểm đã ra ngoài biên và không đặt chấm ngoài ảnh.
- Vì sao: Validator xác nhận các tọa độ tương ứng vượt chiều cao ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model học một điểm có tọa độ ngoài ảnh nhưng lại được đánh dấu nhìn thấy/bị che.

### Ca 2 - ảnh `train_13`, người thứ `1` và `2`, khớp `left_knee`/`right_knee`/`left_ankle`/`right_ankle`

- Mơ hồ ở chỗ nào: Một số chân bị cắt ở cạnh dưới ảnh, nhưng export ban đầu vẫn có `v=1`.
- Bạn quyết thế nào: Đổi bốn điểm ngoài khung thành `v=0`; giữ nguyên các điểm còn trong ảnh.
- Vì sao: Quy tắc Outside phụ thuộc vào biên ảnh, không phụ thuộc việc điểm có khó nhìn hay không.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model có thể học chân nằm ở vị trí không tồn tại trong khung ảnh.

### Ca 3 - ảnh `train_06`, người thứ `1`, khớp `left_elbow`/`left_wrist`

- Mơ hồ ở chỗ nào: Gold đánh dấu đây là skeleton có OKS thấp nhất (`0.199`) và nghi đảo trái/phải.
- Bạn quyết thế nào: Kiểm lại trái/phải theo cơ thể người, sau đó đối chiếu vai, khuỷu và cổ tay trên ảnh visualize.
- Vì sao: Đánh giá Gold gọi tên lỗi đảo trái/phải và trượt hẳn ở hai khớp này.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Augmentation lật ảnh có thể khuếch đại nhãn trái/phải sai.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: Chưa xác định (chưa có thư mục nhãn của bạn cùng nhóm).
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Chưa kết luận.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Chưa có dữ liệu kiểm chéo để thống nhất thêm.
