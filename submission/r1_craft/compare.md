# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_123090.jpg
- L2 mid SPURIOUS
## adasind_128310.jpg
- L5+R4 center WRONG_CLASS
## adasind_199770.jpg
- L3 edge IGNORE_SCOPE
- L9 mid IGNORE_SCOPE
- L2 mid SPURIOUS
- L4 center SPURIOUS
- L6 center SPURIOUS
- R3 mid MISSING
- R4 mid MISSING
- R5 edge MISSING
- R6 edge MISSING

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 6 | 5 | 1 | 3 |
| mid | 6 | 4 | 2 | 2 |
| edge | 5 | 3 | 2 | 0 |
