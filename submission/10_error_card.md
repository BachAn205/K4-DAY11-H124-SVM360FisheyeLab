# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B2 | MISSING | 8 |
| center | B2 | SPURIOUS | 10 |
| center | C0 | MISSING | 1 |
| center | C0 | SPURIOUS | 3 |
| edge | B2 | MISSING | 1 |
| edge | B2 | SPURIOUS | 2 |
| mid | B2 | ATTRIBUTE | 2 |
| mid | B2 | MISSING | 2 |
| mid | B2 | SPURIOUS | 3 |
| mid | B2 | WRONG_CLASS | 1 |
| mid | C0 | SPURIOUS | 1 |
| unknown | B2 | SPURIOUS | 2 |

## Top defects
- SPURIOUS: 21 (ví dụ frame adasind_019560.jpg)
- MISSING: 12 (ví dụ frame adasind_019560.jpg)
- ATTRIBUTE: 2 (ví dụ frame adasind_062370.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- **Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy:**
  - Hai dạng lỗi chiếm áp đảo là `SPURIOUS` (21 ca) và `MISSING` (12 ca).
  - Về phía người gán nhãn (`E1_annotator_error`): Do chưa kiểm soát chặt chẽ ngưỡng chiều cao $H=40$ px quy định tại R01, dẫn tới việc vẽ các vật thể hậu cảnh quá nhỏ ($H \approx 20\text{--}22$ px như `L1, L7` ở frame `adasind_069450.jpg` và `L9` ở `adasind_117120.jpg`), đồng thời nhầm lẫn class đặc thù phương tiện địa phương (`ThreeWheeler` vs `Car/Truck` theo R04).
  - Về phía Model (`E4_model_domain`): Model YOLO đóng băng được huấn luyện trên camera pinhole thông thường, khi áp dụng trực tiếp lên fisheye gặp độ méo góc rộng và nhiễu viền tròn, sinh ra hàng loạt false positive (SPURIOUS) ở vỉa hè và bóng râm (`M3, M4, M5` ở `adasind_062370.jpg`).
- **Cách sửa và ai nhận việc (`owner`):**
  - `owner: annotator`: Thực hiện rework trên slice để xóa các box dưới ngưỡng $H=40$, chuẩn hóa lại class `ThreeWheeler` cho các phương tiện 3 bánh chở khách/hàng và gộp rider vào `Bike` theo đúng R03/R04.
  - `owner: ai_team`: Đưa dữ liệu fisheye đã gán nhãn vào pipeline huấn luyện lại/finetune model, thêm data augmentation mô phỏng méo thấu kính fisheye và che khuất `ego_body`.
  - `owner: qa`: Tăng cường rule kiểm tra tự động kích thước bounding box và thiết lập quy trình leo thang (escalation) cho các ca mơ hồ.
- **Bằng chứng:**
  - Bằng chứng hình ảnh trong `submission/screenshots/` và `submission/r3_diag/model_compare.html`.
  - Các dòng findings tiêu biểu: `adasind_069450.jpg / L1` (R01 - kích thước 22.4px), `adasind_019560.jpg / L9` (R04 - phân loại sai ThreeWheeler), `adasind_062370.jpg / M3-M10` (R02 - model domain shift).
