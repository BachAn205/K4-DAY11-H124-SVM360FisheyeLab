# Escalation ticket

## Ticket 1

- **Frame:** `adasind_069450.jpg`
- **Ảnh chụp:** `submission/screenshots/adasind_069450_escalation.jpg`
- **Expected impact:** Tránh trừ điểm sai cho Annotator và Model khi cả hai nguồn đều phát hiện chính xác phương tiện ThreeWheeler thực tế này trên đường nhưng teaching reference bị thiếu (lỗi `E0_reference_defect`).
- **Owner:** `qa`
- **Recommendation:** Bổ sung box `ThreeWheeler` cho đối tượng `L5+M7` tại tọa độ nhìn thấy trên frame `adasind_069450.jpg` vào teaching reference của slice B2-dense để đảm bảo tính khách quan của ground truth.
