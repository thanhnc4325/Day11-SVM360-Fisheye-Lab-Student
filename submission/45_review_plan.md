# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Frame có vật đỗ/che nhau bị phủ `ignore_region` (B3-edge 128310, 199770; C0 019560) | 5 MISSING + 1 WRONG_CLASS cùng gốc R06 (C0 R5/R6, 128310 L5↔R4, 199770 R5/R6); P0–P1 | Cùng một hiểu sai luật lặp qua hai round → lỗi hệ thống của annotator, không phải ngẫu nhiên; ignore còn làm vật biến mất khỏi mọi phép đo | Ảnh phóng vùng ignore, polygon/rectangle trong XML khóa, dòng findings, `screenshots/l-128310-ignore-rect-vs-car.png` |
| Frame mà reference có ignore/ego_body bất thường (B3-edge 199770) | 2 IGNORE_SCOPE + 1 R_only + 1 M_only do `ego_body` reference đặt sai (E0); thêm 1 người có thể bị cả L và R sót (M13) | Lỗi reference làm sai số của mọi người dùng slice; phải sửa trước khi dùng reference để chấm | `screenshots/ref-199770-ego-body.png`, XML reference, Ticket 1–2 |

Giới hạn của kết luận từ ba frame ADASIND: chỉ 3 frame (17 box reference) từ một camera fisheye gắn xe hai bánh, cùng
một chuyến quay ban ngày; riêng 199770 chiếm phần lớn lỗi. Không đủ để ước lượng tỉ lệ lỗi theo zone hay class, và
không nói gì về camera rear/left/right của hệ SVM. Kết luận chỉ là **giả thuyết cần kiểm** trên mẫu lớn hơn.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv`: lấy mẫu **phân tầng** theo camera × normal/hard, rồi trong
mỗi ô chọn theo chuyến quay/cảnh (clip), tối đa 1–2 frame mỗi clip và cách nhau vài giây, để 10 frame liên tiếp của
cùng một cảnh không bị đếm như 10 ca độc lập. Ô hard được chọn theo thẻ điều kiện (đêm/ngược sáng, mưa/bẩn ống kính,
vật ở rìa vòng kính, vật ở seam, vật sát xe, ego_body/gương lớn, cụm vật che nhau như lỗi R06 ở trên), kèm bảng đếm
độ phủ mỗi thẻ × camera; ô nào thẻ bằng 0 thì bổ sung trước khi review. Kế hoạch này chỉ giúp **tìm ca cần soi**:
mẫu hard được chọn có chủ đích (không ngẫu nhiên, trọng số lệch) nên không đo được tỉ lệ lỗi của toàn bộ 50.000
frame; muốn đo tỉ lệ cần một mẫu ngẫu nhiên riêng.
