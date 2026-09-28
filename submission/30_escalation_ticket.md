# Escalation ticket

## Ticket 1

- **Frame:** `adasind_230910.jpg` (đối tượng L7 / M9 so với R7)
- **Ảnh chụp:** `submission/screenshots/escalation_ticket1_adasind_230910.jpg`
- **Expected impact:** Ảnh hưởng trực tiếp tới độ tin cậy phân lớp giữa hai class cốt lõi `Car` và `Truck`. Trong các đô thị Ấn Độ, nhóm phương tiện mini-truck/van thương mại nhỏ chiếm ~7–12% lưu lượng; nếu không có chuẩn phân loại thống nhất, sai số gán nhãn sẽ lan sang cả mô hình dự đoán và làm giảm chỉ số mAP/IoU của toàn hệ thống nhận thức.
- **Owner:** `guideline`
- **Recommendation:** Guideline team cần xem xét và phê duyệt quy định bổ sung R04b (tại `submission/20_guideline_patch.md`), xác định chuẩn class cho phương tiện cụ thể tại frame `adasind_230910.jpg` là `Truck` (do có thùng chở hàng sau chuyên dụng), đồng thời cập nhật bộ ảnh catalogue chuẩn để annotator và QA có căn cứ đối chiếu rõ ràng.
