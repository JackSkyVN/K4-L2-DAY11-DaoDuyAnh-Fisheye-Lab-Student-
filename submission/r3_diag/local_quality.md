# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `d7a1a892d0d50f8e6a8391a07365964a5b4d7bf60b96bc5ee8e7ed1afa091774`; slice `B3-mid`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_128310.jpg, adasind_140160.jpg, adasind_230910.jpg. Frame thiếu trong export: không.
TP=16; FP=6; FN=4; số lần đối chiếu=23; mean IoU của TP=0.861.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.696 | 0.928 | 0.870 |
| precision | 0.727 | 0.668 | 0.000 |
| recall | 0.800 | 0.708 | 0.000 |
| jaccard | 0.615 | 0.569 | 0.000 |
| dice | 0.762 | 0.663 | 0.000 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 2 | 3 | 0 | 0.870 | 0.400 | 1.000 | 0.400 | 0.571 |
| Bus | 0 | 1 | 0 | 0.957 | 0.000 | 0.000 | 0.000 | 0.000 |
| Car | 3 | 0 | 1 | 0.957 | 1.000 | 0.750 | 0.750 | 0.857 |
| Pedestrian | 6 | 1 | 2 | 0.870 | 0.857 | 0.750 | 0.667 | 0.800 |
| ThreeWheeler | 2 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Truck | 3 | 1 | 1 | 0.913 | 0.750 | 0.750 | 0.600 | 0.750 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_128310.jpg | 4 | 1 | 1 | 0.800 | 0.800 | 0.800 |
| adasind_140160.jpg | 3 | 1 | 0 | 0.750 | 0.750 | 1.000 |
| adasind_230910.jpg | 9 | 4 | 3 | 0.643 | 0.692 | 0.750 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Bus | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|---:|
| Bike | 2 | 0 | 0 | 0 | 0 | 0 | 0 |
| Bus | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Car | 0 | 0 | 3 | 0 | 0 | 1 | 0 |
| Pedestrian | 1 | 0 | 0 | 6 | 0 | 0 | 1 |
| ThreeWheeler | 0 | 0 | 0 | 0 | 2 | 0 | 0 |
| Truck | 0 | 1 | 0 | 0 | 0 | 3 | 0 |
| <extra> | 2 | 0 | 0 | 1 | 0 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
