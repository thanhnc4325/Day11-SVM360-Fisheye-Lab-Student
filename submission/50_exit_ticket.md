# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?
   **Không phải DUPLICATE.** DUPLICATE là hai box cho cùng một vật trên **cùng một ảnh** (như 123090 L2 Car chồng L4
   Bike). Ở seam, mỗi camera có ảnh riêng và trên mỗi ảnh vật đó cần đúng một box hợp lệ. Cần một quy tắc riêng về
   output: nếu đầu ra là per-camera thì giữ cả hai box; nếu đầu ra là fused/BEV thì chỉ ghép thành một vật khi có
   timestamp đồng bộ, calibration để chiếu hai box về cùng hệ toạ độ và policy chọn box/ghép.
2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.
   Giữ cùng track ID khi vẫn là cùng một vật quan sát liên tục (kể cả bị che ngắn rồi xuất hiện lại, theo guideline
   task). Thêm keyframe khi hình học đổi lớn — vật đi từ tâm ra rìa vòng kính, bị cong/cắt (truncated), đổi kích
   thước nhanh — để nội suy không lệch. Đặt Outside khi vật rời trường nhìn hoặc bị che hoàn toàn, không kéo box theo
   vật không thấy. Nối track qua hai camera cần: timestamp đồng bộ, calibration/extrinsics để chiếu vị trí về cùng hệ,
   vị trí/vận tốc liên tục qua seam, và policy output cho phép cross-camera ID; thiếu thì giữ hai track riêng.
3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?
   Ở `adasind_128310.jpg` L1 mình vẽ một box Truck (724–1075) cho cả cabin sơn màu và thùng vàng; khi cold review
   (r2_qa) mình lại nghi phải tách hai xe. Đối chiếu reference R5 (một Truck) và nhìn kỹ ảnh thì đó là một xe tải dài,
   nên giữ nhãn và ghi lý do (decision log D05) thay vì sửa theo cảm giác. Ngược lại, ở C0 và 199770 mình tin phủ
   `ignore_region` lên cụm xe đỗ là đúng, nhưng R06 và reference cho thấy xe vẫn đọc được phải box. Nếu làm lại, mình
   sẽ vẽ vật bằng rectangle trước, chỉ dùng ignore khi thật sự không tách được ở ảnh phóng 2×, và soát riêng vùng
   tối/mép ảnh (199770 R3 bị sót).
