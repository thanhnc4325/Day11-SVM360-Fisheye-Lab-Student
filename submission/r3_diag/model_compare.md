# L / R / M

L/R/M là thứ tự box in-scope theo XML của từng frame.

## adasind_123090.jpg
- L4+R3+M4: LRM (mid)
- L3+R2+M2: LRM (edge)
- L1+R1+M1: LRM (center)
- L2: L_only (mid)
- M3: M_only (mid)
- M6: M_only (mid)
## adasind_128310.jpg
- L2+R2+M1: LRM (edge)
- L3+R3+M3: LRM (mid)
- L4+R1+M2: LRM (center)
- L1+R5: LR_noM (mid)
- L5: L_only (center)
- R4+M6: RM_noL (center)
- M4: M_only (center)
## adasind_199770.jpg
- L7+R1+M1: LRM (center)
- L1+R7: LR_noM (mid)
- L10+R2: LR_noM (edge)
- L5+R8: LR_noM (center)
- L8+R9+M3: LRM (center)
- L2+M9: LM_noR (mid)
- L4: L_only (center)
- L6: L_only (center)
- R3+M10: RM_noL (mid)
- R4: R_only (mid)
- R5: R_only (edge)
- R6: R_only (edge)
- M4: M_only (edge)
- M5: M_only (edge)
- M6: M_only (mid)
- M8: M_only (edge)
- M11: M_only (center)
- M12: M_only (center)
- M13: M_only (center)

## Zone × cell
| zone | LRM | LR_noM | LM_noR | L_only | RM_noL | R_only | M_only |
|---|---|---|---|---|---|---|---|
| center | 4 | 1 | 0 | 3 | 1 | 0 | 4 |
| mid | 2 | 2 | 1 | 1 | 1 | 1 | 3 |
| edge | 2 | 1 | 0 | 0 | 0 | 2 | 3 |
