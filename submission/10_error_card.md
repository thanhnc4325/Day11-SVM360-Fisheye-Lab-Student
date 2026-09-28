# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B3 | IGNORE_SCOPE | 1 |
| center | B3 | MISSING | 2 |
| center | B3 | SPURIOUS | 9 |
| center | B3 | WRONG_CLASS | 1 |
| center | C0 | MISSING | 1 |
| center | C0 | SPURIOUS | 1 |
| edge | B3 | IGNORE_SCOPE | 1 |
| edge | B3 | MISSING | 5 |
| edge | B3 | SPURIOUS | 3 |
| edge | B3 | WRONG_CLASS | 1 |
| mid | B3 | BOX_GEOMETRY | 1 |
| mid | B3 | DUPLICATE | 1 |
| mid | B3 | IGNORE_SCOPE | 1 |
| mid | B3 | MISSING | 6 |
| mid | B3 | SPURIOUS | 7 |
| mid | C0 | MISSING | 1 |

## Top defects
- SPURIOUS: 20 (ví dụ frame adasind_019560.jpg)
- MISSING: 15 (ví dụ frame adasind_019560.jpg)
- IGNORE_SCOPE: 3 (ví dụ frame adasind_128310.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: SPURIOUS (20) và MISSING (15) phần lớn là dòng của **model** (M_only/LR_noM) — `E4_model_domain`, vì cùng một mẫu lặp lại ở nhiều box/frame: YOLO26m không có lớp ThreeWheeler (199770 M7, M8, M12 gọi Car/Truck), gọi người lái ego là Pedestrian (123090 M5, 128310 M5, 199770 M4) và tách rider thành người + xe (123090 M3, M6 — giống C0). Phần của **người gán nhãn** (`E1`) có chung một gốc: dùng `ignore_region` để né vật vẫn đọc được — C0 R5/R6 (`unreadable`), 128310 L5 (`unreadable`, lại vẽ bằng rectangle), 199770 R5/R6 (`crowd_or_group`) — tức hiểu sai R06, không phải lỗi hình học (mean IoU TP 0.810). IGNORE_SCOPE ở 199770 (L3, L9) là do **reference** đặt `ego_body` sai chỗ (`E0`).
- Cách sửa và ai nhận việc (`owner`): annotator — rework các ca E1 P0/P1 (đã làm: `rework/delta.md`, missing 5→1); guideline — thêm ví dụ R06 "ô mờ privacy/biển số không làm vật thành unreadable" và luật xe đẩy có dù (`20_guideline_patch.md`); data_ops — sửa `ego_body` reference 199770 (`30_escalation_ticket.md`); ai_team — không dùng model này làm prelabel cho ThreeWheeler/rider trước khi fine-tune với lớp ThreeWheeler và mask ego_body.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): `screenshots/l-128310-ignore-rect-vs-car.png` (R06, findings r3_diag 128310 L5, R4+M6); `screenshots/ref-199770-ego-body.png` (R07, findings r3_diag 199770 R4, M4); `r3_diag/local_quality_conflicts.csv` dòng 128310 mismatching_label ignore_region↔Car IoU 0.874.
