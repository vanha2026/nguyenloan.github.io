# Quản lý & Giao đề thi tự học

Ứng dụng một tệp (`index.html`) dành cho giáo viên quản lý đề thi, giao đề cho học sinh, chấm và xem kết quả.
Bản này là **bản sạch**: không chứa đề, lớp, học sinh, bài làm, kết quả, mật khẩu hay cấu hình máy chủ nào.

## Đưa lên GitHub Pages
1. Tạo repository mới (ví dụ `de-thi`), tải lên 3 tệp: `index.html`, `.nojekyll`, `README.md`.
2. Vào **Settings → Pages → Build and deployment**: Source = *Deploy from a branch*, Branch = `main`, thư mục `/ (root)`.
3. Sau 1–2 phút trang chạy tại `https://<tên-tài-khoản>.github.io/de-thi/`.

> Tệp phải tên đúng là `index.html`. Học sinh cần có mạng (ứng dụng tải Tailwind, KaTeX, Excel, QR... từ CDN).

## Dữ liệu nằm ở đâu
- Đề thi, lớp học, kết quả, cấu hình được lưu **trên trình duyệt** (localStorage / IndexedDB) của từng máy.
- Muốn giáo viên và học sinh thấy chung dữ liệu: dùng nút ☁️ **Đồng bộ** trong ứng dụng, nhập địa chỉ Firebase Realtime Database của bạn.
  Sau khi kết nối, nên tải `cloud-config.json` từ ứng dụng và đặt cạnh `index.html` để mọi máy tự nhận đúng kho.

## Làm sạch dữ liệu
Dữ liệu cũ nằm ở **3 nơi**, cần xử lý đủ cả 3 thì ứng dụng mới sạch hoàn toàn:
1. **Trình duyệt (từng máy):** bản này tự xóa dữ liệu cũ **một lần** khi mở lần đầu trên mỗi trình duyệt (đề, lớp, kết quả học sinh, tài khoản, kết nối máy chủ), sau đó giữ dữ liệu mới bình thường. Muốn xóa lại: mở `.../index.html?reset=1`. Chỉ xóa dữ liệu của ứng dụng này, không ảnh hưởng trang GitHub Pages khác cùng tên miền.
2. **Repository GitHub:** nếu có tệp `cloud-config.json` hoặc `index.html` cũ có nhúng kết nối, hãy **xóa `cloud-config.json`** khỏi repository. Nếu còn, mọi máy sẽ tự nối lại kho Firebase cũ và tải kết quả cũ về.
3. **Firebase:** vào Firebase Console → Realtime Database → xóa các nhánh dữ liệu cũ (hoặc tạo database mới). Ứng dụng không tự xóa phía máy chủ.

## Đã chỉnh trong bản này
- Rà soát: cú pháp toàn bộ 22 khối mã, biến chưa khai báo, ID trùng, hàm gọi từ nút bấm (97 nút trong HTML + 136 nút tạo động) — không phát hiện lỗi chạy.
- Gỡ ~118 chú thích lịch sử bản, thẻ `<meta>`/`<link>` bị lặp, thẻ `Cache-Control` vô tác dụng, một đoạn mã chết.
- Tự dọn dữ liệu cũ một lần trên mỗi trình duyệt, thêm `?reset=1`, thông báo khi trình duyệt tắt JavaScript.
- Sửa: xóa đề trong Thư viện thì xóa luôn kết quả học sinh của đề đó; trang chủ giáo viên không còn hiện "Học sinh tiêu biểu" / "Hoạt động gần đây" của đề đã xóa (trước đây hiện "Đề đã xóa").
