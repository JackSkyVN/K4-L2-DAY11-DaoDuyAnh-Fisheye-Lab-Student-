# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| adasind_128310.jpg & adasind_140160.jpg (Zone Edge / Ego Body) | 4 ca: SPURIOUS & IGNORE_SCOPE (L1, L4 vẽ đè lên thân xe ego; L3, L4 vi phạm H < 40px) | Lỗi vẽ box lên thân xe ego (vi phạm R07/R09) có mức độ nguy hiểm cao vì khiến xe tự hành nhận diện chính mình là vật cản; vùng biên quang học méo mạnh nhất nên annotator rất dễ lặp lại lỗi. | Ảnh overlay hiển thị polygon `ego_body`, vòng kính `lens_border`, và toạ độ các box bị xoá trong XML. |
| adasind_230910.jpg (Zone Mid / Đô thị mật độ cao) | 7 ca: WRONG_CLASS, MISSING, ATTRIBUTE (L7 Truck vs Car, R8/R10 sót vật che khuất, L11/L12 thiếu truncated) | Mật độ xe cộ và người đi bộ dày đặc tập trung nhiều rủi ro: nhầm lẫn ranh giới phân loại phương tiện, bỏ sót vật thể bị che khuất và sai lệch thuộc tính truncated ở mép kính. | Ảnh chụp chi tiết ca escalation, ma trận nhầm lẫn `local_quality_confusion.csv`, bảng so sánh L/R/M trong `model_compare.md`. |

Giới hạn của kết luận từ ba frame ADASIND: Slice 3 frame chỉ có 20 đối tượng tham chiếu trong điều kiện ban ngày đô thị khô ráo góc camera trước. Kích thước mẫu quá nhỏ không đủ ý nghĩa thống kê để kết luận tỷ lệ lỗi tổng thể, không phản ánh các biến thể thời tiết (ban đêm, mưa, sương mù) hay góc đặt camera hông/sau với vùng quan sát thân xe ego hoàn toàn khác biệt.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:
- Để kiểm soát độ phủ, cần áp dụng quy tắc giãn cách thời gian (temporal subsampling) tối thiểu 2–5 giây giữa các frame được trích xuất trong cùng một clip, tránh hiện tượng tự tương quan cao (autocorrelation) khi các frame liền kề chỉ lặp lại cùng một nhóm xe và cùng một góc nhìn. Mỗi camera (front, rear, left, right) phải được phân bổ cân đối giữa điều kiện tiêu chuẩn (normal) và trường hợp biên thách thức (hard - seam zone, chói lóa, trời tối).
- Kế hoạch 200 frame này là phép lấy mẫu có chủ đích (targeted/stratified audit) nhằm phát hiện các chế độ hỏng hóc (failure modes) và lỗ hổng quy chuẩn tại các điểm rủi ro cao. Vì mẫu không được rút ngẫu nhiên đồng đều (I.I.D) từ toàn bộ không gian dữ liệu vận hành thực tế, nên tỷ lệ lỗi quan sát được từ 200 frame này chỉ dùng để định hướng khu vực cần soi kỹ và hoàn thiện guideline, không thể suy diễn thành tỷ lệ lỗi đại diện (unbiased error rate) của toàn hệ sinh thái sản xuất.
