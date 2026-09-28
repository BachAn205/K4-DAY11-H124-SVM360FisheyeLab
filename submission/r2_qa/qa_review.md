# QA review · B2-mid

Mã khóa: 8496-E40E

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_060000.jpg | L1 | R01 | Box Pedestrian ở hậu cảnh có chiều cao H=29.7px < 40px, dưới ngưỡng kích thước bắt buộc của R01 |
| adasind_086220.jpg | L5 | R01 | Box Bike ở xa có chiều cao H=34.2px < 40px, không đạt ngưỡng tối thiểu |
| adasind_102750.jpg | L6 | R01 | Box Bike nhỏ ở xa có chiều cao H=16.6px < 40px, nằm ngoài phạm vi gán nhãn |
| adasind_086220.jpg | L2 | R05 | ThreeWheeler kích thước lớn sát mép khung hình cần rà soát lại thuộc tính truncated theo R05 |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
