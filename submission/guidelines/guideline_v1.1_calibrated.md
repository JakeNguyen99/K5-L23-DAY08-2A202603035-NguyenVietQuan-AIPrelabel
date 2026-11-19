# Label contract — Phố châu Á — BỔ SUNG v1.1 — 2A202603035 - Nguyễn Việt Quân

> **Chỉ viết phần thay đổi so với v1.0** (không copy cả file). Chốt các câu hỏi mở ở mục 6 của v1.0 (hoặc nói rõ câu nào chưa chốt, vì sao)
> và thêm quy tắc mới rút ra từ lỗi của máy / lỗi của bạn. Mỗi ca khó **có ảnh ví dụ** (`img_0xx`) và khớp một dòng trong `qc/calibration_decisions.csv`.

## 1. Câu hỏi mở đã chốt (từ mục 6 của v1.0)

| # | Câu hỏi | Quyết định | Ảnh ví dụ | `case_id` |
|---|---|---|---|---|
| 1 | xe đạp điện/xe máy điện: bicycle hay motorbike? | motorbike | img_025 | CAL-04 |

## 2. Ca khó mới (từ lỗi của máy, bẫy lọt, box `can_review`)

| Ảnh | Tình huống | Máy làm gì (model nào) | Quyết định của bạn | Quy tắc mới (vào mục nào của v1.0) |
|---|---|---|---|---|
| img_013,img_004 | xe tuk-tuk bị gán là là car | YOLO26 gọi tuk-tuk là car | luôn gán three_wheeler | xe ba bánh có mui vẫn là three_wheeler |

## 3. Lịch sử thay đổi

| Phiên bản | Thay đổi | Lý do (ca nào, `case_id`) |
|---|---|---|
| v1.0 | Bản giảng viên | — |
| v1.1 | Cập nhật lớp motorbike và three_wheeler | Guideline chưa đề cập tới xe đạp điện/xe máy điện và xe tuk-tuk bị nhận diện nhầm |
