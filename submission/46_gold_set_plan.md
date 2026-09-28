# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn                                                                                            | Vì sao dễ sai                                                                                                                                   | Annotation space / calibration cần giữ                                                                          | Cách review trước khi gọi là gold                                                                                                                                                     |
| --------- | ------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| front     | Vật thể bị che khuất bởi xe phía trước; xe/người ở xa (khoảng 30–50m); ánh sáng chói từ phía trước (mặt trời) | Che khuất dẫn đến FN (False Negative) hoặc box sai kích thước; ánh sáng xấu gây khó khăn trong việc phân biệt class (ví dụ: xe đen vs bóng đổ). | Thông tin calibration (focal length, distortion coefficients), FOV, vị trí camera so với ego vehicle, timestamp | Review bằng **ít nhất 3 annotator độc lập**, so sánh với model đóng băng (IoU ≥ 0.7), kiểm tra consistency giữa các frame lân cận. Sử dụng overlay để xác nhận bounding box và class. |
| rear      | Vật thể nhỏ (xe xa >50m); bị méo mạnh do góc nhìn fisheye; che khuất bởi xe phía sau                          | Vật thể xa bị bỏ sót (FN) do kích thước <40px; méo fisheye gây sai lệch hình dạng box.                                                          | Calibration (distortion parameters), FOV, độ cao lắp camera, vùng chồng (seam) với camera left/right            | Review bằng **2 annotator + model**, ưu tiên frame có vật thể ở rìa FOV. Kiểm tra độ chính xác của polygon `ignore_region` cho lens border.                                           |
| left      | Che khuất bởi thân xe (ego body); vật thể ở góc chết (blind spot); ánh sáng thiếu (bóng đổ từ xe)             | Ego body che khuất dẫn đến FN; blind spot có thể bỏ sót người đi bộ.                                                                            | Calibration, vị trí camera so với thân xe, vùng chồng với front/rear                                            | Review bằng **2 annotator + teaching reference**, tập trung vào frame có vật thể gần biên giới (seam) với front/rear. Xác nhận `ignore_region` cho ego body.                          |
| right     | Che khuất bởi gương chiếu hậu; ánh sáng chói (nếu mặt trời ở bên phải); vật thể ở rìa frame                   | Gương che khuất gây FN; ánh sáng chói dẫn đến nhầm lẫn class (ví dụ: xe trắng vs nền sáng).                                                     | Calibration, vị trí gương chiếu hậu, FOV, timestamp                                                             | Review bằng **2 annotator + model**, ưu tiên frame có ánh sáng phức tạp. Kiểm tra xóa nhãn trên gương (nếu có).                                                                       |

- **Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):**
  - Khi **thay đổi phần cứng camera** (vị trí, góc lắp, model camera mới).
  - Khi **cập nhật calibration** (focal length, distortion parameters thay đổi).
  - Khi **thay đổi rule gán nhãn** (ví dụ: định nghĩa mới cho class `Bike` hoặc `Pedestrian`).
  - Khi phát hiện **lỗi hệ thống** (ví dụ: model liên tục sai trên một loại vật thể nhất định).
  - **Tần suất:** Refresh định kỳ (ví dụ: mỗi 6 tháng) hoặc sau khi có **≥5% conflict không giải thích được** giữa gold set và model.

- **Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:**
  - **Policy:** Chỉ ghép hai box từ hai camera **nếu có**:
    1. **Timestamp trùng khớp** (cùng frame hoặc sai lệch ≤ 10ms).
    2. **Calibration đầy đủ** (biết vị trí 3D tương đối giữa các camera).
    3. **Evidence hình học**: IoU 3D ≥ 0.5 (nếu có) **hoặc** khoảng cách giữa center point 2D sau projection ≤ 20px.
    4. **Quолy mối quan hệ**: Box trên hai camera có cùng class, kích thước tương thích (sai lệch ≤ 30%).
  - **Không tự ghép** nếu thiếu calibration hoặc timestamp không khớp. Ghi nhận vào `30_escalation_ticket.md` nếu cần chính sách mới.

- **Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:**
  - **Peer agreement trên một camera** chỉ chứng minh **sự nhất trí trong slice đó**, không đại diện cho **toàn bộ hệ thống bốn camera**.
  - **Quality report (precision/recall) trên một camera** có thể cao, nhưng **lỗi hệ thống** (ví dụ: calibration sai, seam conflict) **không xuất hiện** trên camera đó.
  - **Bốn camera có đặc thù riêng**: front/rear có FOV rộng về phía trước/sau, left/right bị ảnh hưởng bởi thân xe/gương. Lỗi trên camera left (ví dụ: che khuất bởi ego body) **không thể phát hiện** bằng data từ camera front.
  - **Seam (vùng chồng)** giữa các camera đòi hỏi **kiểm chứng chéo** (cross-validation). Gold set phải cover **cả bốn camera** và **các vùng seam** để đảm bảo tính nhất quán của hệ thống.
