# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| `adasind_086220.jpg` (Vùng rìa/mid xung đột) | 3 ca (1 SPURIOUS box dưới ngưỡng H=40, 1 ATTRIBUTE truncated rìa, 1 LM_noR xung đột reference) | Lỗi vi phạm ngưỡng kích thước R01, sai thuộc tính viền biên R05 và phát hiện nghi vấn thiếu sót của reference (E0) | Screenshot `adasind_086220_escalation.jpg` và bảng đối chiếu L vs M vs R |
| `adasind_060000.jpg` (Vùng trung tâm/hậu cảnh) | 3 ca (1 SPURIOUS box H=29.7px, 2 MISSING do khoảng cách xa) | Vùng trung tâm có mật độ tập trung cao, dễ vi phạm ngưỡng lọc nhiễu nhỏ hơn 40px và bỏ sót đối tượng nhỏ xa | Bảng đối chiếu `compare.md`, `zone_table.md` và file `compare.html` |

Giới hạn của kết luận từ ba frame ADASIND: Bộ dữ liệu chỉ gồm 3 frame trong cùng một điều kiện thời tiết ban ngày, không đủ tính khái quát thống kê cho toàn bộ hệ thống SVM 360 độ gồm 4 camera đa hướng.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh
như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:
- Áp dụng kỹ thuật lấy mẫu phân tầng (stratified sampling) kết hợp bước nhảy thời gian (temporal subsampling, ví dụ lấy cách nhau ít nhất 5--10 giây) để tránh trùng lặp dữ liệu liên tiếp trong cùng một cảnh quay.
- Kế hoạch 200 frame được thiết kế ưu tiên tìm kiếm các ca biên/ca khó (hard cases, seam, chói sáng, ngược chiều) nhằm phát hiện sớm các lỗ hổng hệ thống và quy tắc, chứ không nhằm đo lường tỷ lệ lỗi tổng thể (error rate) trên tập 50.000 frame.
