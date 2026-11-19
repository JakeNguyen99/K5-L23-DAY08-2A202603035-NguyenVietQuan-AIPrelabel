# Thông tin học viên

- Họ và tên: Nguyễn Việt Quân
- MSHV: 2A202603035
- Lớp (L2 / L3 / khác): L2
- Tài khoản GitHub: JakeNguyen99
- Repo bài nộp (URL): https://github.com/JakeNguyen99/K5-L23-DAY08-2A202603035-NguyenVietQuan-AIPrelabel
- Commit chốt bài (hash): 9097d6ba36ad59d670bad58b24f4fe69065e7de9
- Máy: Window 10, GTX1050
- CVAT (Docker): v2.7

## Kết quả tóm tắt (điền cuối bài; số lấy từ `submission/qc/*`, kèm mẫu số)

- Máy: accepted 422/444 = 95.0%; edited 2/444 = 0.5%; deleted 18/444 = 4.1%; added 9/433 = 2.1%.
- So với gold (4 ảnh, 39 đối tượng): precision / recall của **máy** 60.7% / 94.9% (KTC 95% lần lượt: 48–72% / 83–99%).
- Bắt bẫy: lượt đầu 8/12 = 66.7%; sau khi xem key và re-review 12/12 = 100.0%.
- Thời gian: gán tay 25/4 = 6.25 phút/ảnh; duyệt có pre-label 105/24 = 4.38 phút/ảnh (time_log đã cập nhật sau re-review).
- Lỗi QC: 40.0% theo ảnh (2/5); 2.1% theo đối tượng (2/95). Mẫu nhỏ; cận trên Wilson 95% theo ảnh là 77%.
- **Quyết định:** Rework — lỗi xuất hiện ở 2/5 ảnh QC, cao hơn ngưỡng 2%; cần sửa lỗi và kiểm tra lại.
