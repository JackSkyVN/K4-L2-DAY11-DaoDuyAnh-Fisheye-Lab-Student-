# QA review · B3-mid

Mã khóa: D7A1-A892

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_128310.jpg | L4 | R07 | Box Bike vẽ đè lên phần thân xe/gương chiếu hậu ego ở góc trái dưới; đây là vùng ego_body cần gắn polygon ignore_region reason=ego_body thay vì box đối tượng. |
| adasind_140160.jpg | L3 | R01 | Box Bike có chiều cao H=36.1px < 40px; dưới ngưỡng kích thước tối thiểu theo quy định R01. |
| adasind_140160.jpg | L4 | R01 | Box Bike có chiều cao H=8.7px < 40px; đối tượng quá nhỏ ở hậu cảnh xa vi phạm ngưỡng H=40 của R01. |
| adasind_230910.jpg | L11 | R05 | Box Bike chạm mép ảnh bên trái (x=0) nhưng thuộc tính truncated đang là false; cần bật truncated=true theo R05. |
| adasind_230910.jpg | L12 | R05 | Box Bike chạm mép ảnh bên phải (x=1080) và cắt qua vành kính nhưng truncated đang là false; cần bật truncated=true theo R05. |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
