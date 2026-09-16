# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: ______   Nhóm: ______   Ngày: 2026-09-16

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 331 / 132 / 30 |
| Thời gian trung bình mỗi ảnh | Chưa ghi nhận |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` - 66% v=1
2. `right_ear` - 48% v=1
3. `left_hip` - 41% v=1

Đây là các khớp có tỷ lệ bị che cao nhất theo báo cáo, không tự động chứng minh
chúng là các khớp khó xác định nhất. Tai thường bị tóc/góc mặt che; hông thường
phải ước lượng theo giải phẫu qua quần áo, nên cần đối chiếu với ảnh và guideline.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9241 | 0.9241 |
| OKS@0.50 | 0.9655 | 0.9655 |
| OKS@0.75 | 0.9655 | 0.9655 |
| Lỗi `dao_trai_phai` | 1 | 1 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- `train_04`: đổi `left_knee`, `right_knee` và `right_hip` ngoài khung sang `v=0`.
- `train_13`: đổi `left_knee`, `right_knee`, `left_ankle` và `right_ankle` ngoài khung sang `v=0`.
- Đây là sửa cờ visibility để qua validator, không sửa vị trí hình học; OKS giữ nguyên 0.9241.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** `train_06.jpg`, người #1;
Gold chấm skeleton này OKS 0.199 và gọi tên lỗi đảo trái/phải kèm trượt
`left_elbow`, `left_wrist`. Chưa có ghi chú đủ để kết luận ảnh dễ hay khó;
cần đối chiếu ảnh visualize trước khi mô tả nguyên nhân thao tác.

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm: chưa có dữ liệu kiểm chéo.

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| Chưa có dữ liệu |  |  |  | Chưa chạy so sánh với bạn cùng nhóm |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- Chỉ chọn `v=0` khi khớp nằm ngoài biên ảnh; nếu còn trong ảnh nhưng bị che,
  vẫn giữ tọa độ ước lượng và chọn `v=1`.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` tăng `0.0055`, từ `0.6853` lên `0.6908`. Vì chỉ tăng nhẹ
   trên 10 ảnh test, chưa thể kết luận 20 ảnh tạo ra năng lực tổng quát mới;
   kết quả cho thấy fine-tune có cải thiện pose nhưng không lớn.

2. Sau fine-tune, `box_mAP50-95` là `0.8041` và `pose_mAP50-95` là `0.6908`,
   chênh `0.1133`. Model tìm đúng vùng người dễ hơn xác định chính xác từng khớp.

3. Chưa có ghi chú ảnh test cụ thể trong output notebook để gán chắc một trong
   bốn loại lỗi. Cần chọn một ảnh từ thư mục predictions và đối chiếu trực quan.

4. `train_06` có OKS thấp nhất giữa model và nhãn của bạn: `0.072`. Gold cũng
   chỉ ra `train_06`, người #1 là skeleton yếu nhất với OKS `0.199`; vì vậy
   có bằng chứng đây là ca cần xem lại, nhưng chưa đủ để kết luận riêng model hay nhãn đúng.

5. Có dấu hiệu trùng: `train_06` là skeleton tệ nhất khi so với gold và cũng là
   ảnh có OKS thấp nhất khi so model với nhãn. Điều này gợi ý ảnh có pose khó
   hoặc lỗi trái/phải ở người #1; kết luận cuối cần dựa trên ảnh visualize và gold.

## 5. Một rule evidence bạn đã dùng

Ví dụ từ dữ liệu đã sửa: ở `train_04`, các khớp `left_knee`, `right_knee` và
`right_hip` có tọa độ vượt đáy ảnh. Vì các điểm đã nằm ngoài biên nên chúng phải
được ghi `v=0`, không phải `v=1` hoặc `v=2`. Đây là quyết định dựa trên biên ảnh,
khác với khớp bị che nhưng vẫn còn trong khung, vốn phải giữ chấm ước lượng và `v=1`.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
