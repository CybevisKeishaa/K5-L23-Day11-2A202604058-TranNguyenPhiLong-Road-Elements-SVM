# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): phamphunguyenhung (2A202604062)        Người kiểm (học viên): Trần Nguyễn Phi Long (2A202604058)  
Ngày: 21/11/2026                          Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `phamphunguyenhung-front.zip`, `phamphunguyenhung-fisheye.zip`, `phamphunguyenhung-manifest.json` · `revision` 1 (sha256 hai file zip khớp manifest)  
Số ảnh kiểm: 6 (+ 0 ảnh thêm)   Số nhãn đã kiểm (shape + tag): 67 (43 shape + 24 tag)  

Sáu ảnh kiểm: A01, A04, A07, A08 (front), B03 (fisheye_rear), B05 (fisheye_left). Chọn theo các điểm CHECKPOINTS yêu cầu: vạch đứt, vàng đôi và drivable direct/alternative (A04), đèn (A07–A08), fisheye trước/sau và camera hai bên.  
Cách kiểm: mở XML, vẽ overlay từng nhãn (kèm id, position, style) lên ảnh gốc trong `data/`, phóng to chỗ khó nhìn; thêm `road_check lint` cho cả lô.  
Số nhãn theo ảnh: A01 6 + 4 tag · A04 2 + 4 · A07 2 + 4 · A08 9 + 4 · B03 18 + 4 · B05 6 + 4.  
Id nhãn là thứ tự của phần tử trong `<image>` của `annotations.xml`, đếm từ 0, tính cả tag.

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 6 (tag camera, 6 ảnh) | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 5 (ảnh front/rear: A01, A04, A07, A08, B03) | 2 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 16 (lane) | 12 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 16 (lane) | 1 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 16 (lane) | 1 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 16 (lane) | 1 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 13 (drivable, crosswalk) + 6 ảnh xem có thiếu | 5 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 2 (box đèn) + đèn có trong 6 ảnh | 10 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 9 (box đèn, biển) | 3 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 7 (box biển) + biển có trong 6 ảnh | 7 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 16 (7 polyline fisheye + 9 box `edge_zone`) | 0 | chính |
| QC1-12 file | lỗi file | 3 (2 zip + manifest) | 0 | nhẹ / nặng |

## Danh sách lỗi (mỗi lỗi một dòng)
| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-03 | A01 | lane #0 | nặng | Vạch đứt trắng sát bên phải làn xe đang đi, gán `L1`, đúng là `R1` (vì vậy ảnh không có R1) |
| 2 | QC1-03 | A01 | lane #4 | nặng | Vạch liền trắng ở mép trái đường, gán `L1`, đúng là `L2` (L1 thật là lane #5) |
| 3 | QC1-03 | A01 | lane #1 | nặng | Vạch liền trắng ở mép phải sát lan can, gán `L1`, đúng là `R2` |
| 4 | QC1-07 | A01 | drivable #3 | chính | `direct` phủ cả làn đang đi lẫn làn bên phải (qua vạch lane #0 tới lan can); phần bên phải lane #0 phải là `alternative` |
| 5 | QC1-10 | A01 | traffic_sign (thiếu) | chính | Bỏ sót biển chỉ dẫn xanh lá ~(551,408)–(604,481), cao 73 px |
| 6 | QC1-10 | A01 | traffic_sign (thiếu) | chính | Bỏ sót biển xanh nhỏ ~(432,462)–(450,482), cao 20 px |
| 7 | QC1-02 | A04 | lane L1 (thiếu) | nặng | Không vẽ vạch vàng đôi bên trái làn xe (~(50,545) → (545,330)); vạch này rõ, phải là L1, `yellow / solid / double` |
| 8 | QC1-03 | A04 | lane #0 | nặng | Vạch đứt trắng bên phải làn xe, gán `L1`, đúng là `R1` |
| 9 | QC1-03 | A04 | lane #1 | nặng | Vạch liền trắng mép làn xe đạp xanh, gán `L1`, đúng là `R2` |
| 10 | QC1-06 | A04 | lane #1 | chính | `kind = double` nhưng là một vạch đơn → `single` |
| 11 | QC1-07 | A04 | drivable (thiếu) | chính | Ảnh không có polygon drivable; cần `direct` cho làn đang đi (giữa vàng đôi và lane #0) và `alternative` cho làn bên phải |
| 12 | QC1-03 | A07 | lane #1 | nặng | Vạch liền trắng sát bên phải làn xe, gán `L1`, đúng là `R1` (bên trái là dải bê tông, không có L1) |
| 13 | QC1-03 | A07 | lane #0 | nặng | Vạch liền trắng cạnh xe tải, gán `L1`, đúng là `R2` |
| 14 | QC1-07 | A07 | drivable (thiếu) | chính | Ảnh không có polygon drivable |
| 15 | QC1-08 | A07 | traffic_light (thiếu) | chính | Bỏ sót đèn xa phía trước ~(661,229)–(668,239), cao ~10 px, đang xanh |
| 16 | QC1-02 | A08 | lane L1 (thiếu) | nặng | Không vẽ vạch đứt trắng (mờ) bên trái làn xe ~(258,423) → (495,312); biên trái của drivable #5 đi theo đúng vạch này |
| 17 | QC1-03 | A08 | lane #1 | nặng | Vạch đứt trắng bên phải làn xe, gán `L1`, đúng là `R1` |
| 18 | QC1-03 | A08 | lane #7 | nặng | Vạch liền trắng phía phải, gán `L1`, đúng là `R2` |
| 19 | QC1-03 | A08 | lane #8 | nặng | Vạch liền trắng phía trái (ngoài L1 thật), gán `L1`, đúng là `L2` |
| 20 | QC1-03 | A08 | lane #2 | nặng | Vạch trắng sát mép trái đường, gán `L1`; nằm ngoài L2 nên không phải L1 |
| 21 | QC1-03 | A08 | lane #0 | nặng | Đoạn đứt nối tiếp lane #8 về phía xa, gán `L1`; đây là cùng vạch với lane #8 nên không phải L1 |
| 22 | QC1-05 | A08 | lane #3 | chính | Đoạn xa của vạch R1 (lane #1) bị vẽ thành polyline riêng → nối vào lane #1 thành một polyline |
| 23 | QC1-07 | A08 | crosswalk (thiếu) | chính | Bỏ sót vạch ngựa vằn ngay trước xe (y ≈ 420–540, hết chiều ngang ảnh) |
| 24 | QC1-08 | A08 | traffic_light (thiếu) | chính | Bỏ sót đèn ~(360,95)–(375,127), đang xanh, facing front |
| 25 | QC1-08 | A08 | traffic_light (thiếu) | chính | Bỏ sót đèn ~(670,150)–(683,187), đang xanh, facing front |
| 26 | QC1-08 | A08 | traffic_light (thiếu) | chính | Bỏ sót đèn ~(765,163)–(775,198), đang xanh, facing front |
| 27 | QC1-08 | A08 | traffic_light (thiếu) | chính | Bỏ sót đèn xa ~(648,212)–(657,220), cao 8 px, đang xanh |
| 28 | QC1-08 | A08 | traffic_light (thiếu) | chính | Bỏ sót đèn đi bộ ~(697,240)–(710,253), `type = pedestrian` |
| 29 | QC1-08 | A08 | traffic_light (thiếu) | chính | Bỏ sót đèn quay lưng ~(887,62)–(920,128), `facing = back` |
| 30 | QC1-08 | A08 | traffic_light (thiếu) | chính | Bỏ sót đèn nhìn nghiêng ~(870,145)–(892,205), `facing = side` |
| 31 | QC1-08 | A08 | traffic_light (thiếu) | chính | Bỏ sót đèn nhìn nghiêng ~(202,102)–(222,142), `facing = side` |
| 32 | QC1-08 | A08 | traffic_light (thiếu) | chính | Bỏ sót đèn nhìn nghiêng ~(292,138)–(306,163), `facing = side` |
| 33 | QC1-10 | A08 | traffic_sign (thiếu) | chính | Bỏ sót biển chỉ dẫn xanh lá "Harlem River Dr / RFK Br" ~(752,62)–(880,128) |
| 34 | QC1-10 | A08 | traffic_sign (thiếu) | chính | Bỏ sót biển "Passenger cars only" phía trên ~(778,38)–(855,63) |
| 35 | QC1-10 | A08 | traffic_sign (thiếu) | chính | Bỏ sót biển xanh nhỏ ~(701,183)–(715,198) |
| 36 | QC1-10 | A08 | traffic_sign (thiếu) | chính | Bỏ sót biển xanh nhỏ ~(433,185)–(450,198) |
| 37 | QC1-04 | B03 | lane #4 | chính | Polyline dừng ở (785,362), trong khi vạch đứt còn chạy theo cung tới gần xe (~(990,675)); biên phải của drivable #6 đã đi hết vạch này |
| 38 | QC1-09 | B03 | traffic_sign #10 | chính | Biển chữ nhật xanh dương, gán `group = prohibitory`, đúng là `information` |
| 39 | QC1-09 | B03 | traffic_sign #11 | chính | Biển tròn xanh mũi tên, gán `prohibitory`, đúng là `mandatory` |
| 40 | QC1-09 | B03 | traffic_sign #12 | chính | Biển tròn xanh mũi tên, gán `prohibitory`, đúng là `mandatory` |
| 41 | QC1-10 | B03 | traffic_sign (thiếu) | chính | Bỏ sót biển tròn xanh "đi bên phải" trên cột vàng ~(1132,361)–(1156,400), cao ~39 px |
| 42 | QC1-07 | B05 | drivable #1 | chính | Polygon `direct` nằm trên vỉa hè phía trên và dải đảo/vỉa hè bên phải (giữa road_curb #3 và #4), không phải mặt đường → xóa |

Ảnh không lỗi: không có ảnh nào trong 6 ảnh kiểm sạch lỗi. Riêng tag thì cả 6 ảnh đều đúng (camera, timeofday, weather, source).

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| B03 | traffic_light #16, #17 gán `color = red`, `facing = front`. Ở độ phân giải này vỏ đèn có vẻ nhìn nghiêng/quay lưng, không thấy mặt đèn. Nên là `facing = side`, `color = unknown`? Nếu đúng là đèn quay về xe và không đỏ thì thành lỗi nặng QC1-08. | (chưa có) |
| B03 | Ảnh camera lùi (fisheye_rear): L1/R1 tính theo trái/phải của ảnh hay theo chiều xe? Bạn gán vạch đứt bên phải ảnh là `L1`, vạch xa hơn là `R1` (#13). Guideline mục fisheye không nói rõ. | (chưa có) |
| B05 | Camera bên trái (fisheye_left) thấy làn bên cạnh và vạch qua đường: drivable #2 nên là `direct` hay `alternative`? | (chưa có) |

## Quyết định
Lỗi nặng: 14   Lỗi chính: 28 / 67 nhãn = 41,8 %   Lỗi nhẹ: 0   Ca chờ lead: 3  
Quyết định (Release / Hold / Rework): **Rework**  
Lý do (theo luật của phiếu): Còn 14 lỗi nặng (2 × QC1-02, 12 × QC1-03), một lỗi nặng đã đủ để Rework. Tỉ lệ lỗi chính 28 / 67 = 41,8 % cũng vượt 5 %; nếu cộng thêm 20 nhãn bị bỏ sót vào mẫu số (28 / 87 = 32,2 %) thì vẫn vượt. Ba ca mơ hồ không đổi kết luận, nên không ra Hold.

## Ghi chú cho bạn (không chấm)
- **Lỗi lặp nhiều nhất là `position`.** Trong cả lô 16 ảnh front, 39/42 lane có `position = L1` và không lane nào là `R1`; L1 đúng là giá trị CVAT tự điền khi vẽ polyline mới. Vì vậy lint báo "chưa có R1" ở cả 16/16 ảnh front. Cách sửa nhanh: trong mỗi job lọc theo `position == "L1"`; mỗi ảnh chỉ được có đúng một L1 và một R1 (vạch sát hai bên xe), vạch kế tiếp ra ngoài là L2/R2.
- **Ảnh front không có đèn và biển nào** (manifest: front chỉ có drivable, lane, road_curb). A07–A08 có đèn, A01 và A08 có biển chỉ dẫn lớn. Cần gán mọi cụm cao từ 8 px.
- **Thiếu drivable ở 9/16 ảnh front**: A03, A04, A05, A06, A07, A13, A14, A15, A16.
- Không có lane nào bật `occluded`. Theo CHECKPOINTS, A03 có vạch bị che: bạn xem lại.
- File: mở được, `contains_images = false`, 28/28 ảnh đủ 4 tag, sha256 khớp manifest. Phần file ổn.

---

# Kiểm lại sau Rework — bản sửa

Ngày: 21/11/2026  
File được kiểm: `phamphunguyenhung-front.zip` (sha256 `4fd60594…0cef`, XML export 21/11/2026 03:29), `phamphunguyenhung-fisheye.zip` (sha256 `0ac261be…df57`). **Chưa nhận manifest mới** (`--revision 2`, theo SUBMISSION.md), nên không đối chiếu được sha256.  
Số ảnh kiểm: 6, cùng 6 ảnh như lần 1   Số nhãn đã kiểm (shape + tag): 100 (76 shape + 24 tag)  
Số nhãn theo ảnh: A01 9 + 4 tag · A04 8 + 4 · A07 8 + 4 · A08 26 + 4 · B03 20 + 4 · B05 5 + 4. Id nhãn đếm theo XML mới.

## Đối chiếu 42 lỗi của lần 1
- **Đã sửa 42/42:** position L1/R1/L2/R2 (12 lỗi), thêm L1 ở A04 và A08, A04 `kind`, drivable ở A01, A04, A07, crosswalk A08, đèn và biển ở A01, A07, A08, nối R1 ở A08, kéo dài R1 ở B03, group biển ở B03, xóa drivable trên vỉa hè ở B05.
- **Ca mơ hồ lần 1:** bạn đã đổi đèn B03 thành `facing = side`, `color = unknown`, và đổi vạch B03 thành `R1`/`R2`. Hai ca này không còn cần hỏi lead. Ca B05 (`direct` ở camera bên) vẫn giữ.
- **Soát lại:** người kiểm mở lại trên CVAT và xem bằng mắt 9 nhãn đã nghi ngờ khi soát trên ảnh tĩnh (đèn A04 #7, A07 #2, A08 #19; drivable A01 #7, A07 #3, #4; biển A01 #4/#8, A04 #6, B03 #16). Cả 9 nhãn đều đúng, không ghi lỗi.

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 6 (tag camera, 6 ảnh) | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 5 (ảnh front/rear: A01, A04, A07, A08, B03) | 0 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 16 (lane) | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 16 (lane) | 0 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 16 (lane) | 0 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 16 (lane) | 0 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 16 (drivable, crosswalk) | 0 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 20 (box đèn) | 0 (+1 chờ lead) | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 39 (box đèn, biển) | 0 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 19 (box biển) | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 18 (7 polyline fisheye + 11 box `edge_zone`) | 0 | chính |
| QC1-12 file | lỗi file | 2 zip (manifest chưa nhận) | 1 | nhẹ |

## Danh sách lỗi (mỗi lỗi một dòng)
| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-12 | A08 | drivable #24 | nhẹ | Polygon có 1 điểm lặp liền nhau (lint báo, click hai lần): xóa điểm thừa |

Ảnh không lỗi: **A01, A04, A07, B03, B05**. A08 chỉ có lỗi nhẹ #1 và một ca chờ lead.

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| A08 | traffic_light #5 (428,228)–(441,244) đang sáng màu cam, gán `type = vehicle`, `color = unknown`, `facing = front`. Đây là đèn đi bộ (bàn tay, nên là `pedestrian`) hay đèn vàng của xe hướng về xe? Nếu là đèn xe thì `unknown` thay cho `yellow` là sai màu trên đèn hướng về xe, tức lỗi nặng QC1-08. | (chưa có) |
| B05 | Camera bên trái (fisheye_left): vùng đường thấy được nên là `direct` hay `alternative`? (giữ từ lần 1) | (chưa có) |

## Quyết định
Lỗi nặng: 0   Lỗi chính: 0 / 100 nhãn = 0 %   Lỗi nhẹ: 1   Ca chờ lead: 2  
Quyết định (Release / Hold / Rework): **Hold**  
Lý do (theo luật của phiếu): Không còn lỗi nặng; tỉ lệ lỗi chính 0 % dưới 5 %. Còn ca A08 #5 chưa có lời đáp, và ca này ảnh hưởng tới kết luận: nếu lead xác nhận đó là đèn xe hướng về xe thì thành lỗi nặng và lô chuyển sang Rework. Nếu là đèn đi bộ thì chỉ cần sửa `type` (mức chính, 1/100 = 1 %) và lô được Release. Chờ lead trả lời ca này rồi kết luận.

## Ghi chú cho lần sửa tới (không chấm)
- Bản sửa tốt hơn rất nhiều: lint từ 28 cảnh báo xuống 9; trong 6 ảnh mẫu, phần làn, vùng lái, đèn và biển đều đã đúng.
- Xóa điểm lặp ở A08 drivable #24.
- Ngoài mẫu, lint còn báo: thiếu R1 ở A09 và A14, hai polyline cùng `L2` ở A11. Bạn tự xem lại; nếu là ảnh không thấy vạch thì ghi lý do vào báo cáo.
- Khi nộp lại, nhớ gửi kèm manifest `--revision 2` và giữ bản cũ trong `r1/` theo SUBMISSION.md.
