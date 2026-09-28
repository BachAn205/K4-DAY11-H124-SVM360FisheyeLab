# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 9 | 3 | 3 | 5 | 8 | MISSING (3) |
| mid | 8 | 0 | 1 | 4 | 10 | ATTRIBUTE (2) |
| edge | 3 | 0 | 0 | 2 | 0 | — |

## Nhận xét

- **Zone gãy nhiều nhất:** Về số lượng tuyệt đối, zone `center` có mật độ tập trung cao nhất (n_ref = 9) nên có số lỗi bỏ sót cao nhất ở cả Annotator (3 missing, 3 spurious) và Model (5 missing, 8 thừa). Về tỷ lệ false positive, vùng `mid` là nơi Model (M) gãy nhiều nhất với 10 box thừa (spurious) do nhầm lẫn các chi tiết ven đường và bóng râm. Vùng `edge` model bỏ sót 2/3 đối tượng do biến dạng thấu kính mạnh.
- **Giả thuyết nguyên nhân & Giới hạn:**
  - *Model:* Bị lệch miền dữ liệu (domain shift) do kiến trúc pretrain trên ảnh phối cảnh phẳng (pinhole). Khi gặp thấu kính mắt cá fisheye góc rộng, hiện tượng méo biên (barrel distortion) làm biến dạng hình học vật thể, khiến model bỏ sót đối tượng ở rìa và nhầm lẫn các chi tiết nền (bóng râm, hàng rào) thành vật thể (M spurious tới 18 box ở center và mid).
  - *Annotator (L):* Lỗi chủ yếu do bỏ sót các phương tiện nhỏ ở xa hoặc nhầm lẫn thuộc tính `truncated` ở sát rìa khung hình.
  - *Giới hạn:* Đánh giá dựa trên slice 3 frame chỉ cung cấp mẫu thăm dò cục bộ, chưa đủ đại diện thống kê cho mọi điều kiện thời tiết, ánh sáng và phân bố phương tiện của cả hệ thống SVM 4 camera.
