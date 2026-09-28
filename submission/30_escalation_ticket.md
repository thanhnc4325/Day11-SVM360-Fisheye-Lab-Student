# Escalation ticket

## Ticket 1

- **Frame:** `adasind_199770.jpg` (slice B3-edge, teaching reference `refs/slice-B3-edge.zip`)
- **Ảnh chụp:** `submission/screenshots/ref-199770-ego-body.png`
- **Expected impact:** Reference có polygon `ignore_region reason=ego_body` ở **mép phải** (x 924–1080, y 813–1552), trùm
  lên xe ba bánh và chân một người đứng cạnh, trong khi thân/tay người lái ego (áo caro) ở **mép trái** không có
  `ego_body`. Hậu quả: (1) box L9 ThreeWheeler và L3 của học viên bị tính IGNORE_SCOPE, không bao giờ khớp được;
  (2) reference tự có box R4 ThreeWheeler nằm trong vùng ignore của nó nên luôn thành MISSING (còn lại sau rework);
  (3) model box người lái ego (M4) thành M_only thay vì bị loại như ở 123090/128310. Mọi thống kê mid/edge của frame
  này và của ai dùng slice B3-edge đều lệch; học viên có thể bị dạy sai rằng ego_body nằm bên phải.
- **Owner:** `data_ops`
- **Recommendation:** Sửa reference 199770: xóa polygon ego_body bên phải; vẽ ego_body theo thân/tay người lái ở mép
  trái (tương tự 123090: x 0–211, 128310: x 0–230); giữ R4 ThreeWheeler (935,990)–(1080,1300) và thêm box Pedestrian
  cho người đứng mép phải nếu ≥40 px. Sau đó chạy lại `compare`/`local-quality` cho B3-edge và ghi phiên bản reference mới.
  Kèm kiểm tra tự động: cảnh báo khi polygon ego_body không chạm mép ảnh gần tâm vòng kính phía dưới hoặc chứa box vật.

## Ticket 2

- **Frame:** `adasind_199770.jpg`
- **Ảnh chụp:** `submission/screenshots/ref-199770-ego-body.png` (người đứng dưới mái hiên, x≈752–776)
- **Expected impact:** Model thấy một người cao ~73 px (M13 (752,850)–(776,923)); cả L và reference đều không box. Nếu có
  thật, reference thiếu một Pedestrian và mọi so sánh đang phạt model sai.
- **Owner:** `qa`
- **Recommendation:** Người soát thứ hai xem ảnh gốc ở 3× (và frame liền kề nếu có) để xác nhận; nếu có người ≥40 px
  thì thêm vào reference và ghi E0, nếu không thì giữ E5 với lý do.
