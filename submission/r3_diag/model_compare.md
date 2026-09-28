# L / R / M

L/R/M là thứ tự box in-scope theo XML của từng frame.

## adasind_128310.jpg
- L1+R1+M2: LRM (center)
- L2+R2+M1: LRM (edge)
- L3+R5: LR_noM (mid)
- L6+R4+M6: LRM (center)
- L5: L_only (mid)
- R3+M3: RM_noL (mid)
- M4: M_only (center)
## adasind_140160.jpg
- L3+R1: LR_noM (edge)
- L4+R3: LR_noM (center)
- L2+R2+M4: LRM (mid)
- L1: L_only (edge)
- M1: M_only (edge)
- M2: M_only (edge)
- M3: M_only (center)
- M5: M_only (center)
- M6: M_only (mid)
## adasind_230910.jpg
- L5+R6+M6: LRM (edge)
- L1+R12+M10: LRM (mid)
- L4+R4+M7: LRM (center)
- L6+R5: LR_noM (mid)
- L14+R2+M4: LRM (mid)
- L3+R9+M5: LRM (center)
- L12+R1+M1: LRM (mid)
- L8+R3+M3: LRM (center)
- L2+R11: LR_noM (center)
- L7+M9: LM_noR (mid)
- L9: L_only (mid)
- L10: L_only (mid)
- L13: L_only (edge)
- R7: R_only (mid)
- R8: R_only (mid)
- R10: R_only (center)
- M8: M_only (mid)

## Zone × cell
| zone | LRM | LR_noM | LM_noR | L_only | RM_noL | R_only | M_only |
|---|---|---|---|---|---|---|---|
| center | 5 | 2 | 0 | 0 | 0 | 1 | 3 |
| mid | 4 | 2 | 1 | 3 | 1 | 2 | 2 |
| edge | 2 | 1 | 0 | 2 | 0 | 0 | 2 |
