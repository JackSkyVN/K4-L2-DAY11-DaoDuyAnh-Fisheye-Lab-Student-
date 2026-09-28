# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B3 | MISSING | 4 |
| center | B3 | SPURIOUS | 4 |
| center | C0 | SPURIOUS | 1 |
| edge | B3 | ATTRIBUTE | 1 |
| edge | B3 | IGNORE_SCOPE | 3 |
| edge | B3 | MISSING | 1 |
| edge | B3 | SPURIOUS | 7 |
| edge | C0 | SPURIOUS | 1 |
| edge | C0 | WRONG_CLASS | 1 |
| mid | B3 | ATTRIBUTE | 2 |
| mid | B3 | BOX_GEOMETRY | 1 |
| mid | B3 | MISSING | 5 |
| mid | B3 | SPURIOUS | 7 |
| mid | B3 | WRONG_CLASS | 2 |
| mid | C0 | MISSING | 1 |

## Top defects
- SPURIOUS: 20 (ví dụ frame adasind_019560.jpg)
- MISSING: 11 (ví dụ frame adasind_019560.jpg)
- WRONG_CLASS: 3 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy:
  1. Nhóm lỗi `SPURIOUS` (20 ca) chiếm tỉ trọng lớn nhất, bắt nguồn từ hai phía:
     - `E1_annotator_error`: Người gán nhãn vẽ nhầm box đối tượng giao thông lên phần thân xe/gương xe ego ở zone edge (`adasind_128310.jpg L4`, `adasind_140160.jpg L1`) do chưa vẽ polygon `ego_body` ngay từ đầu; vẽ các box quá nhỏ ở hậu cảnh xa vi phạm ngưỡng H < 40px (`adasind_140160.jpg L3, L4`); hoặc tách người ngồi trên xe thành box Pedestrian riêng vi phạm luật Rider R03 (`adasind_019560.jpg L3`, `adasind_230910.jpg L9`).
     - `E4_model_domain`: Mô hình phát hiện vật thể gặp hiện tượng dự đoán ảo (false positives) ở rìa mép và trung tâm (`M1-M6`) do phân phối dữ liệu huấn luyện của mô hình thiếu các trường hợp biến dạng mắt cá góc cực rộng.
  2. Nhóm lỗi `MISSING` (11 ca):
     - `E1_annotator_error`: Người gán nhãn bỏ sót các đối tượng bị che khuất một phần (occluded) ở khu vực mật độ cao zone mid (`adasind_230910.jpg R8, R10`).
     - `E4_model_domain`: Mô hình bỏ sót các đối tượng bị che khuất và xe ba bánh góc nghiêng (`LR_noM`).
- Cách sửa và ai nhận việc (`owner`):
  - `owner: annotator`: Thực hiện rework trên CVAT: xóa box thừa trong vùng thân xe ego và bổ sung polygon `ignore_region` `reason="ego_body"` (R07); xóa các box có H < 40px (R01); gộp người lái xe vào box Bike duy nhất (R03); bổ sung các box bị che khuất R8, R10.
  - `owner: ai_team`: Đánh dấu `keep_with_reason` đối với các lỗi domain của mô hình; thu thập thêm ảnh fisheye vùng mép để fine-tune mô hình cho bài toán SVM/360.
  - `owner: guideline`: Cập nhật ví dụ trực quan về nhận diện thân xe ego và chuẩn hóa phân loại xe van/xe tải nhẹ (R04).
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule):
  - Dòng findings: `r2_qa L4`, `r3_diag L1, L5, L9, L10` và các luật: R01 (ngưỡng chiều cao), R03 (luật Rider), R04 (class mapping), R07 (`ego_body`), R09 (không box trong ignore).
  - Ảnh minh chứng trong thư mục `submission/screenshots/`:
    + `submission/screenshots/ego_body_violation_adasind_128310.jpg`: minh chứng box Bike vẽ đè lên thân xe ego.
    + `submission/screenshots/escalation_ticket1_adasind_230910.jpg`: minh chứng ca xung đột phân loại xe tải nhẹ vs xe van.
    + `submission/screenshots/rider_bike_adasind_140160.jpg`: minh chứng các box vi phạm ngưỡng chiều cao H=40.
