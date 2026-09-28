# Guideline patch

- **Rule mới đề xuất:**
  1. **R06a — Privacy blur không làm vật thành `unreadable`.** Ô mờ che biển số/khuôn mặt là xử lý quyền riêng tư của
     dataset; nếu vẫn nhận ra loại xe/người qua hình dạng thì phải box theo class, không phủ `ignore_region`.
     `crowd_or_group` chỉ dùng khi **không thể** tách từng vật bằng mắt ở độ phóng 2×. `ignore_region` luôn vẽ bằng
     **polygon**, không dùng rectangle.
     *Ví dụ:* `adasind_128310.jpg` xe trắng (425,927)–(476,992) cao ~65 px → `Car`, không phải `unreadable`;
     `adasind_199770.jpg` hai xe máy đỗ (≈90–117, ≈110–145) → hai `Bike` occluded, không phải `crowd_or_group`;
     `adasind_019560.jpg` (C0) hai xe ba bánh đỗ → hai `ThreeWheeler`.
  2. **R04a — Xe đẩy/xích lô bán hàng.** Xe đẩy hàng có bánh do người đẩy/đạp (có hoặc không dù che) → `ThreeWheeler`
     nếu có 3 bánh, `Bike` nếu 2 bánh; khi dính sát một xe khác, box riêng nếu thấy được ranh giới hai thân xe.
     *Ví dụ:* `adasind_199770.jpg` L5/L6: xe đẩy có dù (x≈377–457) sát một auto-rickshaw (x≈457–530).
- **Áp dụng cho:** `ignore_region.reason` (`unreadable`, `crowd_or_group`), class `ThreeWheeler`/`Bike`, mọi zone.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** R06 chỉ liệt kê reason, không nói ô mờ privacy có tính là
  "mờ" hay không, và không nêu ngưỡng tách được/không tách được của `crowd_or_group`; mình đã hiểu sai ở C0 và B3
  (5 box MISSING cùng gốc). R04 không nhắc xe đẩy hàng, nên L và reference xử lý khác nhau ở 199770 L6 (E2).
- **`rules_version` mới:** v1.0.0 → v1.1.0
- **Hiệu lực từ:** round `rework` của lab tiếp theo (không áp ngược lên bản đã khóa); reference B3-edge cần soát lại
  theo R04a trước khi dùng làm mẫu.
