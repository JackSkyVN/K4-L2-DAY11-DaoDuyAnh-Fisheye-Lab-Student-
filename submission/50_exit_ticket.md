# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?
   - Đây không phải là lỗi `DUPLICATE` thông thường mà cần một quy tắc riêng về "cross-camera association / seam fusion".
   - Vì sao: Mỗi camera fisheye ghi lại hình ảnh từ một tâm quang học và góc chiếu khác nhau, chịu các biến dạng phi tuyến và góc nhìn khác nhau; việc xuất hiện box trên ảnh 2D của từng camera độc lập là hiển nhiên về mặt quang học. Nếu máy móc đánh dấu DUPLICATE và xóa đi một box thì camera tương ứng sẽ mất dấu vật cản trong trường nhìn của nó. Ở tầng SVM/360, bài toán yêu cầu hợp nhất hai box này vào cùng một thực thể trong không gian 3D (BEV) thông qua ma trận hiệu chuẩn camera ngoại (extrinsics) và thuật toán ghép seam, gán chung một `global_track_id` thay vì xử lý như hai vật cản tách biệt.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.
   - Khi nào giữ cùng track ID: Khi vật di chuyển liên tục trong trường nhìn, bảo toàn quỹ đạo và đặc trưng hình dạng/màu sắc, hoặc bị che khuất ngắn hạn (occlusion < 1–2s) nhưng vị trí xuất hiện lại hoàn toàn phù hợp với vận tốc ngoại suy.
   - Khi nào thêm keyframe: Khi vật thể thay đổi đột ngột về hình học (đang nhìn ngang chuyển sang nhìn chéo góc do xe rẽ hoặc do méo fisheye phóng đại ở rìa kính), hoặc chuyển từ trạng thái di chuyển sang dừng đỗ.
   - Khi nào đặt trạng thái Outside: Khi toàn bộ vật thể đi ra ngoài biên khung hình (hoặc đi sâu vào vùng viền kính `lens_border`/thân xe `ego_body` không còn nhìn thấy).
   - Bằng chứng cần trước khi nối track qua hai camera: (1) Đồng bộ thời gian chính xác (timestamp khớp nhau), (2) Ma trận hiệu chuẩn ngoại (extrinsic calibration) và trường nhìn giao thoa giữa 2 camera, (3) Tính liên tục của vector vận tốc (vật ra khỏi mép camera này thì đồng thời đi vào mép tương ứng của camera kia), (4) Độ tương đồng đặc trưng ngoại quan (Re-ID: màu xe, kiểu dáng, biển số nếu có).

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?
   - Chỗ tin nhãn đúng nhưng khác reference: Tại frame `adasind_230910.jpg`, đối tượng `L7+M9` (tôi và mô hình đóng băng đều gán nhãn `Truck`) so với reference `R7` (gán nhãn `Car`). Dựa vào quan sát thực tế, đây là dòng xe van/xe tải nhẹ có khoang thùng chở hàng riêng phía sau, phù hợp với định nghĩa xe vận tải chuyên dụng trong giao thông đô thị hơn là xe con du lịch.
   - Cách xử lý: Tôi không sửa âm thầm nhãn theo reference, mà ghi nhận finding với `why=E2_guideline_gap`, gắn `action=escalate`, viết ticket escalation (`30_escalation_ticket.md`) và đề xuất bản vá quy chuẩn (`20_guideline_patch.md`) đồng thời ghi log với `status=escalated` trong decision log để hội đồng kỹ thuật xem xét.
   - Nếu làm lại: Tôi sẽ vẽ polygon `ignore_region reason="ego_body"` ngay từ bước đầu tiên trước khi vẽ bất kỳ box nào để loại bỏ triệt để các box spurious trên thân xe ego; đồng thời đọc kỹ mục ngoại lệ phân lớp xe địa phương (R04) và đo chiều cao H trên ảnh gốc trước khi quyết định gán nhãn cho các vật ở xa.
