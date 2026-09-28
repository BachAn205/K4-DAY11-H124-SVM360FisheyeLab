# Guideline patch

- **Rule mới đề xuất:** Bổ sung phụ lục quy chuẩn nhận diện phân biệt `ThreeWheeler` chở hàng (máy kéo nhỏ, xe lôi, e-rickshaw) và `Truck`/`Car`, đồng thời làm rõ tiêu chí xử lý các đối tượng nhỏ sát ngưỡng chiều cao $H=40$ px khi bị che khuất một phần trong vùng méo quang học rìa kính fisheye.
- **Áp dụng cho:** Các class `ThreeWheeler`, `Truck`, `Car` và thuộc tính `occluded`, `truncated` trên toàn bộ các frame fisheye.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Rule R04 hiện tại chỉ liệt kê tên phương tiện mà thiếu tiêu chí nhận diện đặc trưng khi góc nhìn bị méo biên bởi thấu kính fisheye, dẫn đến việc annotator dễ nhầm xe ba bánh thành xe tải nhỏ (Truck). Đồng thời, R01 chưa nêu rõ cách đo khi vật thể bị che một phần thân dưới (chiều cao nhìn thấy < 40px nhưng kích thước thực tế > 40px).
- **`rules_version` mới:** `v1.1.0`
- **Hiệu lực từ:** Vòng `rework` (P5) và các đợt gán nhãn tiếp theo của dự án.
