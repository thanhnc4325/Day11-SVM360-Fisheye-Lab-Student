# Rework delta

| zone | matched before | matched after | missing before | missing after | spurious before | spurious after |
|---|---:|---:|---:|---:|---:|---:|
| center | 5 | 6 | 1 | 0 | 3 | 2 |
| mid | 4 | 5 | 2 | 1 | 2 | 1 |
| edge | 3 | 5 | 2 | 0 | 0 | 0 |

## Findings action=rework
- adasind_123090.jpg L2 SPURIOUS: đã sửa
- adasind_128310.jpg L5+R4 WRONG_CLASS: đã sửa
- adasind_199770.jpg R3 MISSING: đã sửa
- adasind_199770.jpg R5 MISSING: đã sửa
- adasind_199770.jpg R6 MISSING: đã sửa
- adasind_123090.jpg L2 SPURIOUS: đã sửa
- adasind_128310.jpg L5 SPURIOUS: đã sửa
- adasind_128310.jpg R4+M6 MISSING: đã sửa
- adasind_199770.jpg R3+M10 MISSING: đã sửa
- adasind_199770.jpg R5 MISSING: đã sửa
- adasind_199770.jpg R6 MISSING: đã sửa

## Đọc delta

Tổng matched 12 → 16, missing 5 → 1, spurious 5 → 3 (IoU 0.5, cùng teaching reference). Chỉ sửa ca `action=rework`
mức P0/P1 (và xóa box 31 px < H, lỗi trình bày P3); không đụng ca `keep_with_reason`/`escalate`:

- 123090: xóa Car (180,964) trùng rider → center spurious −1.
- 128310: ignore_region rectangle `unreadable` → box Car (425,927)–(476,992) → hết WRONG_CLASS, +1 matched.
- 199770: bỏ polygon `crowd_or_group`, vẽ 2 Bike cho hai xe máy đỗ (≈90–117 và ≈110–145) → edge missing 2 → 0;
  thêm Pedestrian dưới mái hiên (853,843)–(903,947) → mid +1; đổi L3 Bike → Pedestrian (chân người), xóa box 31 px.

Còn lại có chủ đích: **mid missing 1** = R4 ThreeWheeler nằm trong `ego_body` đặt sai của reference (E0, đã
escalate — box L9 của mình bị tính IGNORE_SCOPE nên không thể khớp); **spurious 3** = 199770 L2 (E5, model cũng
thấy người này), L4 (E5, cần frame liền kề), L6 (E2, xe đẩy có dù chưa có luật). Số cải thiện đo độ khớp với
teaching reference trên 3 frame, không chứng minh nhãn mới là gold. Rework được tạo từ bản khóa r1_craft bằng script
sửa đúng các ca trên (exports/rework.xml); nạp file này vào task CVAT để task khớp bản khóa (D03 decision log).
