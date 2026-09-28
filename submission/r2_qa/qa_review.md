# QA review · B2-dense

Mã khóa: 8755-E75E

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_069450.jpg | L1 | R01 | Box xe Car ở xa có chiều cao H=22.4px < 40px, dưới ngưỡng kích thước bắt buộc của R01 |
| adasind_069450.jpg | L7 | R01 | Box Pedestrian có chiều cao H=22.1px < 40px, nằm ngoài phạm vi gán nhãn |
| adasind_117120.jpg | L9 | R01 | Box Car ở hậu cảnh có chiều cao H=21.9px < 40px, không đạt ngưỡng H=40 |
| adasind_062370.jpg | L7 | R05 | ThreeWheeler kích thước lớn ở mép khung hình cần rà soát lại attribute truncated |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
