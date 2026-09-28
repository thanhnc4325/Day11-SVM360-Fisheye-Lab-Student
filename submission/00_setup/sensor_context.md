# Sensor context

- Rig: theo quan sát ảnh (ADASIND không kèm tài liệu rig), đây là **một** camera fisheye gắn trên xe hai bánh,
  ảnh dọc 1080×1920, nhìn về phía trước theo chiều chạy; ở đáy ảnh thấy bóng người lái + xe đổ trên mặt đường
  (vd `adasind_019560.jpg`). Không có calibration, timestamp hay camera khác đi kèm, nên dữ liệu này **không**
  đại diện cho bốn camera SVM front/rear/left/right.
- `ego_body`: thân/tay lái xe ego lộ ở mép dưới-trái vòng kính (vd `adasind_019560.jpg`, khoảng x<150, y 1080–1550);
  thấy ở 46/48 frame, riêng `adasind_006840.jpg` và `adasind_271039.jpg` không thấy thân xe nên không vẽ.
- Vòng kính: hình tròn/elip gần như lấp đầy chiều ngang ảnh, tâm khoảng giữa ảnh (y ≈ 900–1000), mép trên ≈ y 115,
  mép dưới ≈ y 1720; bốn góc và dải trên/dưới là vành đen (`lens_border`), chiếm khoảng 20–25% khung hình.
  Méo mạnh nhất ở rìa vòng kính: vật gần rìa bị cong và bị cắt (→ `truncated`).
