# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Đèn pha chói lóa ban đêm, xe cắt ngang đột ngột | Chói sáng làm mất viền, vận tốc góc cao gây motion blur | Fisheye gốc, thông số nội tại camera (intrinsics) | Double review độc lập từ 2 annotator kinh nghiệm + đối soát chéo của QA Lead |
| rear | Điểm mù đuôi xe, lùi xe ban đêm, đèn pha rọi thẳng | Ánh sáng đèn xe sau làm lóa camera, khoảng cách gần dễ cắt box | Fisheye gốc kèm thông số vị trí gắn rig đuôi xe | Kiểm tra nghiêm ngặt thuộc tính `truncated` và `occluded` khi vật vào gần cản sau |
| left | Vùng chồng seam góc trước trái và sau trái | Vật thể biến dạng mạnh ở biên thấu kính, dễ xuất hiện 2 lần trên 2 camera | Tọa độ camera extrinsics và thời gian đồng bộ frame (timestamps) | Review đồng thời cặp frame cùng timestamp của camera Front và Left |
| right | Vỉa hè đông người đi bộ và chướng ngại vật tĩnh che khuất | Mật độ giao thông hỗn hợp cao, người đi bộ bị che bởi cây cối/cột điện | Fisheye gốc và mask vùng tĩnh của xe ego | Phân tích tách biệt người dắt xe và rider, kiểm tra kỹ checklist 9 mục |

- **Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):** Khi có sự thay đổi về phần cứng cảm biến (thay loại lens/camera), thay đổi vị trí góc đặt rig xe (re-calibration), hoặc khi guideline gán nhãn được bump phiên bản mới (ví dụ từ v1.0.0 lên v1.1.0).
- **Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:** Cần có bằng chứng đồng bộ chính xác về thời gian (timestamp microsecond), ma trận biến đổi tọa độ calibration không gian thực tế giữa hai camera liền kề, và policy hợp nhất (fusion threshold IoU trong không gian 3D/BEV). Không tự ý gộp hai box hoặc gán cùng track ID chỉ dựa trên quan sát mắt thường ở không gian ảnh 2D.
- **Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:** Bởi vì mỗi vị trí camera (trước, sau, hai bên sườn) có đặc tính quang học, trường nhìn (FOV), hướng ánh sáng và góc tiếp xúc giao thông hoàn toàn khác nhau. Sự đồng thuận trên 1 camera không thể khái quát hóa cho các ca phức tạp tại vùng giao thoa (seam) hoặc điểm mù đặc thù của các camera khác.
