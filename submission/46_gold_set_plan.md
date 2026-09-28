# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. "Gold set" ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Giao lộ đông đúc, người đi bộ cắt ngang nhanh, ánh sáng ngược/tối, vật tại seam góc trước-trái và trước-phải | Vật ở edge zone bị méo fisheye mạnh, dễ nhầm class Bike/ThreeWheeler ở tầm gần, crowd_or_group xuất hiện nhiều ở giao lộ | Đảm bảo frame có phân bố đều các zone center/mid/edge; giữ frame khi calibration rig thay đổi (xe thay camera) | Ít nhất 2 annotator độc lập review, Lab Coach xác nhận trước khi đưa vào gold; kiểm agreement ≥90% trên hard case |
| rear | Lùi trong bãi đỗ hẹp, người/xe đạp tiếp cận từ phía sau, điều kiện đêm hoặc ánh đèn hậu xe khác | ego_body lớn hơn camera front, dễ nhầm vùng ignore với vật thật; parking_line gây nhiễu khi tìm vật | Giữ frame đêm và hoàng hôn riêng biệt; calibration lens_border có thể khác do góc gắn camera sau | Review theo cặp frame ban ngày/ban đêm cùng scene; không gọi là gold nếu chưa kiểm đủ cả hai điều kiện ánh sáng |
| left | Xe máy/xe đạp từ ngõ bất ngờ, ThreeWheeler (xích lô) sát lề, đông đúc tắc đường bên vỉa hè | Vật di chuyển ngang nhanh dễ bị truncated nhiều; ThreeWheeler dễ nhầm với Bike nếu nhỏ và méo | Giữ frame có vật tại zone edge để kiểm fill ratio; calibration theo mùa (bóng đổ thay đổi) | Annotator phải xem ảnh gốc fisheye, không xem ảnh đã undistort; xác nhận R07 ego_body đúng trước khi commit gold |
| right | Xe máy dày đặc ở làn phải (đặc thù VN), crowd_or_group bên lề, vật ở seam phải-trước/phải-sau | Mật độ Bike/Pedestrian cao nhất trong 4 camera ở môi trường đô thị VN; dễ thiếu box khi crowd | Giữ frame rush hour sáng/chiều riêng; đảm bảo có frame parking_line/lề đường để kiểm free_space không nhầm | So sánh agreement giữa 2 annotator trên từng ca crowd_or_group; refresh gold khi policy crowd thay đổi |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):
  Cần refresh khi: (1) thay camera hoặc thay vị trí gắn camera thay đổi vùng ego_body/lens_border; (2) rules_version bump từ v1.0.0 lên v1.x.x do có thay đổi định nghĩa class hoặc ngưỡng H; (3) môi trường vận hành mở rộng (thêm đêm, thêm mưa) mà gold set hiện tại chưa bao phủ; (4) precision/recall trên production giảm >5% so với baseline gold.

- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:
  Ca điển hình: một xe máy (Bike) xuất hiện tại seam giữa camera right và camera rear khi xe đang rẽ phải. Camera right thấy phần đầu xe máy ở zone edge, camera rear thấy phần thân ở zone mid — hai box với class Bike nhưng IoU=0 vì không chồng nhau (ảnh khác nhau). Chưa thể tự động gán cùng track ID mà không có: (a) timestamp đồng bộ chứng minh cùng thời điểm, (b) calibration extrinsic để biết vị trí 3D tương đối, (c) policy rõ: giữ cả 2 box để tầng sau xử lý hay chọn camera chính. Cần Lab Coach quyết định policy trước khi annotator gán nhãn seam.

- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:
  ADASIND chỉ có 1 camera góc nhìn cố định. Agreement giữa 2 annotator trên 48 frame của 1 camera chứng minh nhãn nhất quán cho góc nhìn đó, nhưng không cover: (a) các góc méo khác nhau của 3 camera còn lại; (b) vùng seam nơi cùng một vật xuất hiện với kích thước và zone khác nhau trên 2 camera; (c) điều kiện ánh sáng đặc thù của từng camera (camera rear thường ngược sáng vào sáng sớm; camera right thường bị lóa buổi chiều). Gold set 4 camera phải được kiểm riêng trên từng camera với annotator đã quen góc nhìn đó.
