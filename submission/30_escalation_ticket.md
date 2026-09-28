# Escalation ticket

## Ticket 1

- **Frame:** `adasind_086220.jpg`
- **Ảnh chụp:** `submission/screenshots/adasind_086220_escalation.jpg`
- **Expected impact:** Tránh trừ điểm sai cho Annotator và Model khi cả hai nguồn đều phát hiện chính xác đối tượng xe máy/xe đạp thực tế này trên đường nhưng teaching reference bị thiếu (lỗi `E0_reference_defect`).
- **Owner:** `qa`
- **Recommendation:** Bổ sung box cho đối tượng `L5+M8` tại tọa độ nhìn thấy trên frame `adasind_086220.jpg` vào teaching reference của slice B2-mid để đảm bảo tính khách quan của ground truth.
