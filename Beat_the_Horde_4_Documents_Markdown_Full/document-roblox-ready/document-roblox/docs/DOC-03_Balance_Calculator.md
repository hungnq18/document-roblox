# DOC-03 — Beat the Horde — Full Game Balance Calculator (Markdown Export)

> Nguồn: Google Sheets DOC-03, snapshot 27/10/2026.
> Xuất **đầy đủ mọi worksheet** theo thứ tự; công thức gốc Excel được giữ dưới dạng `=...`, giá trị tính sẵn được ghi sau `⇒` khi có. Markdown là bản tham khảo tĩnh, không tự tính lại công thức.

**Tổng số sheet:** 23

## Mục lục

1. [00_HuongDan](#00-huongdan)
2. [01_ThongSo](#01-thongso)
3. [02_EnemyScaling](#02-enemyscaling)
4. [03_Weapons_DPS](#03-weapons-dps)
5. [04_XP_Level](#04-xp-level)
6. [05_LuckRarity](#05-luckrarity)
7. [06_DropXP](#06-dropxp)
8. [07_RunSimulator](#07-runsimulator)
9. [08_QuyetDinh](#08-quyetdinh)
10. [09_GBD_Overview](#09-gbd-overview)
11. [10_Combat_Model](#10-combat-model)
12. [11_Enemy_TTK](#11-enemy-ttk)
13. [12_XP_Approved_History](#12-xp-approved-history)
14. [13_Phase_Encounters](#13-phase-encounters)
15. [14_Economy_Guardrails](#14-economy-guardrails)
16. [15_Validation_Plan](#15-validation-plan)
17. [16_Secondary_Mastery](#16-secondary-mastery)
18. [17_E2E_Inputs](#17-e2e-inputs)
19. [18_E2E_Run12Min](#18-e2e-run12min)
20. [19_BLV2_ChangeRegister](#19-blv2-changeregister)
21. [20_BLV2_RuntimeTimeline](#20-blv2-runtimetimeline)
22. [21_BLV2_E2E_Acceptance](#21-blv2-e2e-acceptance)
23. [22_BLV2_E2E_Scenarios](#22-blv2-e2e-scenarios)

---

## 00_HuongDan

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 |
|---|---|---|
| 1 | BEAT THE HORDE — BALANCE DESIGN / CALCULATOR v1.0 | Công thức / hướng dẫn |
| 2 | GDD/GSD-aligned. Tabs 09–15: baseline, scenario comparison, test plan. Tabs 01–08: working calculators; assumptions must not be treated as Approved. | DOC-01 GDD + Math & Balance V3.1 |
| 3 | GDD | https://docs.google.com/document/d/1Hn6hznuTHmY0q2g77ptnrUcs-F0YEHUAWHyY_0cxUpo/edit |
| 4 | APPROVAL 25/10/2026: Solo design proposals accepted; Co-op deferred. Numerical TBDs remain for balance/playtest. | (Base+Flat)*(1+Buff)*CritMultiplier*100/(100+Armor) |
| 5 | HP enemy | HPbase*(1+0.15*t)^1.3 |
| 6 | Damage enemy | Damagebase*(1+0.08*t) |
| 7 | XP per level | L<=10:60*L; L<=25:600+150*(L-10); L>=26:3200*1.12^(L-25) |
| 8 | DPS kỳ vọng | Damage/hit*(1+CritChance*(CritMultiplier-1))/Cooldown |
| 9 | Chú ý | t tính bằng phút. SOURCE = từ GDD; ASSUMPTION = giả định; OPEN = chưa duyệt. |

---

## 01_ThongSo

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 |
|---|---|---|---|
| 1 | Tham số | Giá trị | Nguồn |
| 2 | HP growth | 0.15 | SOURCE |
| 3 | HP exponent | 1.3 | SOURCE |
| 4 | Damage growth | 0.08 | SOURCE |
| 5 | Crit multiplier | 1.5 | ASSUMPTION - chưa duyệt |
| 6 | Flat bonus | 0.0 | ASSUMPTION |
| 7 | Buff percent | 0.0 | ASSUMPTION |
| 8 | Armor target | 0.0 | ASSUMPTION |
| 9 | Phase duration seconds | 360.0 | SOURCE |
| 10 | Survival phases (Phase 1 & Phase 2 only) | 2.0 | SOURCE: 2 timed phases; Phase 3 = Map Boss (no survival timer) |
| 11 | Green XP | 5.0 | SOURCE |
| 12 | Purple XP | 20.0 | SOURCE |
| 13 | Red XP | 100.0 | SOURCE |
| 14 | Secondary cap | 99.0 | APPROVED 25/10/2026 — Secondary cap 99; Stainless Steel Pot overflow TBD |

---

## 02_EnemyScaling

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 | Col 7 | Col 8 |
|---|---|---|---|---|---|---|---|---|
| 1 | Enemy | Base HP | Base damage | t phút | HP(t) | Damage(t) | HP factor | Damage factor |
| 2 | Normal | 20.0 | 5.0 | 0.0 | =B2*(1+'01_ThongSo'!$B$2*D2)^'01_ThongSo'!$B$3 ⇒ 20 | =C2*(1+'01_ThongSo'!$B$4*D2) ⇒ 5 | =E2/B2 ⇒ 1 | =F2/C2 ⇒ 1 |
| 3 | Normal | 20.0 | 5.0 | 3.0 | =B3*(1+'01_ThongSo'!$B$2*D3)^'01_ThongSo'!$B$3 ⇒ 32.41965572 | =C3*(1+'01_ThongSo'!$B$4*D3) ⇒ 6.2 | =E3/B3 ⇒ 1.620982786 | =F3/C3 ⇒ 1.24 |
| 4 | Normal | 20.0 | 5.0 | 6.0 | =B4*(1+'01_ThongSo'!$B$2*D4)^'01_ThongSo'!$B$3 ⇒ 46.06909458 | =C4*(1+'01_ThongSo'!$B$4*D4) ⇒ 7.4 | =E4/B4 ⇒ 2.303454729 | =F4/C4 ⇒ 1.48 |
| 5 | Normal | 20.0 | 5.0 | 9.0 | =B5*(1+'01_ThongSo'!$B$2*D5)^'01_ThongSo'!$B$3 ⇒ 60.73208856 | =C5*(1+'01_ThongSo'!$B$4*D5) ⇒ 8.6 | =E5/B5 ⇒ 3.036604428 | =F5/C5 ⇒ 1.72 |
| 6 | Normal | 20.0 | 5.0 | 12.0 | =B6*(1+'01_ThongSo'!$B$2*D6)^'01_ThongSo'!$B$3 ⇒ 76.2667845 | =C6*(1+'01_ThongSo'!$B$4*D6) ⇒ 9.8 | =E6/B6 ⇒ 3.813339225 | =F6/C6 ⇒ 1.96 |
| 7 | Elite | 100.0 | 12.0 | 0.0 | =B7*(1+'01_ThongSo'!$B$2*D7)^'01_ThongSo'!$B$3 ⇒ 100 | =C7*(1+'01_ThongSo'!$B$4*D7) ⇒ 12 | =E7/B7 ⇒ 1 | =F7/C7 ⇒ 1 |
| 8 | Elite | 100.0 | 12.0 | 3.0 | =B8*(1+'01_ThongSo'!$B$2*D8)^'01_ThongSo'!$B$3 ⇒ 162.0982786 | =C8*(1+'01_ThongSo'!$B$4*D8) ⇒ 14.88 | =E8/B8 ⇒ 1.620982786 | =F8/C8 ⇒ 1.24 |
| 9 | Elite | 100.0 | 12.0 | 6.0 | =B9*(1+'01_ThongSo'!$B$2*D9)^'01_ThongSo'!$B$3 ⇒ 230.3454729 | =C9*(1+'01_ThongSo'!$B$4*D9) ⇒ 17.76 | =E9/B9 ⇒ 2.303454729 | =F9/C9 ⇒ 1.48 |
| 10 | Elite | 100.0 | 12.0 | 9.0 | =B10*(1+'01_ThongSo'!$B$2*D10)^'01_ThongSo'!$B$3 ⇒ 303.6604428 | =C10*(1+'01_ThongSo'!$B$4*D10) ⇒ 20.64 | =E10/B10 ⇒ 3.036604428 | =F10/C10 ⇒ 1.72 |
| 11 | Elite | 100.0 | 12.0 | 12.0 | =B11*(1+'01_ThongSo'!$B$2*D11)^'01_ThongSo'!$B$3 ⇒ 381.3339225 | =C11*(1+'01_ThongSo'!$B$4*D11) ⇒ 23.52 | =E11/B11 ⇒ 3.813339225 | =F11/C11 ⇒ 1.96 |
| 12 | Bats | 15.0 | 4.0 | 0.0 | =B12*(1+'01_ThongSo'!$B$2*D12)^'01_ThongSo'!$B$3 ⇒ 15 | =C12*(1+'01_ThongSo'!$B$4*D12) ⇒ 4 | =E12/B12 ⇒ 1 | =F12/C12 ⇒ 1 |
| 13 | Bats | 15.0 | 4.0 | 3.0 | =B13*(1+'01_ThongSo'!$B$2*D13)^'01_ThongSo'!$B$3 ⇒ 24.31474179 | =C13*(1+'01_ThongSo'!$B$4*D13) ⇒ 4.96 | =E13/B13 ⇒ 1.620982786 | =F13/C13 ⇒ 1.24 |
| 14 | Bats | 15.0 | 4.0 | 6.0 | =B14*(1+'01_ThongSo'!$B$2*D14)^'01_ThongSo'!$B$3 ⇒ 34.55182093 | =C14*(1+'01_ThongSo'!$B$4*D14) ⇒ 5.92 | =E14/B14 ⇒ 2.303454729 | =F14/C14 ⇒ 1.48 |
| 15 | Bats | 15.0 | 4.0 | 9.0 | =B15*(1+'01_ThongSo'!$B$2*D15)^'01_ThongSo'!$B$3 ⇒ 45.54906642 | =C15*(1+'01_ThongSo'!$B$4*D15) ⇒ 6.88 | =E15/B15 ⇒ 3.036604428 | =F15/C15 ⇒ 1.72 |
| 16 | Bats | 15.0 | 4.0 | 12.0 | =B16*(1+'01_ThongSo'!$B$2*D16)^'01_ThongSo'!$B$3 ⇒ 57.20008838 | =C16*(1+'01_ThongSo'!$B$4*D16) ⇒ 7.84 | =E16/B16 ⇒ 3.813339225 | =F16/C16 ⇒ 1.96 |

---

## 03_Weapons_DPS

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 | Col 7 | Col 8 | Col 9 | Col 10 | Col 11 | Col 12 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Weapon | Base damage | Cooldown s | Crit chance | Flat | Buff | Crit mult | Armor | Noncrit hit | Expected hit | Expected DPS | Hits to kill Normal 20HP |
| 2 | Sword | 20.0 | 0.8 | 0.05 | ='01_ThongSo'!$B$6 ⇒ 0 | ='01_ThongSo'!$B$7 ⇒ 0 | ='01_ThongSo'!$B$5 ⇒ 1.5 | ='01_ThongSo'!$B$8 ⇒ 0 | =(B2+E2)*(1+F2)*100/(100+H2) ⇒ 20 | =I2*(1+D2*(G2-1)) ⇒ 20.5 | =J2/C2 ⇒ 25.625 | =ROUNDUP(20/J2,0) ⇒ 1 |
| 3 | Bow | 14.0 | 0.5 | 0.1 | ='01_ThongSo'!$B$6 ⇒ 0 | ='01_ThongSo'!$B$7 ⇒ 0 | ='01_ThongSo'!$B$5 ⇒ 1.5 | ='01_ThongSo'!$B$8 ⇒ 0 | =(B3+E3)*(1+F3)*100/(100+H3) ⇒ 14 | =I3*(1+D3*(G3-1)) ⇒ 14.7 | =J3/C3 ⇒ 29.4 | =ROUNDUP(20/J3,0) ⇒ 2 |
| 4 | Staff | 26.0 | 1.3 | 0.03 | ='01_ThongSo'!$B$6 ⇒ 0 | ='01_ThongSo'!$B$7 ⇒ 0 | ='01_ThongSo'!$B$5 ⇒ 1.5 | ='01_ThongSo'!$B$8 ⇒ 0 | =(B4+E4)*(1+F4)*100/(100+H4) ⇒ 26 | =I4*(1+D4*(G4-1)) ⇒ 26.39 | =J4/C4 ⇒ 20.3 | =ROUNDUP(20/J4,0) ⇒ 1 |
| 5 | Energy Orbs | 8.0 | 0.2 | 0.0 | ='01_ThongSo'!$B$6 ⇒ 0 | ='01_ThongSo'!$B$7 ⇒ 0 | ='01_ThongSo'!$B$5 ⇒ 1.5 | ='01_ThongSo'!$B$8 ⇒ 0 | =(B5+E5)*(1+F5)*100/(100+H5) ⇒ 8 | =I5*(1+D5*(G5-1)) ⇒ 8 | =J5/C5 ⇒ 40 | =ROUNDUP(20/J5,0) ⇒ 3 |
| 6 | Daggers | 12.0 | 0.6 | 0.08 | ='01_ThongSo'!$B$6 ⇒ 0 | ='01_ThongSo'!$B$7 ⇒ 0 | ='01_ThongSo'!$B$5 ⇒ 1.5 | ='01_ThongSo'!$B$8 ⇒ 0 | =(B6+E6)*(1+F6)*100/(100+H6) ⇒ 12 | =I6*(1+D6*(G6-1)) ⇒ 12.48 | =J6/C6 ⇒ 20.8 | =ROUNDUP(20/J6,0) ⇒ 2 |

---

## 04_XP_Level

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 |
|---|---|---|---|---|---|---|
| 1 | Level | XP next level | Cumulative XP | Green orbs | Purple orbs | Red orbs |
| 2 | 1.0 | =IF(A2<=10,60*A2,IF(A2<=25,600+150*(A2-10),3200*(1+12%)^(A2-25))) ⇒ 60 | 0.0 | =ROUNDUP(B2/'01_ThongSo'!$B$11,0) ⇒ 12 | =ROUNDUP(B2/'01_ThongSo'!$B$12,0) ⇒ 3 | =ROUNDUP(B2/'01_ThongSo'!$B$13,0) ⇒ 1 |
| 3 | 2.0 | =IF(A3<=10,60*A3,IF(A3<=25,600+150*(A3-10),3200*(1+12%)^(A3-25))) ⇒ 120 | =C2+B2 ⇒ 60 | =ROUNDUP(B3/'01_ThongSo'!$B$11,0) ⇒ 24 | =ROUNDUP(B3/'01_ThongSo'!$B$12,0) ⇒ 6 | =ROUNDUP(B3/'01_ThongSo'!$B$13,0) ⇒ 2 |
| 4 | 3.0 | =IF(A4<=10,60*A4,IF(A4<=25,600+150*(A4-10),3200*(1+12%)^(A4-25))) ⇒ 180 | =C3+B3 ⇒ 180 | =ROUNDUP(B4/'01_ThongSo'!$B$11,0) ⇒ 36 | =ROUNDUP(B4/'01_ThongSo'!$B$12,0) ⇒ 9 | =ROUNDUP(B4/'01_ThongSo'!$B$13,0) ⇒ 2 |
| 5 | 4.0 | =IF(A5<=10,60*A5,IF(A5<=25,600+150*(A5-10),3200*(1+12%)^(A5-25))) ⇒ 240 | =C4+B4 ⇒ 360 | =ROUNDUP(B5/'01_ThongSo'!$B$11,0) ⇒ 48 | =ROUNDUP(B5/'01_ThongSo'!$B$12,0) ⇒ 12 | =ROUNDUP(B5/'01_ThongSo'!$B$13,0) ⇒ 3 |
| 6 | 5.0 | =IF(A6<=10,60*A6,IF(A6<=25,600+150*(A6-10),3200*(1+12%)^(A6-25))) ⇒ 300 | =C5+B5 ⇒ 600 | =ROUNDUP(B6/'01_ThongSo'!$B$11,0) ⇒ 60 | =ROUNDUP(B6/'01_ThongSo'!$B$12,0) ⇒ 15 | =ROUNDUP(B6/'01_ThongSo'!$B$13,0) ⇒ 3 |
| 7 | 6.0 | =IF(A7<=10,60*A7,IF(A7<=25,600+150*(A7-10),3200*(1+12%)^(A7-25))) ⇒ 360 | =C6+B6 ⇒ 900 | =ROUNDUP(B7/'01_ThongSo'!$B$11,0) ⇒ 72 | =ROUNDUP(B7/'01_ThongSo'!$B$12,0) ⇒ 18 | =ROUNDUP(B7/'01_ThongSo'!$B$13,0) ⇒ 4 |
| 8 | 7.0 | =IF(A8<=10,60*A8,IF(A8<=25,600+150*(A8-10),3200*(1+12%)^(A8-25))) ⇒ 420 | =C7+B7 ⇒ 1260 | =ROUNDUP(B8/'01_ThongSo'!$B$11,0) ⇒ 84 | =ROUNDUP(B8/'01_ThongSo'!$B$12,0) ⇒ 21 | =ROUNDUP(B8/'01_ThongSo'!$B$13,0) ⇒ 5 |
| 9 | 8.0 | =IF(A9<=10,60*A9,IF(A9<=25,600+150*(A9-10),3200*(1+12%)^(A9-25))) ⇒ 480 | =C8+B8 ⇒ 1680 | =ROUNDUP(B9/'01_ThongSo'!$B$11,0) ⇒ 96 | =ROUNDUP(B9/'01_ThongSo'!$B$12,0) ⇒ 24 | =ROUNDUP(B9/'01_ThongSo'!$B$13,0) ⇒ 5 |
| 10 | 9.0 | =IF(A10<=10,60*A10,IF(A10<=25,600+150*(A10-10),3200*(1+12%)^(A10-25))) ⇒ 540 | =C9+B9 ⇒ 2160 | =ROUNDUP(B10/'01_ThongSo'!$B$11,0) ⇒ 108 | =ROUNDUP(B10/'01_ThongSo'!$B$12,0) ⇒ 27 | =ROUNDUP(B10/'01_ThongSo'!$B$13,0) ⇒ 6 |
| 11 | 10.0 | =IF(A11<=10,60*A11,IF(A11<=25,600+150*(A11-10),3200*(1+12%)^(A11-25))) ⇒ 600 | =C10+B10 ⇒ 2700 | =ROUNDUP(B11/'01_ThongSo'!$B$11,0) ⇒ 120 | =ROUNDUP(B11/'01_ThongSo'!$B$12,0) ⇒ 30 | =ROUNDUP(B11/'01_ThongSo'!$B$13,0) ⇒ 6 |
| 12 | 11.0 | =IF(A12<=10,60*A12,IF(A12<=25,600+150*(A12-10),3200*(1+12%)^(A12-25))) ⇒ 750 | =C11+B11 ⇒ 3300 | =ROUNDUP(B12/'01_ThongSo'!$B$11,0) ⇒ 150 | =ROUNDUP(B12/'01_ThongSo'!$B$12,0) ⇒ 38 | =ROUNDUP(B12/'01_ThongSo'!$B$13,0) ⇒ 8 |
| 13 | 12.0 | =IF(A13<=10,60*A13,IF(A13<=25,600+150*(A13-10),3200*(1+12%)^(A13-25))) ⇒ 900 | =C12+B12 ⇒ 4050 | =ROUNDUP(B13/'01_ThongSo'!$B$11,0) ⇒ 180 | =ROUNDUP(B13/'01_ThongSo'!$B$12,0) ⇒ 45 | =ROUNDUP(B13/'01_ThongSo'!$B$13,0) ⇒ 9 |
| 14 | 13.0 | =IF(A14<=10,60*A14,IF(A14<=25,600+150*(A14-10),3200*(1+12%)^(A14-25))) ⇒ 1050 | =C13+B13 ⇒ 4950 | =ROUNDUP(B14/'01_ThongSo'!$B$11,0) ⇒ 210 | =ROUNDUP(B14/'01_ThongSo'!$B$12,0) ⇒ 53 | =ROUNDUP(B14/'01_ThongSo'!$B$13,0) ⇒ 11 |
| 15 | 14.0 | =IF(A15<=10,60*A15,IF(A15<=25,600+150*(A15-10),3200*(1+12%)^(A15-25))) ⇒ 1200 | =C14+B14 ⇒ 6000 | =ROUNDUP(B15/'01_ThongSo'!$B$11,0) ⇒ 240 | =ROUNDUP(B15/'01_ThongSo'!$B$12,0) ⇒ 60 | =ROUNDUP(B15/'01_ThongSo'!$B$13,0) ⇒ 12 |
| 16 | 15.0 | =IF(A16<=10,60*A16,IF(A16<=25,600+150*(A16-10),3200*(1+12%)^(A16-25))) ⇒ 1350 | =C15+B15 ⇒ 7200 | =ROUNDUP(B16/'01_ThongSo'!$B$11,0) ⇒ 270 | =ROUNDUP(B16/'01_ThongSo'!$B$12,0) ⇒ 68 | =ROUNDUP(B16/'01_ThongSo'!$B$13,0) ⇒ 14 |
| 17 | 16.0 | =IF(A17<=10,60*A17,IF(A17<=25,600+150*(A17-10),3200*(1+12%)^(A17-25))) ⇒ 1500 | =C16+B16 ⇒ 8550 | =ROUNDUP(B17/'01_ThongSo'!$B$11,0) ⇒ 300 | =ROUNDUP(B17/'01_ThongSo'!$B$12,0) ⇒ 75 | =ROUNDUP(B17/'01_ThongSo'!$B$13,0) ⇒ 15 |
| 18 | 17.0 | =IF(A18<=10,60*A18,IF(A18<=25,600+150*(A18-10),3200*(1+12%)^(A18-25))) ⇒ 1650 | =C17+B17 ⇒ 10050 | =ROUNDUP(B18/'01_ThongSo'!$B$11,0) ⇒ 330 | =ROUNDUP(B18/'01_ThongSo'!$B$12,0) ⇒ 83 | =ROUNDUP(B18/'01_ThongSo'!$B$13,0) ⇒ 17 |
| 19 | 18.0 | =IF(A19<=10,60*A19,IF(A19<=25,600+150*(A19-10),3200*(1+12%)^(A19-25))) ⇒ 1800 | =C18+B18 ⇒ 11700 | =ROUNDUP(B19/'01_ThongSo'!$B$11,0) ⇒ 360 | =ROUNDUP(B19/'01_ThongSo'!$B$12,0) ⇒ 90 | =ROUNDUP(B19/'01_ThongSo'!$B$13,0) ⇒ 18 |
| 20 | 19.0 | =IF(A20<=10,60*A20,IF(A20<=25,600+150*(A20-10),3200*(1+12%)^(A20-25))) ⇒ 1950 | =C19+B19 ⇒ 13500 | =ROUNDUP(B20/'01_ThongSo'!$B$11,0) ⇒ 390 | =ROUNDUP(B20/'01_ThongSo'!$B$12,0) ⇒ 98 | =ROUNDUP(B20/'01_ThongSo'!$B$13,0) ⇒ 20 |
| 21 | 20.0 | =IF(A21<=10,60*A21,IF(A21<=25,600+150*(A21-10),3200*(1+12%)^(A21-25))) ⇒ 2100 | =C20+B20 ⇒ 15450 | =ROUNDUP(B21/'01_ThongSo'!$B$11,0) ⇒ 420 | =ROUNDUP(B21/'01_ThongSo'!$B$12,0) ⇒ 105 | =ROUNDUP(B21/'01_ThongSo'!$B$13,0) ⇒ 21 |
| 22 | 21.0 | =IF(A22<=10,60*A22,IF(A22<=25,600+150*(A22-10),3200*(1+12%)^(A22-25))) ⇒ 2250 | =C21+B21 ⇒ 17550 | =ROUNDUP(B22/'01_ThongSo'!$B$11,0) ⇒ 450 | =ROUNDUP(B22/'01_ThongSo'!$B$12,0) ⇒ 113 | =ROUNDUP(B22/'01_ThongSo'!$B$13,0) ⇒ 23 |
| 23 | 22.0 | =IF(A23<=10,60*A23,IF(A23<=25,600+150*(A23-10),3200*(1+12%)^(A23-25))) ⇒ 2400 | =C22+B22 ⇒ 19800 | =ROUNDUP(B23/'01_ThongSo'!$B$11,0) ⇒ 480 | =ROUNDUP(B23/'01_ThongSo'!$B$12,0) ⇒ 120 | =ROUNDUP(B23/'01_ThongSo'!$B$13,0) ⇒ 24 |
| 24 | 23.0 | =IF(A24<=10,60*A24,IF(A24<=25,600+150*(A24-10),3200*(1+12%)^(A24-25))) ⇒ 2550 | =C23+B23 ⇒ 22200 | =ROUNDUP(B24/'01_ThongSo'!$B$11,0) ⇒ 510 | =ROUNDUP(B24/'01_ThongSo'!$B$12,0) ⇒ 128 | =ROUNDUP(B24/'01_ThongSo'!$B$13,0) ⇒ 26 |
| 25 | 24.0 | =IF(A25<=10,60*A25,IF(A25<=25,600+150*(A25-10),3200*(1+12%)^(A25-25))) ⇒ 2700 | =C24+B24 ⇒ 24750 | =ROUNDUP(B25/'01_ThongSo'!$B$11,0) ⇒ 540 | =ROUNDUP(B25/'01_ThongSo'!$B$12,0) ⇒ 135 | =ROUNDUP(B25/'01_ThongSo'!$B$13,0) ⇒ 27 |
| 26 | 25.0 | =IF(A26<=10,60*A26,IF(A26<=25,600+150*(A26-10),3200*(1+12%)^(A26-25))) ⇒ 2850 | =C25+B25 ⇒ 27450 | =ROUNDUP(B26/'01_ThongSo'!$B$11,0) ⇒ 570 | =ROUNDUP(B26/'01_ThongSo'!$B$12,0) ⇒ 143 | =ROUNDUP(B26/'01_ThongSo'!$B$13,0) ⇒ 29 |
| 27 | 26.0 | =IF(A27<=10,60*A27,IF(A27<=25,600+150*(A27-10),3200*(1+12%)^(A27-25))) ⇒ 3584 | =C26+B26 ⇒ 30300 | =ROUNDUP(B27/'01_ThongSo'!$B$11,0) ⇒ 717 | =ROUNDUP(B27/'01_ThongSo'!$B$12,0) ⇒ 180 | =ROUNDUP(B27/'01_ThongSo'!$B$13,0) ⇒ 36 |
| 28 | 27.0 | =IF(A28<=10,60*A28,IF(A28<=25,600+150*(A28-10),3200*(1+12%)^(A28-25))) ⇒ 4014.08 | =C27+B27 ⇒ 33884 | =ROUNDUP(B28/'01_ThongSo'!$B$11,0) ⇒ 803 | =ROUNDUP(B28/'01_ThongSo'!$B$12,0) ⇒ 201 | =ROUNDUP(B28/'01_ThongSo'!$B$13,0) ⇒ 41 |
| 29 | 28.0 | =IF(A29<=10,60*A29,IF(A29<=25,600+150*(A29-10),3200*(1+12%)^(A29-25))) ⇒ 4495.7696 | =C28+B28 ⇒ 37898.08 | =ROUNDUP(B29/'01_ThongSo'!$B$11,0) ⇒ 900 | =ROUNDUP(B29/'01_ThongSo'!$B$12,0) ⇒ 225 | =ROUNDUP(B29/'01_ThongSo'!$B$13,0) ⇒ 45 |
| 30 | 29.0 | =IF(A30<=10,60*A30,IF(A30<=25,600+150*(A30-10),3200*(1+12%)^(A30-25))) ⇒ 5035.261952 | =C29+B29 ⇒ 42393.8496 | =ROUNDUP(B30/'01_ThongSo'!$B$11,0) ⇒ 1008 | =ROUNDUP(B30/'01_ThongSo'!$B$12,0) ⇒ 252 | =ROUNDUP(B30/'01_ThongSo'!$B$13,0) ⇒ 51 |
| 31 | 30.0 | =IF(A31<=10,60*A31,IF(A31<=25,600+150*(A31-10),3200*(1+12%)^(A31-25))) ⇒ 5639.493386 | =C30+B30 ⇒ 47429.11155 | =ROUNDUP(B31/'01_ThongSo'!$B$11,0) ⇒ 1128 | =ROUNDUP(B31/'01_ThongSo'!$B$12,0) ⇒ 282 | =ROUNDUP(B31/'01_ThongSo'!$B$13,0) ⇒ 57 |
| 32 | 31.0 | =IF(A32<=10,60*A32,IF(A32<=25,600+150*(A32-10),3200*(1+12%)^(A32-25))) ⇒ 6316.232593 | =C31+B31 ⇒ 53068.60494 | =ROUNDUP(B32/'01_ThongSo'!$B$11,0) ⇒ 1264 | =ROUNDUP(B32/'01_ThongSo'!$B$12,0) ⇒ 316 | =ROUNDUP(B32/'01_ThongSo'!$B$13,0) ⇒ 64 |
| 33 | 32.0 | =IF(A33<=10,60*A33,IF(A33<=25,600+150*(A33-10),3200*(1+12%)^(A33-25))) ⇒ 7074.180504 | =C32+B32 ⇒ 59384.83753 | =ROUNDUP(B33/'01_ThongSo'!$B$11,0) ⇒ 1415 | =ROUNDUP(B33/'01_ThongSo'!$B$12,0) ⇒ 354 | =ROUNDUP(B33/'01_ThongSo'!$B$13,0) ⇒ 71 |
| 34 | 33.0 | =IF(A34<=10,60*A34,IF(A34<=25,600+150*(A34-10),3200*(1+12%)^(A34-25))) ⇒ 7923.082164 | =C33+B33 ⇒ 66459.01803 | =ROUNDUP(B34/'01_ThongSo'!$B$11,0) ⇒ 1585 | =ROUNDUP(B34/'01_ThongSo'!$B$12,0) ⇒ 397 | =ROUNDUP(B34/'01_ThongSo'!$B$13,0) ⇒ 80 |
| 35 | 34.0 | =IF(A35<=10,60*A35,IF(A35<=25,600+150*(A35-10),3200*(1+12%)^(A35-25))) ⇒ 8873.852024 | =C34+B34 ⇒ 74382.1002 | =ROUNDUP(B35/'01_ThongSo'!$B$11,0) ⇒ 1775 | =ROUNDUP(B35/'01_ThongSo'!$B$12,0) ⇒ 444 | =ROUNDUP(B35/'01_ThongSo'!$B$13,0) ⇒ 89 |
| 36 | 35.0 | =IF(A36<=10,60*A36,IF(A36<=25,600+150*(A36-10),3200*(1+12%)^(A36-25))) ⇒ 9938.714267 | =C35+B35 ⇒ 83255.95222 | =ROUNDUP(B36/'01_ThongSo'!$B$11,0) ⇒ 1988 | =ROUNDUP(B36/'01_ThongSo'!$B$12,0) ⇒ 497 | =ROUNDUP(B36/'01_ThongSo'!$B$13,0) ⇒ 100 |
| 37 | 36.0 | =IF(A37<=10,60*A37,IF(A37<=25,600+150*(A37-10),3200*(1+12%)^(A37-25))) ⇒ 11131.35998 | =C36+B36 ⇒ 93194.66649 | =ROUNDUP(B37/'01_ThongSo'!$B$11,0) ⇒ 2227 | =ROUNDUP(B37/'01_ThongSo'!$B$12,0) ⇒ 557 | =ROUNDUP(B37/'01_ThongSo'!$B$13,0) ⇒ 112 |
| 38 | 37.0 | =IF(A38<=10,60*A38,IF(A38<=25,600+150*(A38-10),3200*(1+12%)^(A38-25))) ⇒ 12467.12318 | =C37+B37 ⇒ 104326.0265 | =ROUNDUP(B38/'01_ThongSo'!$B$11,0) ⇒ 2494 | =ROUNDUP(B38/'01_ThongSo'!$B$12,0) ⇒ 624 | =ROUNDUP(B38/'01_ThongSo'!$B$13,0) ⇒ 125 |
| 39 | 38.0 | =IF(A39<=10,60*A39,IF(A39<=25,600+150*(A39-10),3200*(1+12%)^(A39-25))) ⇒ 13963.17796 | =C38+B38 ⇒ 116793.1496 | =ROUNDUP(B39/'01_ThongSo'!$B$11,0) ⇒ 2793 | =ROUNDUP(B39/'01_ThongSo'!$B$12,0) ⇒ 699 | =ROUNDUP(B39/'01_ThongSo'!$B$13,0) ⇒ 140 |
| 40 | 39.0 | =IF(A40<=10,60*A40,IF(A40<=25,600+150*(A40-10),3200*(1+12%)^(A40-25))) ⇒ 15638.75931 | =C39+B39 ⇒ 130756.3276 | =ROUNDUP(B40/'01_ThongSo'!$B$11,0) ⇒ 3128 | =ROUNDUP(B40/'01_ThongSo'!$B$12,0) ⇒ 782 | =ROUNDUP(B40/'01_ThongSo'!$B$13,0) ⇒ 157 |
| 41 | 40.0 | =IF(A41<=10,60*A41,IF(A41<=25,600+150*(A41-10),3200*(1+12%)^(A41-25))) ⇒ 17515.41043 | =C40+B40 ⇒ 146395.0869 | =ROUNDUP(B41/'01_ThongSo'!$B$11,0) ⇒ 3504 | =ROUNDUP(B41/'01_ThongSo'!$B$12,0) ⇒ 876 | =ROUNDUP(B41/'01_ThongSo'!$B$13,0) ⇒ 176 |
| 42 | 41.0 | =IF(A42<=10,60*A42,IF(A42<=25,600+150*(A42-10),3200*(1+12%)^(A42-25))) ⇒ 19617.25968 | =C41+B41 ⇒ 163910.4973 | =ROUNDUP(B42/'01_ThongSo'!$B$11,0) ⇒ 3924 | =ROUNDUP(B42/'01_ThongSo'!$B$12,0) ⇒ 981 | =ROUNDUP(B42/'01_ThongSo'!$B$13,0) ⇒ 197 |
| 43 | 42.0 | =IF(A43<=10,60*A43,IF(A43<=25,600+150*(A43-10),3200*(1+12%)^(A43-25))) ⇒ 21971.33084 | =C42+B42 ⇒ 183527.757 | =ROUNDUP(B43/'01_ThongSo'!$B$11,0) ⇒ 4395 | =ROUNDUP(B43/'01_ThongSo'!$B$12,0) ⇒ 1099 | =ROUNDUP(B43/'01_ThongSo'!$B$13,0) ⇒ 220 |
| 44 | 43.0 | =IF(A44<=10,60*A44,IF(A44<=25,600+150*(A44-10),3200*(1+12%)^(A44-25))) ⇒ 24607.89054 | =C43+B43 ⇒ 205499.0879 | =ROUNDUP(B44/'01_ThongSo'!$B$11,0) ⇒ 4922 | =ROUNDUP(B44/'01_ThongSo'!$B$12,0) ⇒ 1231 | =ROUNDUP(B44/'01_ThongSo'!$B$13,0) ⇒ 247 |
| 45 | 44.0 | =IF(A45<=10,60*A45,IF(A45<=25,600+150*(A45-10),3200*(1+12%)^(A45-25))) ⇒ 27560.83741 | =C44+B44 ⇒ 230106.9784 | =ROUNDUP(B45/'01_ThongSo'!$B$11,0) ⇒ 5513 | =ROUNDUP(B45/'01_ThongSo'!$B$12,0) ⇒ 1379 | =ROUNDUP(B45/'01_ThongSo'!$B$13,0) ⇒ 276 |
| 46 | 45.0 | =IF(A46<=10,60*A46,IF(A46<=25,600+150*(A46-10),3200*(1+12%)^(A46-25))) ⇒ 30868.1379 | =C45+B45 ⇒ 257667.8158 | =ROUNDUP(B46/'01_ThongSo'!$B$11,0) ⇒ 6174 | =ROUNDUP(B46/'01_ThongSo'!$B$12,0) ⇒ 1544 | =ROUNDUP(B46/'01_ThongSo'!$B$13,0) ⇒ 309 |
| 47 | 46.0 | =IF(A47<=10,60*A47,IF(A47<=25,600+150*(A47-10),3200*(1+12%)^(A47-25))) ⇒ 34572.31445 | =C46+B46 ⇒ 288535.9537 | =ROUNDUP(B47/'01_ThongSo'!$B$11,0) ⇒ 6915 | =ROUNDUP(B47/'01_ThongSo'!$B$12,0) ⇒ 1729 | =ROUNDUP(B47/'01_ThongSo'!$B$13,0) ⇒ 346 |
| 48 | 47.0 | =IF(A48<=10,60*A48,IF(A48<=25,600+150*(A48-10),3200*(1+12%)^(A48-25))) ⇒ 38720.99218 | =C47+B47 ⇒ 323108.2682 | =ROUNDUP(B48/'01_ThongSo'!$B$11,0) ⇒ 7745 | =ROUNDUP(B48/'01_ThongSo'!$B$12,0) ⇒ 1937 | =ROUNDUP(B48/'01_ThongSo'!$B$13,0) ⇒ 388 |
| 49 | 48.0 | =IF(A49<=10,60*A49,IF(A49<=25,600+150*(A49-10),3200*(1+12%)^(A49-25))) ⇒ 43367.51124 | =C48+B48 ⇒ 361829.2603 | =ROUNDUP(B49/'01_ThongSo'!$B$11,0) ⇒ 8674 | =ROUNDUP(B49/'01_ThongSo'!$B$12,0) ⇒ 2169 | =ROUNDUP(B49/'01_ThongSo'!$B$13,0) ⇒ 434 |
| 50 | 49.0 | =IF(A50<=10,60*A50,IF(A50<=25,600+150*(A50-10),3200*(1+12%)^(A50-25))) ⇒ 48571.61259 | =C49+B49 ⇒ 405196.7716 | =ROUNDUP(B50/'01_ThongSo'!$B$11,0) ⇒ 9715 | =ROUNDUP(B50/'01_ThongSo'!$B$12,0) ⇒ 2429 | =ROUNDUP(B50/'01_ThongSo'!$B$13,0) ⇒ 486 |
| 51 | 50.0 | =IF(A51<=10,60*A51,IF(A51<=25,600+150*(A51-10),3200*(1+12%)^(A51-25))) ⇒ 54400.2061 | =C50+B50 ⇒ 453768.3842 | =ROUNDUP(B51/'01_ThongSo'!$B$11,0) ⇒ 10881 | =ROUNDUP(B51/'01_ThongSo'!$B$12,0) ⇒ 2721 | =ROUNDUP(B51/'01_ThongSo'!$B$13,0) ⇒ 545 |

---

## 05_LuckRarity

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 | Col 7 |
|---|---|---|---|---|---|---|---|
| 1 | Luck | Common | Uncommon | Rare | Epic | Legendary | Tổng |
| 2 | 0.0 | 0.6 | 0.25 | 0.11 | 0.035 | 0.005 | =SUM(B2:F2) ⇒ 1 |
| 3 | 0.2 | 0.45 | 0.3 | 0.16 | 0.075 | 0.015 | =SUM(B3:F3) ⇒ 1 |
| 4 | 0.5 | 0.3 | 0.32 | 0.22 | 0.12 | 0.04 | =SUM(B4:F4) ⇒ 1 |
| 5 | 1.0 | 0.15 | 0.3 | 0.3 | 0.18 | 0.07 | =SUM(B5:F5) ⇒ 1 |

---

## 06_DropXP

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 | Col 7 |
|---|---|---|---|---|---|---|---|
| 1 | Enemy | Green % | Purple % | Red % | Total | Expected XP | Trạng thái |
| 2 | Normal | 0.8 | 0.18 | 0.02 | =SUM(B2:D2) ⇒ 1 | =B2*'01_ThongSo'!B11+C2*'01_ThongSo'!B12+D2*'01_ThongSo'!B13 ⇒ 9.6 | SOURCE: Normal 80% green / 18% purple / 2% red |
| 3 | Elite | 0.6 | 0.3 | 0.1 | =SUM(B3:D3) ⇒ 1 | =B3*'01_ThongSo'!B11+C3*'01_ThongSo'!B12+D3*'01_ThongSo'!B13 ⇒ 19 | APPROVED: 60% Green / 30% Purple / 10% Red |
| 4 | Boss | 0.0 | 0.6 | 0.4 | =SUM(B4:D4) ⇒ 1 | =B4*'01_ThongSo'!B11+C4*'01_ThongSo'!B12+D4*'01_ThongSo'!B13 ⇒ 52 | APPROVED: 60% Purple / 40% Red |

---

## 07_RunSimulator

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 |
|---|---|---|---|
| 1 | Tham số | Giá trị | Giải thích |
| 2 | Timed survival phases | ='01_ThongSo'!B10 ⇒ 2 | 2 timed survival phases only; Phase 3 = untimed Map Boss |
| 3 | Seconds per phase | ='01_ThongSo'!B9 ⇒ 360 | SOURCE |
| 4 | Base survival seconds (excludes Phase 3 Boss) | =B2*B3 ⇒ 720 | 720s excludes Boss fights, portals, transitions and Phase 3 |
| 5 | Base run minutes | =B4/60 ⇒ 12 |  |
| 6 | Player DPS | ='03_Weapons_DPS'!K2 ⇒ 25.625 | Sword default |
| 7 | Normal HP at minute 12 | ='02_EnemyScaling'!E6 ⇒ 76.2667845 | SOURCE scaling |
| 8 | Estimated TTK | =IFERROR(B7/B6,0) ⇒ 2.976264761 | Giả định đứng yên, 1 mục tiêu |
| 9 | Kills per minute | 20.0 | ASSUMPTION |
| 10 | XP per normal kill | ='01_ThongSo'!B11 ⇒ 5 | ASSUMPTION |
| 11 | XP per minute | =B9*B10 ⇒ 100 |  |
| 12 | XP during base run | =B11*B5 ⇒ 1200 |  |

---

## 08_QuyetDinh

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 |
|---|---|---|---|
| 1 | ID | Vấn đề | Trạng thái |
| 2 | P0-01 | Elite XP = 60% Green / 30% Purple / 10% Red | APPROVED |
| 3 | P0-02 | Boss XP = 60% Purple / 40% Red | APPROVED |
| 4 | P0-03 | Chest: hiển thị 3 phần thưởng, chọn 1; Banish/Skip TBD | APPROVED |
| 5 | P0-04 | Map Boss/portal sau Phase 2 | PARTLY RESOLVED: Phase 3 Map Boss confirmed; chest/portal gating TBD |
| 6 | P0-05 | Stainless Steel Pot: tương tác với Secondary cap 99 chưa chốt | TBD: Stainless Steel Pot above approved cap 99; default Evolution at 99 approved |
| 7 | P0-06 | Divine Anvil permanent hay in-run | APPROVED SOLO BASELINE; EFFECT DURATION/STACK TBD |
| 8 | P0-07 | Co-op level-up pause | DEFERRED — CO-OP PHASE |
| 9 | P0-08 | Co-op XP/Gold/Chest ownership | DEFERRED — CO-OP PHASE |
| 10 | P0-09 | Boss timeline 02:00, 04:30, 06:00; Phase 3 Map Boss (đã chốt) | APPROVED: BOSS TIMELINE; OTHER END-RUN DETAILS TBD |
| 11 | P0-10 | Boss timeline, timer, end Run | CHƯA DUYỆT |

---

## 09_GBD_Overview

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 |
|---|---|---|---|
| 1 | RULE | DESIGN BASELINE | STATUS |
| 2 | Run | Phase1 360s; Phase2 360s; Phase3 Map Boss without survival timer | SOURCE |
| 3 | Mini-Boss | 02:00 and 04:30 for each survival phase | SOURCE |
| 4 | Phase Boss | 06:00 in Phase1/2 | SOURCE |
| 5 | Build | Carry 100% between phases (buffs expire naturally) | SOURCE |
| 6 | Primary | Permanent Level at Lobby; only temporary global buffs in Run | SOURCE |
| 7 | Secondary cap | Secondary Level 99 via automatic Mastery XP; 100 XP/level; +500 Mastery/card; Auto Evolution at 99 | APPROVED Solo v1.1 |
| 8 | Tome cap | Tome Level Cap 30 | APPROVED |
| 9 | Chest UX | 3 cards/items presented, player chooses 1; Banish/Skip TBD | APPROVED |
| 10 | Elite XP | 60% Green / 30% Purple / 10% Red | APPROVED |
| 11 | Boss XP | 60% Purple / 40% Red | APPROVED |
| 12 | Gold/Coin/Diamond | Run/Meta/Premium currencies distinct | SOURCE |
| 13 | Co-op | Co-op 2–4 deferred to post-Solo release; no co-op mechanics in current build | DEFERRED |
| 14 | Enemy HP | HP(t)=HP0*(1+0.15t)^1.3 | SOURCE |
| 15 | Enemy Damage | Damage(t)=Damage0*(1+0.08t) | SOURCE |

---

## 10_Combat_Model

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 | Col 7 | Col 8 | Col 9 |
|---|---|---|---|---|---|---|---|---|---|
| 1 | WEAPON | BASE DAMAGE | COOLDOWN | CRIT CHANCE | CRIT MULT ASSUMED | ARMOR ASSUMED | EXPECTED HIT | EXPECTED DPS | SOURCE |
| 2 | Sword | 20.0 | 0.8 | 0.05 | 1.5 | 0.0 | =B2*(1+D2*(E2-1))*100/(100+F2) ⇒ 20.5 | =G2/C2 ⇒ 25.625 | GDD baseline; E/F assumed |
| 3 | Bow | 14.0 | 0.5 | 0.1 | 1.5 | 0.0 | =B3*(1+D3*(E3-1))*100/(100+F3) ⇒ 14.7 | =G3/C3 ⇒ 29.4 | GDD baseline; E/F assumed |
| 4 | Staff | 26.0 | 1.3 | 0.03 | 1.5 | 0.0 | =B4*(1+D4*(E4-1))*100/(100+F4) ⇒ 26.39 | =G4/C4 ⇒ 20.3 | GDD baseline; E/F assumed |
| 5 | Energy Orbs | 8.0 | 0.2 | 0.0 | 1.5 | 0.0 | =B5*(1+D5*(E5-1))*100/(100+F5) ⇒ 8 | =G5/C5 ⇒ 40 | Single-target tick equivalent |
| 6 | Daggers | 12.0 | 0.6 | 0.08 | 1.5 | 0.0 | =B6*(1+D6*(E6-1))*100/(100+F6) ⇒ 12.48 | =G6/C6 ⇒ 20.8 | Assumes every attack hits |

---

## 11_Enemy_TTK

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 | Col 7 | Col 8 | Col 9 |
|---|---|---|---|---|---|---|---|---|---|
| 1 | TIME MIN | ENEMY | HP0 ASSUMED | GROWTH | EXPONENT | HP(t) | SWORD DPS | TTK SECONDS | STATUS |
| 2 | 0.0 | Normal | 20.0 | 0.15 | 1.3 | =C2*(1+D2*A2)^E2 ⇒ 20 | ='10_Combat_Model'!H2 ⇒ 25.625 | =F2/G2 ⇒ 0.7804878049 | HP0 assumed |
| 3 | 3.0 | Normal | 20.0 | 0.15 | 1.3 | =C3*(1+D3*A3)^E3 ⇒ 32.41965572 | ='10_Combat_Model'!H2 ⇒ 25.625 | =F3/G3 ⇒ 1.265157296 | HP0 assumed |
| 4 | 6.0 | Normal | 20.0 | 0.15 | 1.3 | =C4*(1+D4*A4)^E4 ⇒ 46.06909458 | ='10_Combat_Model'!H2 ⇒ 25.625 | =F4/G4 ⇒ 1.797818325 | HP0 assumed |
| 5 | 9.0 | Normal | 20.0 | 0.15 | 1.3 | =C5*(1+D5*A5)^E5 ⇒ 60.73208856 | ='10_Combat_Model'!H2 ⇒ 25.625 | =F5/G5 ⇒ 2.370032724 | HP0 assumed |
| 6 | 12.0 | Normal | 20.0 | 0.15 | 1.3 | =C6*(1+D6*A6)^E6 ⇒ 76.2667845 | ='10_Combat_Model'!H2 ⇒ 25.625 | =F6/G6 ⇒ 2.976264761 | HP0 assumed |
| 7 | 0.0 | Elite | 100.0 | 0.15 | 1.3 | =C7*(1+D7*A7)^E7 ⇒ 100 | ='10_Combat_Model'!H2 ⇒ 25.625 | =F7/G7 ⇒ 3.902439024 | HP0 assumed |
| 8 | 3.0 | Elite | 100.0 | 0.15 | 1.3 | =C8*(1+D8*A8)^E8 ⇒ 162.0982786 | ='10_Combat_Model'!H2 ⇒ 25.625 | =F8/G8 ⇒ 6.325786482 | HP0 assumed |
| 9 | 6.0 | Elite | 100.0 | 0.15 | 1.3 | =C9*(1+D9*A9)^E9 ⇒ 230.3454729 | ='10_Combat_Model'!H2 ⇒ 25.625 | =F9/G9 ⇒ 8.989091625 | HP0 assumed |
| 10 | 9.0 | Elite | 100.0 | 0.15 | 1.3 | =C10*(1+D10*A10)^E10 ⇒ 303.6604428 | ='10_Combat_Model'!H2 ⇒ 25.625 | =F10/G10 ⇒ 11.85016362 | HP0 assumed |
| 11 | 12.0 | Elite | 100.0 | 0.15 | 1.3 | =C11*(1+D11*A11)^E11 ⇒ 381.3339225 | ='10_Combat_Model'!H2 ⇒ 25.625 | =F11/G11 ⇒ 14.88132381 | HP0 assumed |

---

## 12_XP_Approved_History

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 | Col 7 |
|---|---|---|---|---|---|---|---|
| 1 | SCENARIO | GREEN | PURPLE | RED | SUM | EXPECTED XP/ORB | STATUS |
| 2 | Normal | 0.8 | 0.18 | 0.02 | =SUM(B2:D2) ⇒ 1 | =B2*5+C2*20+D2*100 ⇒ 9.6 | SOURCE |
| 3 | Elite GDD A | 0.6 | 0.3 | 0.1 | =SUM(B3:D3) ⇒ 1 | =B3*5+C3*20+D3*100 ⇒ 19 | APPROVED: Elite 60/30/10 |
| 4 | Elite Balance V3.1 | 0.0 | 0.7 | 0.3 | =SUM(B4:D4) ⇒ 1 | =B4*5+C4*20+D4*100 ⇒ 44 | HISTORICAL ONLY — superseded |
| 5 | Boss GDD A | 0.0 | 0.6 | 0.4 | =SUM(B5:D5) ⇒ 1 | =B5*5+C5*20+D5*100 ⇒ 52 | APPROVED: Boss 0/60/40 |
| 6 | Boss Balance V3.1 | 0.0 | 0.0 | 1.0 | =SUM(B6:D6) ⇒ 1 | =B6*5+C6*20+D6*100 ⇒ 100 | HISTORICAL ONLY — superseded |
| 7 | Orb count | 3 orbs/enemy in GDD A |  |  |  |  | Verify probabilities per-orb |

---

## 13_Phase_Encounters

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 | Col 7 |
|---|---|---|---|---|---|---|---|
| 1 | PHASE | SURVIVAL SECONDS | MINIBOSS1 | MINIBOSS2 | PHASE BOSS | MAP BOSS | NOTE |
| 2 | 1 | 360.0 | 02:00 | 04:30 | 06:00 | No | Carry build |
| 3 | 2 | 360.0 | 02:00 | 04:30 | 06:00 | No | Carry build |
| 4 | 3 |  | No | No | No | Yes | No timer |
| 5 | Base survival seconds | =SUM(B2:B3) ⇒ 720 |  |  |  |  | Excludes Boss fight |
| 6 | Spawn when Boss | Normal OFF; Bat ON |  |  |  |  | Existing normal survive |
| 7 | Portal | Boss + required enemies dead |  |  |  |  | Claim gate TBD |

---

## 14_Economy_Guardrails

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 |
|---|---|---|---|---|---|
| 1 | SYSTEM | CURRENCY | SOURCE RULE | MISSING VALUE | STATUS |
| 2 | Merchant | Gold | 5 total/run; 75/20/5 rarity tiers | Prices & inventory | TBD: Merchant prices & inventory |
| 3 | Chest | Gold | Random map chest / guaranteed boss chest | Chest shows 3 choices; Banish/Skip and price TBD | APPROVED RULE; REMAINING DETAILS TBD |
| 4 | Permanent upgrades | Coin | Primary/Secondary/Armor/Pets | Cost, caps | TBD |
| 5 | Gacha | Diamond | Premium crates | Odds, pity, rate | TBD |
| 6 | Daily | Diamond | 4 quests complete = 20 Diamond | Quests/reset | TBD |
| 7 | End Run | Coin/material | Map Boss meta rewards | Reward formula | TBD |

---

## 15_Validation_Plan

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 |
|---|---|---|---|---|
| 1 | TEST | CATEGORY | ACCEPTANCE | PRIORITY |
| 2 | BAL-01 | Combat | DPS uses approved base damage; crit multiplier is assumed | P0 |
| 3 | BAL-02 | Enemy | Verify HP/Damage at 0,3,6,9,12 minutes | P0 |
| 4 | BAL-03 | XP | Verify XP formula segments 1-10,11-25,26+ | P0 |
| 5 | BAL-04 | XP drops | Elite 60/30/10 and Boss 0/60/40 approved; verify probabilities sum to 100% | P0 |
| 6 | BAL-05 | Boss | 2:00,4:30,6:00; Phase3 Map Boss | P0 |
| 7 | BAL-06 | Caps | Mastery 100 XP/level, +500 per Secondary Upgrade Card, Evolution auto at 99; test actual attainment at 12m | P0 |
| 8 | BAL-07 | Economy | Simulate Coin/Run Gold/min after prices decided | P1 |
| 9 | BAL-08 | Luck | Rarity tiers sum 100% | P1 |
| 10 | BAL-09 | Spawn | 20+5 per map = spawn batch baseline; active cap TBD, validate performance | P0 |

---

## 16_Secondary_Mastery

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 |
|---|---|---|---|---|---|---|
| 1 | PARAMETER / SCENARIO | VALUE | UNIT/FORMULA | STATUS / NOTE |  |  |
| 2 | Secondary Starting Level | 1.0 | Level | APPROVED Solo v1.1 |
| 3 | Secondary Max Level | 99.0 | Level | APPROVED |
| 4 | Mastery XP per Level | 100.0 | XP | APPROVED Playtest Baseline |
| 5 | Secondary Upgrade Card | 500.0 | Mastery XP | APPROVED baseline; +5 levels maximum |
| 6 | Mastery damage growth | 0.005 | per level | APPROVED baseline |
| 7 | Evolution damage bonus | 0.25 | multiplier | APPROVED baseline, Secondary only |
| 8 | Evolution trigger | Secondary Level 99 | Auto once | APPROVED; no extra card |
| 9 | XP granting | Same collected XP credited to Player and Secondary | After Secondary equipped | No double consumption; no retroactive XP |
| 10 | Pre-acquisition XP | Not granted to Secondary | None | APPROVED |
| 11 | Co-op | Deferred | None | Outside Solo V1 |
| 12 | Stainless Steel Pot | TBD | Beyond Level 99 interaction | NOT APPROVED |
| 13 | Mastery XP input (after equip) | 9800.0 | XP | Editable simulation input |
| 14 | Upgrade cards selected | 0.0 | Cards | Editable simulation input |
| 15 | Bonus mastery from cards | =B14*B5 ⇒ 0 | XP | Calculated |
| 16 | Total Mastery XP | =B13+B15 ⇒ 9800 | XP | Calculated |
| 17 | Secondary Level | =MIN(B3,B2+ROUNDDOWN(B16/B4,0)) ⇒ 99 | Level | Calculated |
| 18 | Secondary Damage Multiplier | =1+B6*(B17-1) ⇒ 1.49 | x | Calculated |
| 19 | Evolution active | =IF(B17>=B3,1,0) ⇒ 1 | 0 or 1 | Calculated |
| 20 | Damage multiplier after Evolution | =B18*IF(B19=1,1+B7,1) ⇒ 1.8625 | x | Calculated |
| 21 | Remaining XP to Evolution | =MAX(0,(B3-B2)*B4-B16) ⇒ 0 | XP | Calculated |
| 22 | Scenario | XP after equip | Upgrade cards | Secondary Level | Evolution? | Damage multiplier |
| 23 | Early secondary pickup | 9800.0 | 0.0 | =MIN($B$3,$B$2+ROUNDDOWN((B23+C23*$B$5)/$B$4,0)) ⇒ 99 | =IF(D23=$B$3,"YES","NO") ⇒ YES | =(1+$B$6*(D23-1))*IF(D23=$B$3,1+$B$7,1) ⇒ 1.8625 |
| 24 | Late secondary pickup | 5000.0 | 0.0 | =MIN($B$3,$B$2+ROUNDDOWN((B24+C24*$B$5)/$B$4,0)) ⇒ 51 | =IF(D24=$B$3,"YES","NO") ⇒ NO | =(1+$B$6*(D24-1))*IF(D24=$B$3,1+$B$7,1) ⇒ 1.25 |
| 25 | Late pickup + 4 upgrade cards | 7800.0 | 4.0 | =MIN($B$3,$B$2+ROUNDDOWN((B25+C25*$B$5)/$B$4,0)) ⇒ 99 | =IF(D25=$B$3,"YES","NO") ⇒ YES | =(1+$B$6*(D25-1))*IF(D25=$B$3,1+$B$7,1) ⇒ 1.8625 |

---

## 17_E2E_Inputs

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 |
|---|---|---|---|
| 1 | BEAT THE HORDE — SOLO E2E PLAYTEST INPUTS | VALUE | STATUS / SOURCE |
| 2 | Phase seconds | 360.0 | GDD APPROVED |
| 3 | Survival phases | 2.0 | GDD APPROVED |
| 4 | Starting player level | 1.0 | SIMULATION |
| 5 | Normal XP expected per orb | 9.6 | SOURCE: 80/18/2 |
| 6 | Orbs per normal kill | 3.0 | GDD A; confirm per-orb distribution |
| 7 | Elite XP expected per orb | 19.0 | APPROVED: 60/30/10 |
| 8 | Orbs per elite kill | 3.0 | SIMULATION: quantity TBD |
| 9 | Boss XP expected per orb | 52.0 | APPROVED 60/40 |
| 10 | Orbs per boss kill | 3.0 | SIMULATION: quantity TBD |
| 11 | Secondary acquisition minute | 2.0 | SIMULATION: via Level-up card |
| 12 | Mastery XP per level | 100.0 | APPROVED SOLO v1.1 |
| 13 | Secondary max level | 99.0 | APPROVED |
| 14 | Secondary upgrade card XP | 500.0 | APPROVED SOLO v1.1 |
| 15 | Secondary mastery damage per level | 0.005 | APPROVED SOLO v1.1 |
| 16 | Evolution bonus | 0.25 | APPROVED SOLO v1.1 |
| 17 | Normal kills per minute (start) | 12.0 | ASSUMPTION; modify by observed kills |
| 18 | Normal kills growth / minute | 2.0 | ASSUMPTION |
| 19 | Elite kills per minute (start) | 0.0 | ASSUMPTION |
| 20 | Elite kills growth / minute | 0.25 | ASSUMPTION |
| 21 | Pickup efficiency | 0.9 | ASSUMPTION (share of spawned XP picked up) |
| 22 | Secondary upgrade cards at minute 4 | 1.0 | ASSUMPTION |
| 23 | Secondary upgrade cards at minute 8 | 1.0 | ASSUMPTION |
| 24 | Secondary upgrade cards at minute 11 | 1.0 | ASSUMPTION |
| 25 | Mini-Boss kills minute 2 | 1.0 | SIMULATION |
| 26 | Mini-Boss kills minute 5 | 1.0 | SIMULATION |
| 27 | Mini-Boss kills minute 8 | 1.0 | SIMULATION |
| 28 | Mini-Boss kills minute 11 | 1.0 | SIMULATION |
| 29 | Boss kills minute 6 | 1.0 | SIMULATION; phase fight timing simplified |
| 30 | Boss kills minute 12 | 1.0 | SIMULATION; phase fight timing simplified |
| 31 | Mini-Boss orb count | 3.0 | ASSUMPTION; Mini-Boss XP qty TBD |
| 32 | Mini-Boss XP per orb | 52.0 | ASSUMPTION uses Boss color odds |
| 33 | Phase Boss orb count | 3.0 | ASSUMPTION |
| 34 | Total XP from Map Boss | 0.0 | TBD; Phase 3 separate |
| 35 | Run Gold per normal kill | 2.0 | ASSUMPTION |
| 36 | Run Gold per elite kill | 8.0 | ASSUMPTION |
| 37 | Run Gold per mini boss | 30.0 | ASSUMPTION |
| 38 | Run Gold per phase boss | 50.0 | ASSUMPTION |
| 39 | Run Gold per Map Boss | 100.0 | ASSUMPTION |
| 40 | Merchant/Chest spend during Run | 300.0 | ASSUMPTION |
| 41 | Coin completion reward | 150.0 | ASSUMPTION |
| 42 | Boss 1 HP | 3000.0 | ASSUMPTION |
| 43 | Boss 2 HP | 7000.0 | ASSUMPTION |
| 44 | Map Boss HP | 18000.0 | ASSUMPTION |
| 45 | Effective DPS Boss 1 | 75.0 | ASSUMPTION |
| 46 | Effective DPS Boss 2 | 120.0 | ASSUMPTION |
| 47 | Effective DPS Map Boss | 200.0 | ASSUMPTION |
| 48 | Warning | Minute-by-minute EV model, not RNG or real input simulation | NOT A PLAYTEST |
| 49 | Approved vs proposed | Blue inputs are editable; all derived values use formulas | Co-op excluded |

---

## 18_E2E_Run12Min

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 | Col 7 | Col 8 | Col 9 | Col 10 | Col 11 | Col 12 | Col 13 | Col 14 | Col 15 | Col 16 | Col 17 | Col 18 | Col 19 | Col 20 | Col 21 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | BEAT THE HORDE — SOLO END-TO-END 12-MINUTE RUN (EXPECTED VALUES) |
| 2 | Model: kills → XP orb drops → pickup → player level → card upgrades → Secondary Mastery → Evolution → Gold; Boss times simplified |
| 3 | Minute | Phase | Normal kills | Elite kills | MiniBoss kills | PhaseBoss kills | XP spawned | XP collected | XP cumulative | Player Level | Levels gained | Secondary owned | Mastery XP earned | Upgrade cards | Mastery XP cum | Secondary Level | Evolution | Secondary dmg x | Gold gained | Gold cumulative | Event |
| 4 | 1.0 | =IF(A4<=6,"PHASE 1","PHASE 2") ⇒ PHASE 1 | =MAX(0,'17_E2E_Inputs'!$B$17+(1-1)*'17_E2E_Inputs'!$B$18) ⇒ 12 | =MAX(0,'17_E2E_Inputs'!$B$19+(1-1)*'17_E2E_Inputs'!$B$20) ⇒ 0 | =IF(OR(A4=2,A4=5,A4=8,A4=11),1,0) ⇒ 0 | =IF(OR(A4=6,A4=12),1,0) ⇒ 0 | =C4*'17_E2E_Inputs'!$B$5*'17_E2E_Inputs'!$B$6+D4*'17_E2E_Inputs'!$B$7*'17_E2E_Inputs'!$B$8+E4*'17_E2E_Inputs'!$B$31*'17_E2E_Inputs'!$B$32+F4*'17_E2E_Inputs'!$B$9*'17_E2E_Inputs'!$B$33 ⇒ 345.6 | =G4*'17_E2E_Inputs'!$B$21 ⇒ 311.04 | =H4 ⇒ 311.04 | =MAX(1,COUNTIF('04_XP_Level'!$C$2:$C$103,"<="&I4)) ⇒ 3 | =MAX(0,J4-'17_E2E_Inputs'!$B$4) ⇒ 2 | =IF(A4>='17_E2E_Inputs'!$B$11,1,0) ⇒ 0 | =IF(L4=1,H4,0) ⇒ 0 | =IF(A4=4,'17_E2E_Inputs'!$B$22,0)+IF(A4=8,'17_E2E_Inputs'!$B$23,0)+IF(A4=11,'17_E2E_Inputs'!$B$24,0) ⇒ 0 | =M4+N4*'17_E2E_Inputs'!$B$14 ⇒ 0 | =IF(L4=0,0,MIN('17_E2E_Inputs'!$B$13,1+ROUNDDOWN(O4/'17_E2E_Inputs'!$B$12,0))) ⇒ 0 | =IF(P4>='17_E2E_Inputs'!$B$13,1,0) ⇒ 0 | =IF(P4=0,0,(1+'17_E2E_Inputs'!$B$15*(P4-1))*IF(Q4=1,1+'17_E2E_Inputs'!$B$16,1)) ⇒ 0 | =C4*'17_E2E_Inputs'!$B$35+D4*'17_E2E_Inputs'!$B$36+E4*'17_E2E_Inputs'!$B$37+F4*'17_E2E_Inputs'!$B$38 ⇒ 24 | =S4 ⇒ 24 | =IF(E4=1,"MiniBoss",IF(F4=1,"Phase Boss","Survival")) ⇒ Survival |
| 5 | 2.0 | =IF(A5<=6,"PHASE 1","PHASE 2") ⇒ PHASE 1 | =MAX(0,'17_E2E_Inputs'!$B$17+(2-1)*'17_E2E_Inputs'!$B$18) ⇒ 14 | =MAX(0,'17_E2E_Inputs'!$B$19+(2-1)*'17_E2E_Inputs'!$B$20) ⇒ 0.25 | =IF(OR(A5=2,A5=5,A5=8,A5=11),1,0) ⇒ 1 | =IF(OR(A5=6,A5=12),1,0) ⇒ 0 | =C5*'17_E2E_Inputs'!$B$5*'17_E2E_Inputs'!$B$6+D5*'17_E2E_Inputs'!$B$7*'17_E2E_Inputs'!$B$8+E5*'17_E2E_Inputs'!$B$31*'17_E2E_Inputs'!$B$32+F5*'17_E2E_Inputs'!$B$9*'17_E2E_Inputs'!$B$33 ⇒ 573.45 | =G5*'17_E2E_Inputs'!$B$21 ⇒ 516.105 | =I4+H5 ⇒ 827.145 | =MAX(1,COUNTIF('04_XP_Level'!$C$2:$C$103,"<="&I5)) ⇒ 5 | =MAX(0,J5-J4) ⇒ 2 | =IF(A5>='17_E2E_Inputs'!$B$11,1,0) ⇒ 1 | =IF(L5=1,H5,0) ⇒ 516.105 | =IF(A5=4,'17_E2E_Inputs'!$B$22,0)+IF(A5=8,'17_E2E_Inputs'!$B$23,0)+IF(A5=11,'17_E2E_Inputs'!$B$24,0) ⇒ 0 | =O4+M5+N5*'17_E2E_Inputs'!$B$14 ⇒ 516.105 | =IF(L5=0,0,MIN('17_E2E_Inputs'!$B$13,1+ROUNDDOWN(O5/'17_E2E_Inputs'!$B$12,0))) ⇒ 6 | =IF(P5>='17_E2E_Inputs'!$B$13,1,0) ⇒ 0 | =IF(P5=0,0,(1+'17_E2E_Inputs'!$B$15*(P5-1))*IF(Q5=1,1+'17_E2E_Inputs'!$B$16,1)) ⇒ 1.025 | =C5*'17_E2E_Inputs'!$B$35+D5*'17_E2E_Inputs'!$B$36+E5*'17_E2E_Inputs'!$B$37+F5*'17_E2E_Inputs'!$B$38 ⇒ 60 | =T4+S5 ⇒ 84 | =IF(E5=1,"MiniBoss",IF(F5=1,"Phase Boss","Survival")) ⇒ MiniBoss |
| 6 | 3.0 | =IF(A6<=6,"PHASE 1","PHASE 2") ⇒ PHASE 1 | =MAX(0,'17_E2E_Inputs'!$B$17+(3-1)*'17_E2E_Inputs'!$B$18) ⇒ 16 | =MAX(0,'17_E2E_Inputs'!$B$19+(3-1)*'17_E2E_Inputs'!$B$20) ⇒ 0.5 | =IF(OR(A6=2,A6=5,A6=8,A6=11),1,0) ⇒ 0 | =IF(OR(A6=6,A6=12),1,0) ⇒ 0 | =C6*'17_E2E_Inputs'!$B$5*'17_E2E_Inputs'!$B$6+D6*'17_E2E_Inputs'!$B$7*'17_E2E_Inputs'!$B$8+E6*'17_E2E_Inputs'!$B$31*'17_E2E_Inputs'!$B$32+F6*'17_E2E_Inputs'!$B$9*'17_E2E_Inputs'!$B$33 ⇒ 489.3 | =G6*'17_E2E_Inputs'!$B$21 ⇒ 440.37 | =I5+H6 ⇒ 1267.515 | =MAX(1,COUNTIF('04_XP_Level'!$C$2:$C$103,"<="&I6)) ⇒ 7 | =MAX(0,J6-J5) ⇒ 2 | =IF(A6>='17_E2E_Inputs'!$B$11,1,0) ⇒ 1 | =IF(L6=1,H6,0) ⇒ 440.37 | =IF(A6=4,'17_E2E_Inputs'!$B$22,0)+IF(A6=8,'17_E2E_Inputs'!$B$23,0)+IF(A6=11,'17_E2E_Inputs'!$B$24,0) ⇒ 0 | =O5+M6+N6*'17_E2E_Inputs'!$B$14 ⇒ 956.475 | =IF(L6=0,0,MIN('17_E2E_Inputs'!$B$13,1+ROUNDDOWN(O6/'17_E2E_Inputs'!$B$12,0))) ⇒ 10 | =IF(P6>='17_E2E_Inputs'!$B$13,1,0) ⇒ 0 | =IF(P6=0,0,(1+'17_E2E_Inputs'!$B$15*(P6-1))*IF(Q6=1,1+'17_E2E_Inputs'!$B$16,1)) ⇒ 1.045 | =C6*'17_E2E_Inputs'!$B$35+D6*'17_E2E_Inputs'!$B$36+E6*'17_E2E_Inputs'!$B$37+F6*'17_E2E_Inputs'!$B$38 ⇒ 36 | =T5+S6 ⇒ 120 | =IF(E6=1,"MiniBoss",IF(F6=1,"Phase Boss","Survival")) ⇒ Survival |
| 7 | 4.0 | =IF(A7<=6,"PHASE 1","PHASE 2") ⇒ PHASE 1 | =MAX(0,'17_E2E_Inputs'!$B$17+(4-1)*'17_E2E_Inputs'!$B$18) ⇒ 18 | =MAX(0,'17_E2E_Inputs'!$B$19+(4-1)*'17_E2E_Inputs'!$B$20) ⇒ 0.75 | =IF(OR(A7=2,A7=5,A7=8,A7=11),1,0) ⇒ 0 | =IF(OR(A7=6,A7=12),1,0) ⇒ 0 | =C7*'17_E2E_Inputs'!$B$5*'17_E2E_Inputs'!$B$6+D7*'17_E2E_Inputs'!$B$7*'17_E2E_Inputs'!$B$8+E7*'17_E2E_Inputs'!$B$31*'17_E2E_Inputs'!$B$32+F7*'17_E2E_Inputs'!$B$9*'17_E2E_Inputs'!$B$33 ⇒ 561.15 | =G7*'17_E2E_Inputs'!$B$21 ⇒ 505.035 | =I6+H7 ⇒ 1772.55 | =MAX(1,COUNTIF('04_XP_Level'!$C$2:$C$103,"<="&I7)) ⇒ 8 | =MAX(0,J7-J6) ⇒ 1 | =IF(A7>='17_E2E_Inputs'!$B$11,1,0) ⇒ 1 | =IF(L7=1,H7,0) ⇒ 505.035 | =IF(A7=4,'17_E2E_Inputs'!$B$22,0)+IF(A7=8,'17_E2E_Inputs'!$B$23,0)+IF(A7=11,'17_E2E_Inputs'!$B$24,0) ⇒ 1 | =O6+M7+N7*'17_E2E_Inputs'!$B$14 ⇒ 1961.51 | =IF(L7=0,0,MIN('17_E2E_Inputs'!$B$13,1+ROUNDDOWN(O7/'17_E2E_Inputs'!$B$12,0))) ⇒ 20 | =IF(P7>='17_E2E_Inputs'!$B$13,1,0) ⇒ 0 | =IF(P7=0,0,(1+'17_E2E_Inputs'!$B$15*(P7-1))*IF(Q7=1,1+'17_E2E_Inputs'!$B$16,1)) ⇒ 1.095 | =C7*'17_E2E_Inputs'!$B$35+D7*'17_E2E_Inputs'!$B$36+E7*'17_E2E_Inputs'!$B$37+F7*'17_E2E_Inputs'!$B$38 ⇒ 42 | =T6+S7 ⇒ 162 | =IF(E7=1,"MiniBoss",IF(F7=1,"Phase Boss","Survival")) ⇒ Survival |
| 8 | 5.0 | =IF(A8<=6,"PHASE 1","PHASE 2") ⇒ PHASE 1 | =MAX(0,'17_E2E_Inputs'!$B$17+(5-1)*'17_E2E_Inputs'!$B$18) ⇒ 20 | =MAX(0,'17_E2E_Inputs'!$B$19+(5-1)*'17_E2E_Inputs'!$B$20) ⇒ 1 | =IF(OR(A8=2,A8=5,A8=8,A8=11),1,0) ⇒ 1 | =IF(OR(A8=6,A8=12),1,0) ⇒ 0 | =C8*'17_E2E_Inputs'!$B$5*'17_E2E_Inputs'!$B$6+D8*'17_E2E_Inputs'!$B$7*'17_E2E_Inputs'!$B$8+E8*'17_E2E_Inputs'!$B$31*'17_E2E_Inputs'!$B$32+F8*'17_E2E_Inputs'!$B$9*'17_E2E_Inputs'!$B$33 ⇒ 789 | =G8*'17_E2E_Inputs'!$B$21 ⇒ 710.1 | =I7+H8 ⇒ 2482.65 | =MAX(1,COUNTIF('04_XP_Level'!$C$2:$C$103,"<="&I8)) ⇒ 9 | =MAX(0,J8-J7) ⇒ 1 | =IF(A8>='17_E2E_Inputs'!$B$11,1,0) ⇒ 1 | =IF(L8=1,H8,0) ⇒ 710.1 | =IF(A8=4,'17_E2E_Inputs'!$B$22,0)+IF(A8=8,'17_E2E_Inputs'!$B$23,0)+IF(A8=11,'17_E2E_Inputs'!$B$24,0) ⇒ 0 | =O7+M8+N8*'17_E2E_Inputs'!$B$14 ⇒ 2671.61 | =IF(L8=0,0,MIN('17_E2E_Inputs'!$B$13,1+ROUNDDOWN(O8/'17_E2E_Inputs'!$B$12,0))) ⇒ 27 | =IF(P8>='17_E2E_Inputs'!$B$13,1,0) ⇒ 0 | =IF(P8=0,0,(1+'17_E2E_Inputs'!$B$15*(P8-1))*IF(Q8=1,1+'17_E2E_Inputs'!$B$16,1)) ⇒ 1.13 | =C8*'17_E2E_Inputs'!$B$35+D8*'17_E2E_Inputs'!$B$36+E8*'17_E2E_Inputs'!$B$37+F8*'17_E2E_Inputs'!$B$38 ⇒ 78 | =T7+S8 ⇒ 240 | =IF(E8=1,"MiniBoss",IF(F8=1,"Phase Boss","Survival")) ⇒ MiniBoss |
| 9 | 6.0 | =IF(A9<=6,"PHASE 1","PHASE 2") ⇒ PHASE 1 | =MAX(0,'17_E2E_Inputs'!$B$17+(6-1)*'17_E2E_Inputs'!$B$18) ⇒ 22 | =MAX(0,'17_E2E_Inputs'!$B$19+(6-1)*'17_E2E_Inputs'!$B$20) ⇒ 1.25 | =IF(OR(A9=2,A9=5,A9=8,A9=11),1,0) ⇒ 0 | =IF(OR(A9=6,A9=12),1,0) ⇒ 1 | =C9*'17_E2E_Inputs'!$B$5*'17_E2E_Inputs'!$B$6+D9*'17_E2E_Inputs'!$B$7*'17_E2E_Inputs'!$B$8+E9*'17_E2E_Inputs'!$B$31*'17_E2E_Inputs'!$B$32+F9*'17_E2E_Inputs'!$B$9*'17_E2E_Inputs'!$B$33 ⇒ 860.85 | =G9*'17_E2E_Inputs'!$B$21 ⇒ 774.765 | =I8+H9 ⇒ 3257.415 | =MAX(1,COUNTIF('04_XP_Level'!$C$2:$C$103,"<="&I9)) ⇒ 10 | =MAX(0,J9-J8) ⇒ 1 | =IF(A9>='17_E2E_Inputs'!$B$11,1,0) ⇒ 1 | =IF(L9=1,H9,0) ⇒ 774.765 | =IF(A9=4,'17_E2E_Inputs'!$B$22,0)+IF(A9=8,'17_E2E_Inputs'!$B$23,0)+IF(A9=11,'17_E2E_Inputs'!$B$24,0) ⇒ 0 | =O8+M9+N9*'17_E2E_Inputs'!$B$14 ⇒ 3446.375 | =IF(L9=0,0,MIN('17_E2E_Inputs'!$B$13,1+ROUNDDOWN(O9/'17_E2E_Inputs'!$B$12,0))) ⇒ 35 | =IF(P9>='17_E2E_Inputs'!$B$13,1,0) ⇒ 0 | =IF(P9=0,0,(1+'17_E2E_Inputs'!$B$15*(P9-1))*IF(Q9=1,1+'17_E2E_Inputs'!$B$16,1)) ⇒ 1.17 | =C9*'17_E2E_Inputs'!$B$35+D9*'17_E2E_Inputs'!$B$36+E9*'17_E2E_Inputs'!$B$37+F9*'17_E2E_Inputs'!$B$38 ⇒ 104 | =T8+S9 ⇒ 344 | =IF(E9=1,"MiniBoss",IF(F9=1,"Phase Boss","Survival")) ⇒ Phase Boss |
| 10 | 7.0 | =IF(A10<=6,"PHASE 1","PHASE 2") ⇒ PHASE 2 | =MAX(0,'17_E2E_Inputs'!$B$17+(7-1)*'17_E2E_Inputs'!$B$18) ⇒ 24 | =MAX(0,'17_E2E_Inputs'!$B$19+(7-1)*'17_E2E_Inputs'!$B$20) ⇒ 1.5 | =IF(OR(A10=2,A10=5,A10=8,A10=11),1,0) ⇒ 0 | =IF(OR(A10=6,A10=12),1,0) ⇒ 0 | =C10*'17_E2E_Inputs'!$B$5*'17_E2E_Inputs'!$B$6+D10*'17_E2E_Inputs'!$B$7*'17_E2E_Inputs'!$B$8+E10*'17_E2E_Inputs'!$B$31*'17_E2E_Inputs'!$B$32+F10*'17_E2E_Inputs'!$B$9*'17_E2E_Inputs'!$B$33 ⇒ 776.7 | =G10*'17_E2E_Inputs'!$B$21 ⇒ 699.03 | =I9+H10 ⇒ 3956.445 | =MAX(1,COUNTIF('04_XP_Level'!$C$2:$C$103,"<="&I10)) ⇒ 11 | =MAX(0,J10-J9) ⇒ 1 | =IF(A10>='17_E2E_Inputs'!$B$11,1,0) ⇒ 1 | =IF(L10=1,H10,0) ⇒ 699.03 | =IF(A10=4,'17_E2E_Inputs'!$B$22,0)+IF(A10=8,'17_E2E_Inputs'!$B$23,0)+IF(A10=11,'17_E2E_Inputs'!$B$24,0) ⇒ 0 | =O9+M10+N10*'17_E2E_Inputs'!$B$14 ⇒ 4145.405 | =IF(L10=0,0,MIN('17_E2E_Inputs'!$B$13,1+ROUNDDOWN(O10/'17_E2E_Inputs'!$B$12,0))) ⇒ 42 | =IF(P10>='17_E2E_Inputs'!$B$13,1,0) ⇒ 0 | =IF(P10=0,0,(1+'17_E2E_Inputs'!$B$15*(P10-1))*IF(Q10=1,1+'17_E2E_Inputs'!$B$16,1)) ⇒ 1.205 | =C10*'17_E2E_Inputs'!$B$35+D10*'17_E2E_Inputs'!$B$36+E10*'17_E2E_Inputs'!$B$37+F10*'17_E2E_Inputs'!$B$38 ⇒ 60 | =T9+S10 ⇒ 404 | =IF(E10=1,"MiniBoss",IF(F10=1,"Phase Boss","Survival")) ⇒ Survival |
| 11 | 8.0 | =IF(A11<=6,"PHASE 1","PHASE 2") ⇒ PHASE 2 | =MAX(0,'17_E2E_Inputs'!$B$17+(8-1)*'17_E2E_Inputs'!$B$18) ⇒ 26 | =MAX(0,'17_E2E_Inputs'!$B$19+(8-1)*'17_E2E_Inputs'!$B$20) ⇒ 1.75 | =IF(OR(A11=2,A11=5,A11=8,A11=11),1,0) ⇒ 1 | =IF(OR(A11=6,A11=12),1,0) ⇒ 0 | =C11*'17_E2E_Inputs'!$B$5*'17_E2E_Inputs'!$B$6+D11*'17_E2E_Inputs'!$B$7*'17_E2E_Inputs'!$B$8+E11*'17_E2E_Inputs'!$B$31*'17_E2E_Inputs'!$B$32+F11*'17_E2E_Inputs'!$B$9*'17_E2E_Inputs'!$B$33 ⇒ 1004.55 | =G11*'17_E2E_Inputs'!$B$21 ⇒ 904.095 | =I10+H11 ⇒ 4860.54 | =MAX(1,COUNTIF('04_XP_Level'!$C$2:$C$103,"<="&I11)) ⇒ 12 | =MAX(0,J11-J10) ⇒ 1 | =IF(A11>='17_E2E_Inputs'!$B$11,1,0) ⇒ 1 | =IF(L11=1,H11,0) ⇒ 904.095 | =IF(A11=4,'17_E2E_Inputs'!$B$22,0)+IF(A11=8,'17_E2E_Inputs'!$B$23,0)+IF(A11=11,'17_E2E_Inputs'!$B$24,0) ⇒ 1 | =O10+M11+N11*'17_E2E_Inputs'!$B$14 ⇒ 5549.5 | =IF(L11=0,0,MIN('17_E2E_Inputs'!$B$13,1+ROUNDDOWN(O11/'17_E2E_Inputs'!$B$12,0))) ⇒ 56 | =IF(P11>='17_E2E_Inputs'!$B$13,1,0) ⇒ 0 | =IF(P11=0,0,(1+'17_E2E_Inputs'!$B$15*(P11-1))*IF(Q11=1,1+'17_E2E_Inputs'!$B$16,1)) ⇒ 1.275 | =C11*'17_E2E_Inputs'!$B$35+D11*'17_E2E_Inputs'!$B$36+E11*'17_E2E_Inputs'!$B$37+F11*'17_E2E_Inputs'!$B$38 ⇒ 96 | =T10+S11 ⇒ 500 | =IF(E11=1,"MiniBoss",IF(F11=1,"Phase Boss","Survival")) ⇒ MiniBoss |
| 12 | 9.0 | =IF(A12<=6,"PHASE 1","PHASE 2") ⇒ PHASE 2 | =MAX(0,'17_E2E_Inputs'!$B$17+(9-1)*'17_E2E_Inputs'!$B$18) ⇒ 28 | =MAX(0,'17_E2E_Inputs'!$B$19+(9-1)*'17_E2E_Inputs'!$B$20) ⇒ 2 | =IF(OR(A12=2,A12=5,A12=8,A12=11),1,0) ⇒ 0 | =IF(OR(A12=6,A12=12),1,0) ⇒ 0 | =C12*'17_E2E_Inputs'!$B$5*'17_E2E_Inputs'!$B$6+D12*'17_E2E_Inputs'!$B$7*'17_E2E_Inputs'!$B$8+E12*'17_E2E_Inputs'!$B$31*'17_E2E_Inputs'!$B$32+F12*'17_E2E_Inputs'!$B$9*'17_E2E_Inputs'!$B$33 ⇒ 920.4 | =G12*'17_E2E_Inputs'!$B$21 ⇒ 828.36 | =I11+H12 ⇒ 5688.9 | =MAX(1,COUNTIF('04_XP_Level'!$C$2:$C$103,"<="&I12)) ⇒ 13 | =MAX(0,J12-J11) ⇒ 1 | =IF(A12>='17_E2E_Inputs'!$B$11,1,0) ⇒ 1 | =IF(L12=1,H12,0) ⇒ 828.36 | =IF(A12=4,'17_E2E_Inputs'!$B$22,0)+IF(A12=8,'17_E2E_Inputs'!$B$23,0)+IF(A12=11,'17_E2E_Inputs'!$B$24,0) ⇒ 0 | =O11+M12+N12*'17_E2E_Inputs'!$B$14 ⇒ 6377.86 | =IF(L12=0,0,MIN('17_E2E_Inputs'!$B$13,1+ROUNDDOWN(O12/'17_E2E_Inputs'!$B$12,0))) ⇒ 64 | =IF(P12>='17_E2E_Inputs'!$B$13,1,0) ⇒ 0 | =IF(P12=0,0,(1+'17_E2E_Inputs'!$B$15*(P12-1))*IF(Q12=1,1+'17_E2E_Inputs'!$B$16,1)) ⇒ 1.315 | =C12*'17_E2E_Inputs'!$B$35+D12*'17_E2E_Inputs'!$B$36+E12*'17_E2E_Inputs'!$B$37+F12*'17_E2E_Inputs'!$B$38 ⇒ 72 | =T11+S12 ⇒ 572 | =IF(E12=1,"MiniBoss",IF(F12=1,"Phase Boss","Survival")) ⇒ Survival |
| 13 | 10.0 | =IF(A13<=6,"PHASE 1","PHASE 2") ⇒ PHASE 2 | =MAX(0,'17_E2E_Inputs'!$B$17+(10-1)*'17_E2E_Inputs'!$B$18) ⇒ 30 | =MAX(0,'17_E2E_Inputs'!$B$19+(10-1)*'17_E2E_Inputs'!$B$20) ⇒ 2.25 | =IF(OR(A13=2,A13=5,A13=8,A13=11),1,0) ⇒ 0 | =IF(OR(A13=6,A13=12),1,0) ⇒ 0 | =C13*'17_E2E_Inputs'!$B$5*'17_E2E_Inputs'!$B$6+D13*'17_E2E_Inputs'!$B$7*'17_E2E_Inputs'!$B$8+E13*'17_E2E_Inputs'!$B$31*'17_E2E_Inputs'!$B$32+F13*'17_E2E_Inputs'!$B$9*'17_E2E_Inputs'!$B$33 ⇒ 992.25 | =G13*'17_E2E_Inputs'!$B$21 ⇒ 893.025 | =I12+H13 ⇒ 6581.925 | =MAX(1,COUNTIF('04_XP_Level'!$C$2:$C$103,"<="&I13)) ⇒ 14 | =MAX(0,J13-J12) ⇒ 1 | =IF(A13>='17_E2E_Inputs'!$B$11,1,0) ⇒ 1 | =IF(L13=1,H13,0) ⇒ 893.025 | =IF(A13=4,'17_E2E_Inputs'!$B$22,0)+IF(A13=8,'17_E2E_Inputs'!$B$23,0)+IF(A13=11,'17_E2E_Inputs'!$B$24,0) ⇒ 0 | =O12+M13+N13*'17_E2E_Inputs'!$B$14 ⇒ 7270.885 | =IF(L13=0,0,MIN('17_E2E_Inputs'!$B$13,1+ROUNDDOWN(O13/'17_E2E_Inputs'!$B$12,0))) ⇒ 73 | =IF(P13>='17_E2E_Inputs'!$B$13,1,0) ⇒ 0 | =IF(P13=0,0,(1+'17_E2E_Inputs'!$B$15*(P13-1))*IF(Q13=1,1+'17_E2E_Inputs'!$B$16,1)) ⇒ 1.36 | =C13*'17_E2E_Inputs'!$B$35+D13*'17_E2E_Inputs'!$B$36+E13*'17_E2E_Inputs'!$B$37+F13*'17_E2E_Inputs'!$B$38 ⇒ 78 | =T12+S13 ⇒ 650 | =IF(E13=1,"MiniBoss",IF(F13=1,"Phase Boss","Survival")) ⇒ Survival |
| 14 | 11.0 | =IF(A14<=6,"PHASE 1","PHASE 2") ⇒ PHASE 2 | =MAX(0,'17_E2E_Inputs'!$B$17+(11-1)*'17_E2E_Inputs'!$B$18) ⇒ 32 | =MAX(0,'17_E2E_Inputs'!$B$19+(11-1)*'17_E2E_Inputs'!$B$20) ⇒ 2.5 | =IF(OR(A14=2,A14=5,A14=8,A14=11),1,0) ⇒ 1 | =IF(OR(A14=6,A14=12),1,0) ⇒ 0 | =C14*'17_E2E_Inputs'!$B$5*'17_E2E_Inputs'!$B$6+D14*'17_E2E_Inputs'!$B$7*'17_E2E_Inputs'!$B$8+E14*'17_E2E_Inputs'!$B$31*'17_E2E_Inputs'!$B$32+F14*'17_E2E_Inputs'!$B$9*'17_E2E_Inputs'!$B$33 ⇒ 1220.1 | =G14*'17_E2E_Inputs'!$B$21 ⇒ 1098.09 | =I13+H14 ⇒ 7680.015 | =MAX(1,COUNTIF('04_XP_Level'!$C$2:$C$103,"<="&I14)) ⇒ 15 | =MAX(0,J14-J13) ⇒ 1 | =IF(A14>='17_E2E_Inputs'!$B$11,1,0) ⇒ 1 | =IF(L14=1,H14,0) ⇒ 1098.09 | =IF(A14=4,'17_E2E_Inputs'!$B$22,0)+IF(A14=8,'17_E2E_Inputs'!$B$23,0)+IF(A14=11,'17_E2E_Inputs'!$B$24,0) ⇒ 1 | =O13+M14+N14*'17_E2E_Inputs'!$B$14 ⇒ 8868.975 | =IF(L14=0,0,MIN('17_E2E_Inputs'!$B$13,1+ROUNDDOWN(O14/'17_E2E_Inputs'!$B$12,0))) ⇒ 89 | =IF(P14>='17_E2E_Inputs'!$B$13,1,0) ⇒ 0 | =IF(P14=0,0,(1+'17_E2E_Inputs'!$B$15*(P14-1))*IF(Q14=1,1+'17_E2E_Inputs'!$B$16,1)) ⇒ 1.44 | =C14*'17_E2E_Inputs'!$B$35+D14*'17_E2E_Inputs'!$B$36+E14*'17_E2E_Inputs'!$B$37+F14*'17_E2E_Inputs'!$B$38 ⇒ 114 | =T13+S14 ⇒ 764 | =IF(E14=1,"MiniBoss",IF(F14=1,"Phase Boss","Survival")) ⇒ MiniBoss |
| 15 | 12.0 | =IF(A15<=6,"PHASE 1","PHASE 2") ⇒ PHASE 2 | =MAX(0,'17_E2E_Inputs'!$B$17+(12-1)*'17_E2E_Inputs'!$B$18) ⇒ 34 | =MAX(0,'17_E2E_Inputs'!$B$19+(12-1)*'17_E2E_Inputs'!$B$20) ⇒ 2.75 | =IF(OR(A15=2,A15=5,A15=8,A15=11),1,0) ⇒ 0 | =IF(OR(A15=6,A15=12),1,0) ⇒ 1 | =C15*'17_E2E_Inputs'!$B$5*'17_E2E_Inputs'!$B$6+D15*'17_E2E_Inputs'!$B$7*'17_E2E_Inputs'!$B$8+E15*'17_E2E_Inputs'!$B$31*'17_E2E_Inputs'!$B$32+F15*'17_E2E_Inputs'!$B$9*'17_E2E_Inputs'!$B$33 ⇒ 1291.95 | =G15*'17_E2E_Inputs'!$B$21 ⇒ 1162.755 | =I14+H15 ⇒ 8842.77 | =MAX(1,COUNTIF('04_XP_Level'!$C$2:$C$103,"<="&I15)) ⇒ 16 | =MAX(0,J15-J14) ⇒ 1 | =IF(A15>='17_E2E_Inputs'!$B$11,1,0) ⇒ 1 | =IF(L15=1,H15,0) ⇒ 1162.755 | =IF(A15=4,'17_E2E_Inputs'!$B$22,0)+IF(A15=8,'17_E2E_Inputs'!$B$23,0)+IF(A15=11,'17_E2E_Inputs'!$B$24,0) ⇒ 0 | =O14+M15+N15*'17_E2E_Inputs'!$B$14 ⇒ 10031.73 | =IF(L15=0,0,MIN('17_E2E_Inputs'!$B$13,1+ROUNDDOWN(O15/'17_E2E_Inputs'!$B$12,0))) ⇒ 99 | =IF(P15>='17_E2E_Inputs'!$B$13,1,0) ⇒ 1 | =IF(P15=0,0,(1+'17_E2E_Inputs'!$B$15*(P15-1))*IF(Q15=1,1+'17_E2E_Inputs'!$B$16,1)) ⇒ 1.8625 | =C15*'17_E2E_Inputs'!$B$35+D15*'17_E2E_Inputs'!$B$36+E15*'17_E2E_Inputs'!$B$37+F15*'17_E2E_Inputs'!$B$38 ⇒ 140 | =T14+S15 ⇒ 904 | =IF(E15=1,"MiniBoss",IF(F15=1,"Phase Boss","Survival")) ⇒ Phase Boss |
| 17 | SUMMARY | VALUE | STATUS / INTERPRETATION |
| 18 | Survival duration (seconds) | ='17_E2E_Inputs'!B2*'17_E2E_Inputs'!B3 ⇒ 720 | APPROVED; excludes Boss encounters |
| 19 | Final Player Level | =J15 ⇒ 16 | Expected XP model |
| 20 | Final Secondary Level | =P15 ⇒ 99 | Compare to 99 |
| 21 | Evolution happened? | =IF(Q15=1,"YES","NO") ⇒ YES | Requires 9800 Mastery XP after pickup |
| 22 | XP collected | =I15 ⇒ 8842.77 | Expected (not stochastic) |
| 23 | Mastery XP | =O15 ⇒ 10031.73 | XP after secondary + upgrade cards |
| 24 | Gold earned | =T15 ⇒ 904 | Assumed amounts |
| 25 | Gold net | =B24-'17_E2E_Inputs'!B40 ⇒ 604 | Expenses assumption |
| 26 | Boss 1 TTK (s) | ='17_E2E_Inputs'!B42/'17_E2E_Inputs'!B45 ⇒ 40 | Does not include invulnerability/movement |
| 27 | Boss 2 TTK (s) | ='17_E2E_Inputs'!B43/'17_E2E_Inputs'!B46 ⇒ 58.33333333 | Does not include invulnerability/movement |
| 28 | Map Boss TTK (s) | ='17_E2E_Inputs'!B44/'17_E2E_Inputs'!B47 ⇒ 90 | Phase3 simplified |
| 29 | Run total projected (min) | =(B18+B26+B27+B28)/60 ⇒ 15.13888889 | 12min survival + Boss TTKS (simplified) |
| 30 | Coin reward | ='17_E2E_Inputs'!B41 ⇒ 150 | Assumption |

---

## 19_BLV2_ChangeRegister

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 |
|---|---|---|---|---|---|---|
| 1 | ID | System | Change / rule | Source status | Implementation owner | Acceptance |
| 2 | BL-01 | Clock | Survival time only; freeze at boss/portal; Phase2 resumes at 360s | PROPOSED | GSD clock | At 720s exactly two survival phases |
| 3 | BL-02 | Enemy | HP=HP0*(1+0.15*t)^1.3; DMG=DMG0*(1+0.08*t); snapshot on spawn | PROPOSED; formula SOURCE | Spawn | Living enemy stats do not change |
| 4 | BL-03 | XP | Orb pickup once; Secondary gets same XP only after equip timestamp | PROPOSED event precision | XP/Mastery | No retroactive XP |
| 5 | BL-04 | Mastery | Cap99; 100XP/level; 500XP/card; Evolution once +25% | APPROVED baseline | Mastery | Threshold=9800 |
| 6 | BL-05 | Cards | 3 eligible options; seeded rarity and type-weight RNG; no fixed acquisition minute | PROPOSED | Card Director | No invalid offers; reproducible seed |
| 7 | BL-06 | Boss | DPS from build and actual uptime/hit; snapshot boss; log TTK | PROPOSED | Combat | No hardcoded 75/120/200 validation |
| 8 | BL-07 | Survival | Player HP, armor, regen, damage, dodge per encounter | PROPOSED | Combat | Actual death/success derived |
| 9 | BL-08 | Economy | Gold in-run; Coin meta; Map Boss/exit/fail reward separate | PROPOSED | Economy | No double grant or Gold->Coin implicit |
| 10 | BL-09 | Tome | 2 base slots; slot3/4 Coin OR Robux; prices TBD | USER CONFIRMED | Lobby | F2P unlock route exists |
| 11 | BL-10 | Future combo/story | No combo bonus yet; separate approval | FUTURE | GDD | Baseline unaffected |

---

## 20_BLV2_RuntimeTimeline

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 | Col 7 |
|---|---|---|---|---|---|---|---|
| 1 | Start survival sec | End survival sec | Phase | Event / state | Clock | Formula applied | Update timing |
| 2 | 0.0 | 120.0 | Phase1 | Survival + normal/elite | RUN | Enemy HP & Damage(t) | At spawn; event damage at hit |
| 3 | 120.0 | 270.0 | Phase1 | MiniBoss1 at 120s | RUN | Boss snapshot / combat | Once on spawn |
| 4 | 270.0 | 360.0 | Phase1 | MiniBoss2 at 270s | RUN | Boss snapshot / combat | Once on spawn |
| 5 | 360.0 | 360.0 | Phase1 | PhaseBoss1 / chest portal | PAUSE | No survival scale increment | Boss elapsed separately |
| 6 | 360.0 | 480.0 | Phase2 | Survival continues at 360s | RUN | Enemy scaling continuous t | Per spawn |
| 7 | 480.0 | 630.0 | Phase2 | MiniBoss3 at 480s | RUN | Boss stats | Once on spawn |
| 8 | 630.0 | 720.0 | Phase2 | MiniBoss4 at 630s | RUN | Boss stats | Once on spawn |
| 9 | 720.0 | 720.0 | Phase2 | PhaseBoss2 / chest portal | PAUSE | No survival scale increment | Boss elapsed separately |
| 10 | 720.0 | 720.0 | Phase3 | Map Boss no survival timer | PAUSE | Dedicated boss stats; build DPS | On spawn/hit |
| 11 | EVENT | EVENT | Any | XP pickup / Secondary equip | EVENT | Player XP + mastery if equipped | Per validated orb |
| 12 | EVENT | EVENT | Any | Card / Tome / evolution | EVENT | Eligibility, modifiers, threshold | On state change |
| 13 | EVENT | EVENT | Any | Gold Coin ledger | EVENT | Transactional reward | On validated grant/spend |

---

## 21_BLV2_E2E_Acceptance

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 |
|---|---|---|---|---|---|
| 1 | Test ID | Scenario | PASS condition | Current | Action / evidence |
| 2 | T01 | Clock continuity | Phase 2 starts at 360s; bosses freeze clock | NOT TESTED | Sim with per-second state |
| 3 | T02 | Enemy snapshot | No existing enemy stat inflation | NOT TESTED | Spawn timestamp vs stats |
| 4 | T03 | XP after equip | Secondary excludes XP picked before equip | NOT TESTED | Event log equippedAt |
| 5 | T04 | Mastery 99 | Exactly 9800 XP threshold / Evolution once | MODEL PASS | 10031.73 modeled total |
| 6 | T05 | Card RNG | 3 eligible cards & valid weighted odds | NOT TESTED | Seeded 1000 runs |
| 7 | T06 | Boss DPS | DPS derived from build not assumed | NOT TESTED | Telemetry and damage logs |
| 8 | T07 | Survival | Death and win validated by HP over timeline | NOT TESTED | Encounter simulation |
| 9 | T08 | Tome 2 vs 4 | No impossible 2-slot boss, no cap violation 4-slot | NOT TESTED | Win rates / TTK |
| 10 | T09 | Economy | Gold/coin and Map Boss ledger reconcile | PARTIAL | 904 Gold - 300 spend = 604 before Phase3 |
| 11 | T10 | 12m progression | Player L16 from 8842.77 XP | MODEL PASS | Expected-value only |
| 12 | T11 | TTK boss | 40/58.33/90s arithmetic only | ARITHMETIC PASS | Not gameplay PASS |

---

## 22_BLV2_E2E_Scenarios

*Kích thước dữ liệu: None hàng × None cột (bao gồm vùng định dạng/ô trống).*

| Row | Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 |
|---|---|---|---|---|---|---|
| 1 | ID | Scenario | Input / setup | Steps | Expected result / PASS criteria | Status |
| 2 | E2E-01 | Baseline deterministic | Phase=360s x2; Secondary equip t=120s; seed=1 | Run 0-720s; resolve XP/cards/boss by events | No Mastery before equip; two phase bosses; MapBoss follows; reconciled ledger | NOT RUN |
| 3 | E2E-02 | Secondary equip late | equip t=360s; same XP/drop seed | Run replay with changed equip timestamp | Mastery lower than E2E-01 for same pickups; no retroactive grant | NOT RUN |
| 4 | E2E-03 | Low pickup rate | pickup rate=60% vs 90% baseline | Run seeds 1-1000; compare progression | Reported Evolution rate, levels, win rate; no fabricated PASS target | NOT RUN |
| 5 | E2E-04 | High pickup rate | pickup rate=100%; baseline enemy kills | Replay 1000 seeds | Mastery and player XP never less than same-seed 90% case, assuming identical kills | NOT RUN |
| 6 | E2E-05 | Secondary mastery threshold | Mastery 9799 -> +1 | Grant validated XP pickup | Level 99, Evolution exactly once, +25% damage on Secondary | NOT RUN |
| 7 | E2E-06 | Duplicate orb pickup | same orbId awarded twice | Submit identical pickup event twice | Exactly one Player XP grant, one Secondary Mastery grant | NOT RUN |
| 8 | E2E-07 | Card pool empty / cap | Tome slots filled; all upgrade max | Trigger LevelUp; roll 3 cards | Never offers unavailable acquisition/max level; fallback explicitly logged if <3 eligible | NOT RUN |
| 9 | E2E-08 | Card RNG reproducibility | seed=42; fixed eligible pool + weights | Generate offers twice | Exact same three offers/order for same seed and sequence; probabilities sum 1 | NOT RUN |
| 10 | E2E-09 | Two Tome slots | F2P baseline; 2-slot build | Run 1000 seeded scenarios with player survival | Record win rate, DPS, TTK, evolution; feasibility assessed against design thresholds TBD | NOT RUN |
| 11 | E2E-10 | Four Tome slots | Coin- or Robux-unlocked; 4-slot build | Replay matched seeds & primary equipment | No stat-cap violations; compare win/TTK with E2E-09; unlocking method affects no combat stats | NOT RUN |
| 12 | E2E-11 | Boss damage derived | Sword+Secondary+Tomes, configured hitrate/uptime | Fight bosses at t=360/720 | Boss TTK computed from effective per-hit damage; no hardcoded DPS inputs | NOT RUN |
| 13 | E2E-12 | Player survival | Player max HP, armor, dash, regen; boss telegraphs | Simulate incoming hit events | Damage mitigation applied once, death triggers terminal state; no actions after death | NOT RUN |
| 14 | E2E-13 | Clock pause during boss | Boss1 fight 60s, chest 30s | Advance wall clock from 360s to 450s | SurvivalElapsed stays 360s; Phase2 resumes at 360s | NOT RUN |
| 15 | E2E-14 | Existing enemy stat snapshot | Enemy spawned t=60s survives until t=600s | Inspect enemy stats on spawn and later | Enemy HP max/damage remain snapshot; new enemy t=600s uses t=10m scale | NOT RUN |
| 16 | E2E-15 | Spawn performance | Active cap variable; spawn tick 1s proposal | Force kills below spawn rate | Active enemy count never exceeds configured cap; game continues | NOT RUN |
| 17 | E2E-16 | Gold / Coin reconciliation | 904 Gold earned; 300 spent; MapBoss Gold=100 assumed | Complete all phases, claim rewards twice | Before Phase3 net Gold 604; after optional MapBoss grant 704; Coin event idempotent | NOT RUN |
| 18 | E2E-17 | Fail / early exit rewards | Death at 5m; exit after Phase1 | End runs on both conditions | No completion reward unless explicit reward table permits; no double-grant | NOT RUN |
| 19 | E2E-18 | Phase transition | Enemy alive at t=360; portal gating TBD | Defeat Boss then test unresolved enemy | Living enemies preserved; transition follows finalized gate, currently TBD | BLOCKED DESIGN |
| 20 | E2E-19 | Admin config snapshot | Published config v2 during active run v1 | Change remote config after RunInit | Active run stays immutable v1; next run uses approved v2 | NOT RUN |
| 21 | E2E-20 | Robux/Coin purchase parity | Same unlocked slot via Coin vs Robux | Verify unlock and enter identical build seeded run | Same equipment power and slot count; only ledger/payment differ | NOT RUN |
