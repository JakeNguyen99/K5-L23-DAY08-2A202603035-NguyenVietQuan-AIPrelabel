# Số đo review (det)

- Ảnh tính: 24 (batch,gold); box máy: 444; đối tượng sau duyệt: 433; ngưỡng accepted: IoU >= 0.9
- Đã **loại 22 đối tượng nằm quanh các bẫy** khỏi bảng này: lỗi nhân tạo không phản ánh chất lượng máy.

## 1. Bốn trạng thái (AI error profile)
| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---|---|---|
| accepted (giữ nguyên) | 422 | 422/444 = 95.0% | 444 box máy |
| edited (sửa) | 2 | 2/444 = 0.5% | 444 box máy |
| deleted (xoá) | 18 | 18/444 = 4.1% | 444 box máy |
| added (vẽ thêm) | 9 | 9/433 = 2.1% | 433 đối tượng sau duyệt |

Trong số deleted + added, **5 trường hợp là đổi lớp** (máy gán sai lớp, box đúng chỗ): đó không phải máy bỏ sót. Số vật máy **sót thật** ≈ added − đổi lớp = 4.

### Theo lớp
| Lớp | accepted | edited | deleted | added |
|---|---|---|---|---|
| person | 203 | 0 | 0 | 2 |
| bicycle | 7 | 0 | 1 | 1 |
| motorbike | 130 | 0 | 0 | 1 |
| car | 51 | 0 | 2 | 2 |
| bus | 2 | 0 | 1 | 1 |
| truck | 4 | 0 | 2 | 1 |
| three_wheeler | 25 | 2 | 12 | 1 |

## 2. So với gold (nhãn gán tay từ đầu)
Ảnh gold: 4; đối tượng gold: 39 (mẫu số của recall)
| | TP | FP | FN | Precision | Recall |
|---|---|---|---|---|---|
| Máy (pre-label) | 37 | 24 | 2 | 60.7% (KTC 95%: 48–72%) | 94.9% (KTC 95%: 83–99%) |

*Lạc quan: ngưỡng được chọn trên chính các ảnh gold này (in-sample), và chỉ có 4 ảnh. Đọc khoảng tin cậy, đừng đọc con số đơn.*


(Ảnh gold không nằm trong nhãn sau duyệt: không tính residual / new error. Đó là chủ ý: duyệt lại ảnh bạn vừa tự gán tay sẽ đo trí nhớ, không đo thiên kiến.)

## 3. Ảnh bẫy
- Bắt được 8/12 = 66.7% (KTC 95%: 39–86%) (ngưỡng đạt: 80%). Chỉ 12 bẫy nên khoảng tin cậy rất rộng: đọc cùng danh sách bẫy lọt, đừng kết luận chỉ từ %.
  - loose: 1/3
  - wrong_class: 3/3
  - extra: 3/3
  - missing: 1/3
  - lọt: img_034.jpg (loose), img_025.jpg (missing), img_003.jpg (missing), img_023.jpg (loose)

## Gợi ý đọc số
- `deleted` cao → ngưỡng thấp hoặc ánh xạ nhãn sai; `added` cao → máy bỏ sót (xem lớp nào);
- `accepted` thấp (< 50%) → sửa mỗi box lâu hơn vẽ mới: đổi model/ngưỡng hoặc vẽ tay;
- bắt bẫy < 80% → duyệt lại lô, siết checklist (quét ảnh TRƯỚC khi nhìn nhãn máy).
