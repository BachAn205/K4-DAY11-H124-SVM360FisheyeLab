# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh):
  - Vạch 1: Đường chéo từ (58.8, 526.2) đến (49.6, 569.7) — góc dưới bên trái, chạy xiên xuống.
  - Vạch 2: Đường chéo từ (177.9, 524.8) đến (245.3, 564.4) — phía dưới giữa trái và phải, chạy xiên xuống.

- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: Còn rất nhiều vạch không vẽ, tôi chỉ vẽ 2 vạch để thỏa mãn yêu cầu đề bài chỉ vẽ 2 vạch.

- Polygon `free_space` dừng ở đâu; có phần bị che nào không:
  Polygon phủ toàn bộ phần dưới ảnh: từ đường viền trên ở y ≈ 489–506 (thấp nhất 489.1, cao nhất 506.1) xuống đáy (y = 720), trải rộng toàn bộ chiều ngang (x = 0–960). Hình dạng là tứ giác không đều (hình thang). **Không bị che** (occluded=0).

- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): không có
