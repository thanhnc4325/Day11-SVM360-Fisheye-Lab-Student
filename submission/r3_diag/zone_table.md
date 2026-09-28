# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 6 | 1 | 3 | 1 | 4 | SPURIOUS (2) |
| mid | 6 | 2 | 2 | 3 | 4 | SPURIOUS (2) |
| edge | 5 | 2 | 0 | 3 | 3 | MISSING (2) |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: L gãy nhiều nhất ở **center** (3 spurious / n_ref 6: 123090 L2 Car trùng rider, 199770 L4 và L6) và **edge** mất 2/5 (199770 R5, R6 — hai xe máy đỗ bị L phủ bằng `crowd_or_group`). M gãy đều hơn nhưng nặng ở **mid** (3 missing + 4 thừa / n_ref 6) và **edge** (3 missing + 3 thừa / 5): model nhầm ThreeWheeler thành Car/Truck (M8, M12, M7), gọi người lái ego là Pedestrian (M4) và tách rider thành người + xe (123090 M3, M6).
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: lỗi L chủ yếu là **quyết định phạm vi** chứ không phải hình học — mean IoU TP 0.810, và ở IoU 0.3→0.5 số matched của L không đổi; L chỉ tụt ở center khi lên 0.7 (5→3), tức box center hơi lỏng. Hai lỗi lặp lại từ C0: dùng `ignore_region` (unreadable / crowd_or_group) cho vật vẫn đọc được, và tách rider. Lỗi M là lệch miền: không có lớp ThreeWheeler, không biết thân xe ego, quy ước rider của ảnh phẳng. Méo fisheye không phải nguyên nhân chính trong slice này: các box mép (123090 L3, 199770 L10) khớp R với IoU 0.95 và 0.71. **Giới hạn:** chỉ 3 frame, 17 box reference, một camera; 199770 chiếm phần lớn lỗi và có ego_body reference đặt sai (E0), làm sai lệch cột edge/mid của frame này. Zone là vị trí bán kính trên ảnh, không cho biết vật gần hay xa xe; không dùng bảng này để kết luận tỉ lệ lỗi.
