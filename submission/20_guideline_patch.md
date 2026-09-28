# Guideline patch

- **Rule mới đề xuất:** Bổ sung quy định R04b — Phân định phương tiện lai giữa xe bán tải / xe van chở người và xe tải nhẹ chở hàng (Pickup / Panel Van):
  1. Nếu phương tiện có kết cấu cabin tách rời thùng chở hàng phía sau (dù có hoặc không có mui bạt) hoặc panel van kín không có cửa sổ kính khoang sau dùng chuyên chở hàng hóa → Gán nhãn `Truck`.
  2. Nếu phương tiện có thân xe liền khối (monocoque), có cửa sổ kính suốt thân xe phục vụ chở người (từ 9 chỗ ngồi trở xuống) hoặc xe SUV/crossover gia đình → Gán nhãn `Car`.
  3. Trường hợp bị che khuất phần đuôi hoặc không xác định rõ: dựa vào chiều cao gầm và tỷ lệ khoang đầu xe để đối chiếu catalogue phương tiện chuẩn.
- **Áp dụng cho:** Class `Car` và `Truck` trên toàn bộ 4 camera và mọi zone (đặc biệt là zone mid và edge nơi xe thường bị nghiêng góc).
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Luật R04 hiện tại chỉ nêu ngắn gọn "van chở người → Car; pickup/xe tải nhỏ → Truck", nhưng trong thực tế giao thông Ấn Độ (ADASIND) có rất nhiều mẫu xe mini-truck và van đa dụng (như Tata Ace, Mahindra Bolero Maxi Truck) có hình thái hỗn hợp, dẫn tới ca xung đột nhãn giữa annotator (L), reference (R) và model (M) tại frame `adasind_230910.jpg` (`L7+M9` gán Truck trong khi `R7` gán Car).
- **`rules_version` mới:** v1.1.0
- **Hiệu lực từ:** Round `rework` của Day 11 và áp dụng chính thức cho các đợt gắn nhãn mở rộng tiếp theo.
