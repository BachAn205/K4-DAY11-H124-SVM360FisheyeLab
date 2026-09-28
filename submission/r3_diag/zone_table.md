# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 13 | 2 | 1 | 6 | 7 | MISSING (2) |
| mid | 5 | 1 | 1 | 2 | 3 | ATTRIBUTE (1) |
| edge | 2 | 0 | 0 | 1 | 2 | — |

## Nhận xét

- **Zone gãy nhiều nhất:** Về số lượng tuyệt đối, zone `center` có mật độ tập trung cao nhất (n_ref = 13) nên có số lỗi cao nhất ở cả Annotator (2 missing, 1 spurious) và Model (6 missing, 7 thừa). Về tỷ lệ tương đối, Model (M) suy giảm mạnh ở vùng `mid` và `edge` khi bỏ sót tới 50% vật thể ở edge (1/2 missing) và sinh ra tới 5 false positive thừa ở mid và edge cộng lại.
- **Giả thuyết nguyên nhân & Giới hạn:**
  - *Model:* Bị lệch miền dữ liệu (domain shift) do kiến trúc pretrain trên ảnh phối cảnh phẳng (pinhole). Khi gặp thấu kính mắt cá fisheye góc rộng, hiện tượng méo biên (barrel distortion) làm biến dạng hình học vật thể, khiến model bỏ sót đối tượng ở rìa và nhầm lẫn các chi tiết nền (bóng râm, hàng rào) thành vật thể (M spurious).
  - *Annotator (L):* Lỗi chủ yếu do bỏ sót các phương tiện nhỏ ở xa hoặc nhầm lẫn class đặc thù (`ThreeWheeler` bị nhầm thành `Car`/`Truck`) và chưa bao quát thuộc tính `truncated` ở sát rìa.
  - *Giới hạn:* Đánh giá dựa trên slice 3 frame chỉ cung cấp mẫu thăm dò cục bộ, chưa đủ đại diện thống kê cho mọi điều kiện thời tiết, ánh sáng và phân bố phương tiện của cả hệ thống SVM 4 camera.
