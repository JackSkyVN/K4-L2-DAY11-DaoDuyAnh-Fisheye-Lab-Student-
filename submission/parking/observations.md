# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh):
  - Vạch 1 (đại diện khu tiền cảnh-trái): Vạch xiên ngắn từ (55, 543) → (48, 569), chia ranh giới giữa hai ô đỗ ở khu vực gần trung tâm-trái của ảnh, hướng gần thẳng đứng.
  - Vạch 2 (đại diện khu tiền cảnh-phải): Vạch xiên dài từ (696, 622) → (960, 685), phân chia hai ô đỗ ở vùng phải ảnh — vạch rõ nhất và dài nhất trong nhóm tiền cảnh.
  - Tổng cộng 24 polyline `parking_line` được vẽ, phân bố toàn bộ khu vực ô đỗ từ trái sang phải (x: 16–960) và từ khoảng giữa xuống tiền cảnh (y: 487–720).

- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao:
  - Đường xám ở hậu cảnh phía trên (nơi có xe tải đỏ đậu, khoảng y < 460): có các vạch trắng theo chiều ngang nhưng **không vẽ** vì đây là vạch phân làn của đường công cộng bên ngoài bãi đỗ, không phải ranh giới ô đỗ xe.
  - Mép ngoài cùng bên trái và phải bãi đỗ (curb/biên đường): **không vẽ** vì là biên của bãi, không phân chia ô đỗ riêng lẻ nào.

- Polygon `free_space` dừng ở đâu; có phần bị che nào không:
  - **free_space #1** (polygon dải giữa): Bao phần lối xe chạy giữa hai hàng ô đỗ, từ cạnh trái (x≈1) đến cạnh phải (x≈960), theo dải ngang y≈523–685. Phía trên dừng ở ranh giới các vạch ô đỗ hàng giữa; phía dưới dừng tại vùng ô đỗ tiền cảnh (y≈679 bên trái). Bãi trống, không có vật che.
  - **free_space #2** (polygon dải trên): Bao phần lối xe chạy hẹp hơn ở phía trên, dọc theo hàng vạch phía xa (y≈498–522), từ cạnh trái đến cạnh phải ảnh. Đây là lối xe chạy giữa hàng ô đỗ trên và ranh giới đường công cộng.

- Ca chưa chắc cần hỏi người soát:
  - Dải sơn trắng ở góc trên-trái (x≈0–30, y≈480–500): chưa rõ đây là mép biên bãi đỗ (không vẽ) hay ranh giới ô đỗ đầu dãy (nên vẽ). Đã chọn không vẽ vì không thấy rõ cấu trúc ô đỗ ở vị trí này.
