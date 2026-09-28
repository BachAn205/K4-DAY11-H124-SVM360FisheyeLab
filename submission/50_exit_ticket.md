# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. **Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy tắc riêng? Vì sao?**
   - Đây **không phải là lỗi DUPLICATE** của người gán nhãn, mà **cần một quy tắc xử lý riêng (policy/cross-camera association)**.
   - *Vì sao:* Tại vùng chồng lấn (seam) giữa hai camera mắt cá lân cận (ví dụ Front và Left), cùng một vật thể vật lý trong thế giới thực sẽ rơi vào trường nhìn của cả hai cảm biến độc lập. Do sự khác biệt về góc đặt (extrinsics), độ méo quang học và thời điểm phơi sáng, hình chiếu 2D của vật thể trên hai ảnh là hoàn toàn khác nhau. Ở tầng gán nhãn 2D từng camera đơn lẻ, mỗi camera phải gán đúng vật thể nhìn thấy trong phạm vi của mình. Việc liên kết chúng lại thành một thực thể duy nhất phải do module BEV / Tracking / Fusion đa camera đảm nhiệm dựa trên calibration và đồng bộ timestamp.

2. **Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.**
   - *Giữ cùng track ID:* Khi vật thể di chuyển liên tục, có thể theo dõi được quỹ đạo xuyên suốt qua các frame liên tiếp mà không bị ngắt quãng hoàn toàn hoặc thay đổi nhận dạng cốt lõi.
   - *Thêm keyframe / Outside:* Thêm keyframe khi vật thay đổi trạng thái hình học/hướng di chuyển đột ngột. Đánh dấu trạng thái `Outside` (hoặc kết thúc track) khi vật thể hoàn toàn đi ra khỏi vòng kính (vượt ra ngoài viền quang học) hoặc bị che khuất hoàn toàn quá số frame quy định.
   - *Bằng chứng trước khi nối track qua 2 camera:* Cần có timestamp đồng bộ tuyệt đối (synchronization), ma trận hiệu chuẩn ngoại (extrinsics) xác định mối quan hệ không gian giữa 2 camera, và tính toán đường đi liên tục của vật thể trong không gian 3D thế giới thực (world coordinates / BEV plane) để xác nhận hai track 2D thực sự xuất phát từ cùng một đối tượng.

3. **Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`), bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?**
   - *Trường hợp cụ thể:* Tại frame `adasind_069450.jpg`, đối tượng `L5` (xe ba bánh ở hậu cảnh) được cả Annotator và Model YOLO phát hiện (`LM_noR`), nhưng Teaching Reference lại bỏ sót (`E0_reference_defect`).
   - *Cách xử lý:* Thay vì vội vàng xóa nhãn để khớp 100% với reference một cách cơ học, tôi đã lập `30_escalation_ticket.md` kèm bằng chứng ảnh chụp và ghi nhận vào `40_decision_log.csv` với trạng thái `escalated`.
   - *Kinh nghiệm đổi mới nếu làm lại:* Tôi sẽ áp dụng checklist self-QC chặt chẽ ngay từ lần vẽ đầu tiên, đặc biệt là dùng thước đo kiểm tra ngưỡng chiều cao $H=40$ px để tránh vẽ thừa các xe quá nhỏ ở hậu cảnh, và luôn chuẩn bị sẵn tâm thế phản biện ground truth một cách khoa học dựa trên bằng chứng và quy chuẩn.
