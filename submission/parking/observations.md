# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): hai vạch chính ở **dãy ô tiền cảnh** — vạch chéo giữa-dưới
  (407,652)→(530,720) và vạch chéo phải-dưới (698,623)→(960,684); mỗi vạch là ranh giữa hai ô đỗ kề nhau và dừng ở
  mép ảnh chứ không kéo tiếp qua phần không thấy. Ngoài ra đã vẽ thêm các vạch chia ô ngắn của dãy giữa
  (vd (197,537)→(248,564), (329,532)→(420,554)) và dãy xa (y ≈ 486–510); tổng 25 polyline, mỗi đoạn sơn chia ô
  là một polyline riêng.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: **vạch trắng dài chạy ngang gần hết ảnh** ở dãy giữa
  (từ khoảng (0,543) lên (900,508)). Vạch này nối đầu các ô, là ranh giữa dãy ô và lối xe chạy phía trước, không tách
  hai ô riêng, nên không gán `parking_line`. Cũng không vẽ mép bãi/hàng rào ở xa (y ≈ 460) vì là biên bãi, không
  phải sơn chia ô.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: polygon bao lối xe chạy giữa đầu dãy ô giữa và đầu dãy
  ô tiền cảnh — cạnh trên từ (0,576) đến (960,526), ngay dưới đầu các vạch chia ô dãy giữa; cạnh dưới từ (0,681) đến
  (960,592), ngay trên đầu vạch tiền cảnh. Trong vùng này không có xe hay vật cản; xe đỏ duy nhất ở (193–220, 457–478)
  nằm ngoài polygon. Đây là vùng trống nhìn thấy trên ảnh tĩnh, không phải kết luận vùng lái an toàn.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): ở phía phải dãy giữa (x ≈ 550–835, y ≈ 508–530),
  vạch chia ô và vạch đầu dãy gần như chập nhau ở góc nhìn xa, độ phân giải thấp; khó tách chính xác đoạn nào là vạch
  chia ô (các polyline 5, 11, 16 trong export) và đoạn nào là vạch đầu dãy. Các vạch ở dãy xa (y < 510) cũng rất
  ngắn, nên cần người soát xem lại.
