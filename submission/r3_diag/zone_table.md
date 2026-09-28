# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 8 | 1 | 0 | 3 | 3 | MISSING (1) |
| mid | 9 | 3 | 4 | 4 | 3 | WRONG_CLASS (2) |
| edge | 3 | 0 | 2 | 1 | 2 | SPURIOUS (2) |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: Cả người gán nhãn (L) và mô hình (M) đều gãy nhiều nhất ở **zone mid** (L có 3 missing, 4 spurious với lỗi chính là WRONG_CLASS; M có 4 missing từ LR_noM + R_only và 3 thừa từ LM_noR + M_only). Ở zone edge, L gãy 2 spurious do vẽ nhầm box lên thân xe ego. Ở zone center, M bỏ sót 3 box và dự đoán thừa 3 box.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame:
  1. Zone mid có mật độ đối tượng cao, nhiều phương tiện chen chúc và che khuất lẫn nhau (occlusion), dẫn tới người gán nhãn dễ nhầm lẫn class (Truck vs Car vs Bus, Rider vs Bike).
  2. Zone edge chịu độ méo quang học mắt cá cực đại; nếu thiếu polygon `ego_body` người gán nhãn rất dễ nhầm thân xe/gương xe của chính mình thành vật thể giao thông (L1, L4 Bike). Model cũng mất khả năng phát hiện ở rìa mép do hiệu ứng giãn hình và thiếu box huấn luyện trong miền méo cao.
  3. Giới hạn: Slice chỉ gồm 3 frame (20 đối tượng reference), kích thước mẫu nhỏ nên một vài ca đơn lẻ có thể làm sai lệch tỷ lệ phần trăm; cần mở rộng ra tập gold set đa camera đa điều kiện ánh sáng để có đánh giá đại diện hơn.
