# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Rider/xe ba bánh ở rìa vòng kính, cụm xe đỗ che nhau, ngược sáng (30 hard / 25 normal) | Class rider (R03) và ThreeWheeler (R04) dễ nhầm; B3-edge cho thấy annotator phủ ignore lên vật đọc được, model thiếu lớp ThreeWheeler | Box trên ảnh fisheye gốc (không undistort); intrinsics + tâm/bán kính vòng kính camera front; `rules_version` | Hai annotator gán nhãn mù độc lập; reviewer thứ ba phân xử mọi cặp IoU < 0.5 hoặc khác class; ghi lý do vào decision log |
| rear | Người/trẻ em sát cản sau khi lùi, vạch ô đỗ, ánh sáng yếu (30 / 20) | Vật thấp sát xe dễ dưới H=40 hoặc bị nuốt trong ego_body/cản sau; vạch ô đỗ dễ nhầm vạch lối xe chạy (bài parking) | Polygon ego_body/cản sau theo rig, calibration rear, timestamp để đối chiếu clip lùi | Như front, thêm lượt soát riêng ego_body từng frame (lỗi E0 ở 199770 cho thấy ego polygon có thể đặt sai) |
| left | Vật ở seam front-left/rear-left, xe máy vượt sát, gương lớn (27 / 20) | Một vật hiện trên hai camera; méo rìa lớn; gương chiếm ảnh | Extrinsics left + front/rear để chiếu seam; timestamp đồng bộ giữa camera | Reviewer seam xem cặp frame cùng timestamp của hai camera trước khi đánh DUPLICATE hay giữ hai box |
| right | Seam front-right/rear-right, người bước từ vỉa hè, xe đỗ sát khi vào ô (28 / 20) | Như left; người trong vùng tối dưới mái hiên bị sót (199770 R3, M13) | Như left, cho camera right | Như left, thêm một lượt chuyên tìm vật bị sót trong vùng tối/sát mép |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule): khi đổi mẫu camera/ống kính hoặc vị trí gắn (tâm
  vòng kính, ego_body đổi); khi calibration/extrinsics được đo lại; khi `rules_version` tăng (vd v1.1.0 với R06a/R04a
  ở `20_guideline_patch.md`); khi đổi miền dữ liệu (quốc gia, mùa, đêm); hoặc khi tỉ lệ bất đồng giữa reviewer trên mẫu
  kiểm định vượt ngưỡng nhóm đặt ra. Mỗi lần refresh ghi phiên bản gold + rules + calibration. Teaching reference
  ADASIND hiện tại không dùng làm gold: đã thấy lỗi `ego_body` đặt sai ở 199770 (E0).
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box: một xe máy vượt ở góc trước-trái hiện đồng
  thời ở camera front (vùng edge) và left (vùng mid) với hai box khác nhau. Trước khi coi là một vật (ghép hoặc gán
  cùng track ID) cần: timestamp đồng bộ của hai frame; calibration/extrinsics để chiếu hai box về cùng hệ toạ độ (vd
  BEV) và thấy chúng trùng; và policy output nói rõ đầu ra đích là per-camera (giữ cả hai box, không phải lỗi
  DUPLICATE) hay fused (một vật). Thiếu một trong ba thì giữ hai box hợp lệ trên từng camera.
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:
  ADASIND chỉ có một camera fisheye gắn xe hai bánh; điều kiện của rear/left/right (ego_body khác, seam, lùi đỗ) không
  xuất hiện. Hai người đồng ý với nhau vẫn có thể cùng sai (ở C0 mình hiểu sai R06), và teaching reference cũng có
  lỗi (E0 ở 199770). Local quality chỉ đo độ khớp với reference trên 3 frame; gold cần review độc lập, phân xử bất
  đồng và bao phủ từng camera.
