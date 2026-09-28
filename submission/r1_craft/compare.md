# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_128310.jpg
- L4 edge IGNORE_SCOPE
- L5+R3 mid WRONG_CLASS
## adasind_140160.jpg
- L1 edge SPURIOUS
## adasind_230910.jpg
- L11 edge IGNORE_SCOPE
- L12+R1 mid ATTRIBUTE
- L7+R7 mid WRONG_CLASS
- L9+R8 mid BOX_GEOMETRY
- L10 mid SPURIOUS
- L13 edge SPURIOUS
- R10 center MISSING

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 8 | 7 | 1 | 0 |
| mid | 9 | 6 | 3 | 4 |
| edge | 3 | 3 | 0 | 2 |
