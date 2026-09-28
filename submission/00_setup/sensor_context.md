# Sensor context

- **Rig:** Camera mắt cá (fisheye lens) góc siêu rộng gắn trên phương tiện di chuyển (xe ego) ở góc nhìn hướng về phía trước/bên hông nhằm ghi lại giao thông và lề đường theo thời gian thực. Do không có thông số calibration chi tiết từ dataset ADASIND gốc, độ méo quang học quan sát thấy tăng mạnh từ tâm ra rìa (r/R càng lớn độ méo càng cao).
- **`ego_body`:** Quan sát thấy một phần thân xe/gương chiếu hậu của xe ego thò vào ở góc dưới bên trái khung hình (chạy dọc từ cạnh trái xuống góc dưới). Vùng này bị che bởi xe gắn camera nên cần vẽ polygon `ignore_region` với thuộc tính `reason = ego_body`.
- **Vòng kính (lens circle):** Vòng kính tròn fisheye nằm ở trung tâm và chiếm khoảng 80-85% diện tích khung hình chữ nhật 1080x1920. Các phần góc và biên trên/dưới nằm ngoài vòng kính tạo thành vành đen viền quang học, được khoanh vùng bằng polygon `ignore_region` với `reason = lens_border`.
