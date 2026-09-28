# Tự soát

- adasind_199770.jpg L1: chiều cao < H (xem lại phạm vi)
- Tên task thiếu raw_fisheye

## Cảnh báo tự động đã xử lý
- 199770 L1 (Pedestrian 204,827–216,858, cao 31 px < H=40): giữ trong bản khoá, chưa xoá; ghi thành finding để đối chiếu ở P4 (R01).
- "Tên task thiếu raw_fisheye": task CVAT tên đúng `Day11 · ADASIND · B3-edge · raw_fisheye`, nhưng export ở cấp job nên XML không mang tên task; export CVAT for images 1.1 đúng định dạng.
- Cảnh báo truncated ở L5/L7 (hai ThreeWheeler sát mép phải 913–1080 và mép trái 0–83) đã sửa thành truncated=true trước khi khóa.

## Checklist thủ công
- [x] Phạm vi H=40 và vật cần vẽ
- [x] lens_border và ego_body
- [x] Class sáu nhãn
- [x] Rider và Bike
- [x] Geometry trên ảnh fisheye gốc
- [x] truncated và occluded
- [x] Vật thiếu hoặc box trùng
- [x] ignore_region có reason
- [x] Tên task raw_fisheye và export CVAT 1.1

## Fill ratio (K12)
- adasind_123090.jpg box 1 center: 0.846
- adasind_123090.jpg box 3 edge: 0.855
- adasind_128310.jpg box 2 edge: 0.509
- adasind_199770.jpg box 11 edge: 0.786
mean edge: 0.716 (n=3)
mean center: 0.846 (n=1)
