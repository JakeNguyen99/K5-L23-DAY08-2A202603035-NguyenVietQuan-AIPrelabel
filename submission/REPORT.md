# Báo cáo Day 08 — 2A202603035 - Nguyễn Việt Quân

> Một file duy nhất cho cả bài (đã gộp recipe, QC và ghi chú cá nhân). Mọi con số kèm **mẫu số** và nguồn (file nào). Viết bằng lời của bạn.

## 1. Model và recipe (Part 2–3)

| Lớp | Model + cỡ | Ngưỡng | Vì sao (dẫn `run_id` trong `model_log.csv`) |
|---|---|---|---|
| person / bicycle / motorbike / car / bus / truck | YOLO26m | person=0.45,motorbike=0.35, bicycle=0.3,truck=0.3,bus=0.3,car=0.3 |  r02, r03|
| three_wheeler | YOLOE-26s | three_wheeler=0.15 | r04 |

- Lệnh `merge_prelabels.py` đã dùng (dán nguyên): python scripts/merge_prelabels.py work/prelabel/yoloe_all/pre.coco.json work/prelabel/y26_all/pre.coco.json --thr "person=0.45,motorbike=0.35,three_wheeler=0.15" --conf 0.3 --iou 0.6 --split "batch,gold" --out work/prelabel/merged_review/pre.coco.json
- **Phiên bản SAM bạn chọn** (SAM 1 / 2.1 / 3) và lý do: <<chất lượng mask, tốc độ, RAM, quyền truy cập>> SAM 2.1, chất lượng mask tốt, tốc độ 10.13 giây/ảnh, RAM 16gb
- Lỗi nào **không sửa được bằng đổi ngưỡng** (ví dụ tuk-tuk bị gọi `car`)? Sửa thế nào? Lỗi xe tuk-tuk bị gán `car` hoặc `truck`. Sửa bằng cách dùng model có khả năng nhận diện lớp three_wheeler.
- Box **điểm cao mà sai** hoặc **điểm thấp mà đúng** (ảnh nào): img_032.jpg, box bicycle 0.7 sai nhưng conf cao
- Giới hạn: chọn ngưỡng trên chính gold (4 ảnh) nên số đo máy trên gold là lạc quan: Bởi vì 4 ảnh mẫu nhỏ, lớp trong 4 ảnh có ít hoặc không có nên số đo riêng cho lớp đó không đáng tin cậy dẫn đến việc lạc quan khi áp dụng trên dữ liệu mới.

## 2. AI error profile (từ `qc/review_stats.md`)

| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---|---|---|
| accepted | 422 | 422/444 = 95.0% | 444 box máy |
| edited | 2 | 2/444 = 0.5% | 444 box máy |
| deleted | 18 | 18/444 = 4.1% | 444 box máy |
| added | 9 | 9/433 = 2.1% | 433 đối tượng sau duyệt |

- Lớp nào máy tệ nhất? Vì sao (dẫn ảnh)? Lớp Three_wheeler, vì lớp này thường xuyên bị gán lệch, thừa, sai vật thể
- So với gold: precision / recall của **máy** 60.7% (KTC 95%: 48–72%)/94.9% (KTC 95%: 83–99%)

## 3. Thiên kiến tự động hóa và ảnh bẫy

- Bắt bẫy: 8/12; loại nào lọt: loose 1/3, wrong_class 3/3, extra 3/3, missing 1/3; vì sao (bạn đã bỏ bước nào của checklist)? bỏ sót checklist bước 1 và 4
- Accepted cao mà bắt bẫy thấp (hoặc ngược lại) nói gì về cách bạn duyệt? Cách tôi duyệt vẫn chưa được sát theo checklist.
- Re-review: Bắt bẫy: 12/12, loose 3/3, wrong_class 3/3, extra 3/3, missing 3/3

## 4. Máy có đáng dùng không?

- Thời gian (từ `time_log.csv`): gold_manual = 25p, batch_review = 105p.
- Số liệu tham chiếu: accepted 422/444 = 95.0%; deleted 18/444 = 4.1%; added 9/433 = 2.1%; bắt bẫy lượt đầu 8/12 = 66.7% (sau re-review 12/12).
- Kết luận: Có thể dùng pre-label làm điểm khởi đầu, không chấp nhận tự động; vẫn cần review toàn bộ vì lượt đầu bắt bẫy chỉ 8/12 và mẫu QC có lỗi ở 2/5 ảnh.
- **Ba quy tắc giảm thiên kiến của bạn** (mỗi quy tắc kiểm tra được bằng số hoặc việc làm):
  1. Ẩn nhãn máy và quét toàn ảnh ít nhất 15 giây trước khi hiện nhãn ở mọi ảnh; mục tiêu thực hiện đủ 24/24 ảnh.
  2. Với từng ảnh, làm đủ thứ tự: tìm vật bị bỏ sót → xóa box thừa → sửa lớp → chỉnh box lỏng; không bỏ qua bước nào.
  3. Không dùng confidence để tự động chấp nhận box; kiểm tra lớp và độ ôm sát của từng box bằng ảnh gốc.

## 5. Mask SAM cho 3 vật (Part 5)

| Ảnh | SAM (phiên bản) | Số click | Mask gồm người lái / hàng hoá? | Lỗi quan sát | File trong `masks/` |
|---|---|---|---|---|---|
| img_004 | SAM 2.1 | 3 |  | Có chồng lấn lên người | tuktuk_img004_sam2.jpg |
| img_013 | SAM 1 | 3 | Mask gồm người lái | Có chồng lấn lên người | tuktuk_img013_sam1.jpg |
| img_013 | SAM 2.1 | 3 | Mask gồm người lái | Có chồng lấn lên người | tuktuk_img013_sam2.jpg |
| img_017 | SAM 2.1 | 3 |  | Mask không hết phần trần | tuktuk_img017_sam2.jpg |


- Đối chiếu guideline mục 4.2: quy tắc nào giúp, quy tắc nào thiếu? <<>>
- *(Tuỳ chọn)* Interactor SAM trong CVAT: làm được / không (lỗi gì, đã thử gì); khác gì offline: <<>>

## 6. Calibration (Part 6)

- Số quyết định: 4 (`calibration_decisions.csv`, CAL-01–CAL-04); quan trọng nhất: CAL-04, chốt cách gán xe đạp điện/xe máy điện.
- Quy tắc mới đã vào `guideline_v1.1_calibrated.md`: xe ba bánh có mui vẫn là three_wheeler, xe đạp điện và xe máy điện vẫn là motorbike
- Nguyên nhân gốc: Guideline không đề cập đến xe đạp điện, xe máy điện, xe ba bánh có mui.

## 7. QC và quyết định (Part 6)

- Mẫu: 5 ảnh (0 ảnh `can_review` + 5 ảnh ngẫu nhiên, seed 42); 95 đối tượng; người kiểm: bạn (**hạn chế**: bạn vừa là người duyệt).
- Lỗi theo **đối tượng**: 2/95 = 2.1% · theo **ảnh**: 2/5 = 40.0% (ngưỡng Release ≤ 2%)
- Top lỗi: `thua` 1 ảnh (`img_003.jpg`); `bo_sot` 1 ảnh (`img_006.jpg`).
- **Giới hạn mẫu nhỏ** (cận trên 95% trong `qc_report_auto.md`): 77%; mẫu 5 ảnh không đủ chứng minh tỉ lệ lỗi thật đạt ngưỡng 2%.
- Script kiểm được / không kiểm được: script tổng hợp phiếu QC; không tự xác định box có ôm sát vật, nhãn đúng thực tế, vật cả bạn lẫn máy cùng bỏ sót, hay bạn có theo checklist hay không.

**Quyết định: REWORK**,  Lý do: phiếu QC ghi lỗi ở 2/5 ảnh (40.0%), cao hơn ngưỡng 2%; `qc_report_auto.md` cũng gợi ý Rework. Mẫu nhỏ nên tỷ lệ này còn bất định, nhưng không đủ căn cứ để Release.
Việc cần làm tiếp: sửa lỗi đã ghi ở `img_003.jpg` và `img_006.jpg`, cập nhật nhãn cuối, rồi chạy lại `build_release.py`, `qc_sample.py --force` và `qc_report.py`; ghi riêng kết quả QC vòng sau.

## 8. Ghi chú cá nhân

- **Dùng AI để làm bài** (công cụ nào, việc gì, kiểm tra thế nào; "không dùng" nếu không): Copilot kiểm tra bằng cách đọc lại và sửa lỗi chính tả
- Gold có bị nhiễm không (bạn có nhìn pre-label hay hỏi AI trước khi gán xong)? không
- Máy / GPU / CVAT dùng; vướng mắc cài đặt: không
- Dùng cache `data/cache/` thay vì tự chạy? không
- Lỗi / bug script đã gặp (nếu có): 
- Một điều bạn làm khác lần sau: 
