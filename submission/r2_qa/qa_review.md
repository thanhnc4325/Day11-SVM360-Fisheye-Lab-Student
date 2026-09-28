# QA review · B3-edge

Mã khóa: 56AF-5C43

Cold review (không nhận được bản khóa của bạn kế bên trong thời gian P3): soát lại **bản đã khóa của chính mình**
sau khi nghỉ, bằng `qa_overlay.html` + ảnh gốc + rules v1.0.0. Chưa mở reference, model overlay hay worked HTML.
Chỉ ghi điều quan sát và luật liên quan; nguyên nhân (WHY) để P4.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_123090.jpg | L2 | R03 | Box `Car` (180,964)–(251,1032) nằm chồng lên nửa trên của người lái xe máy, cùng vị trí với L4 `Bike` (183,952)–(248,1092). Trên ảnh chỉ có một xe máy có người lái → theo R03 chỉ một box `Bike`; L2 là box thừa/trùng với class không khớp vật nào. |
| adasind_128310.jpg | L1 | R02 | Box `Truck` (724,905)–(1075,1071) ôm chung hai xe: xe tải sơn màu (x≈724–870) và xe tải vàng phía sau (x≈860–1075). Box không bám một vật; nhìn ảnh gốc thấy hai thân xe tách biệt, nên cần hai box. |
| adasind_128310.jpg | L5 | R06 | `ignore_region` (reason `unreadable`) vẽ bằng **rectangle** (425,927)–(476,992) trên xe trắng. R06 yêu cầu polygon; ngoài ra xe cao ≈65 px và vẫn nhận ra là xe, không phải "mờ/che gần hết". |
| adasind_128310.jpg | — | R01 | Mép trái-trên (x≈0–80, y≈750–970) có thùng xe tải nâu bị vòng kính cắt, cao >40 px, chưa có box. Cần người soát xác nhận đó là xe (ảnh mờ ở rìa). |
| adasind_199770.jpg | L3 | R03 | Box `Bike` (959,1280)–(1080,1553) chỉ chứa chân và dép của một người đứng sát mép phải; không thấy bánh xe/khung xe. Class nhìn thấy trên ảnh là người (`Pedestrian`, truncated). |
| adasind_199770.jpg | L1 | R04 | Box `Truck` (225,815)–(275,861) ở xa: hình dạng giống xe con/van xám hơn xe tải (không thấy thùng hàng). Chưa chắc, cần người soát xem lại ở độ phóng lớn. |
| adasind_199770.jpg | (box 204,827–216,858) | R01 | Box `Pedestrian` cao 31 px < H=40: ngoài phạm vi phải box. Selfqc cũng cảnh báo; box không được tính (ngoài scope) nhưng là lỗi trình bày. |

Các mục đạt khi soát: mỗi frame có đủ 2 polygon `lens_border` bám vòng kính và 1 polygon `ego_body` (tay/áo người lái
ở dưới-trái); `truncated=true` cho các vật bị cắt ở mép (123090 L3, 199770 L9, L10); `occluded` dùng cho vật bị vật
khác che (199770 L6, L8); người đứng cạnh xe tay ga đỏ (199770 L7) tách khỏi `Bike` L8 đúng R03.

## Phản hồi sau P4 (vai Diagnostician, sau khi mở reference)

| QA | Kết luận | Căn cứ |
|---|---|---|
| 123090 L2 (R03) | Đồng ý → rework xóa | R chỉ có một Bike R3 cho rider; compare L2 SPURIOUS. |
| 128310 L1 (R02) | **Không đồng ý**, giữ nhãn | R5 cũng là một Truck (704–1080): cabin sơn màu và thùng vàng là cùng một xe tải dài. QA đã đọc nhầm hai phần thân xe thành hai xe. |
| 128310 L5 (R06) | Đồng ý → rework thành box Car | R4 Car (420,925)–(476,991); compare báo WRONG_CLASS. |
| 128310 thùng xe tải mép trái | Không đổi | R và M đều không box; vật quá mờ ở rìa, chưa đủ bằng chứng là xe — giữ ở mức nghi vấn. |
| 199770 L3 (R03) | Đồng ý → rework thành Pedestrian | M2 cũng gọi Pedestrian; box nằm trong ego_body đặt sai của R nên compare báo IGNORE_SCOPE (xem escalation). |
| 199770 L1 (R04) | **Không đồng ý**, giữ Truck | R7 là Truck; chỉ model gọi Car. |
| 199770 box 31 px (R01) | Đồng ý là lỗi trình bày (P3) | Nằm ngoài scope nên không ảnh hưởng số; xóa ở rework. |
