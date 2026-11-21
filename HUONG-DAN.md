# Tập viết Hán – bản iPhone (build bằng Codemagic, IPA chưa ký)

Bản này đã có đủ tính năng như bản Windows: khoá học 16 bài (ngữ pháp, hội thoại), HSK 2.0, HSK 3.0, 30.000 từ theo tần suất,
tra cứu ~115.000 mục, ôn ngắt quãng, flashcard, trắc nghiệm 4 đáp án, nhập/xuất danh sách từ. Chạy offline, tiến độ lưu trong máy.

Thiết bị mục tiêu: iPhone XS Max (chip A12, arm64) chạy iOS 18 (đây là bản iOS cuối cùng XS Max hỗ trợ).
Dự án đặt iOS tối thiểu 15.0 nên chạy tốt trên iOS 18; Xcode 26 chỉ dùng để build, không ép máy phải lên iOS 26.

## 1. Đưa code lên Git (GitHub, repo Private cũng được)
Trong thư mục này:
```
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin https://github.com/TEN-BAN/TEN-REPO.git
git push -u origin main
```
(Không dùng nút “Upload files” trên web vì có hơn 7.000 file.)

## 2. Build trên Codemagic
1. Vào codemagic.io, đăng nhập bằng GitHub.
2. **Add application** > chọn repo > loại dự án **Other / codemagic.yaml** (không chọn Flutter hay Ionic).
3. Mở file `codemagic.yaml`, sửa `BUNDLE_ID` thành App ID trong provisioning profile của bạn (hoặc để nguyên và để công cụ ký ghi đè).
4. **Start new build** > chọn workflow `ios-unsigned` > nhánh `main`.
5. Chờ khoảng 10 đến 15 phút > tải `TapVietHan-unsigned.ipa` ở mục Artifacts (lưu khoảng 30 ngày).

## 3. Tự ký và cài
Dùng công cụ ký IPA của bạn với chứng chỉ `.p12` và `.mobileprovision`.
- Bundle ID trong IPA phải khớp App ID của profile (hoặc đặt lại bằng tuỳ chọn ghi đè Bundle ID của công cụ ký).
- Profile Development: UDID của XS Max phải nằm trong profile, và bật Chế độ nhà phát triển trên iPhone
  (Cài đặt > Quyền riêng tư & Bảo mật > Chế độ nhà phát triển).
- Lần đầu mở nếu báo “nhà phát triển không tin cậy”: Cài đặt > Cài đặt chung > Quản lý VPN & thiết bị > Tin cậy.

## Bản này (v3)
- **Giao diện bo tròn, nhẹ:** thanh tab nổi dạng viên thuốc, nút tròn, thẻ bo góc; chỉ dùng CSS, không thêm ảnh hay thư viện.
- **Giao diện và hình nền** (tab Của tôi): chế độ Tự động/Sáng/Tối, 4 màu nhấn, 6 nền gradient hoặc chọn ảnh từ máy (ảnh tự thu nhỏ để nhẹ), thanh chỉnh độ rõ của nền.
- **Tra từ làm màn hình chính**, kiểu Hanzii: ô tìm kiếm lớn, danh sách “Tra gần đây”.
- **✍ Viết tay để tra:** viết chữ lên khung, app đoán chữ, bấm chữ đúng để thêm vào ô tìm.
- **部 Tra theo bộ thủ:** 216 bộ, có tên Hán Việt, xếp theo số nét.
- **Từ ghép:** mỗi chữ hiện các từ chứa nó (ưu tiên từ HSK).
- Icon mới.
- Dung lượng: dữ liệu trong app khoảng 28 MB (file IPA nén còn nhỏ hơn nhiều), dưới giới hạn 50 MB.

## Tạo repo mới
Giải nén file zip, mở GitHub Desktop: File > Add local repository (hoặc Create new repository) chọn đúng thư mục có `codemagic.yaml`, commit, Publish. Rồi nối repo mới với Codemagic như lần trước.
Trong `codemagic.yaml`, `BUNDLE_ID` đang là App ID của profile bạn gửi trước đó. Nếu ký lại bằng Apple ID miễn phí thì công cụ ký sẽ tự đổi, không cần sửa.

## Gì mới ở bản trước
- Ô viết chữ lớn gần hết bề ngang màn hình, màn hình không còn bị nhảy khi mở từ, bấm nút hay chuyển màn.
- Tập viết rộng rãi hơn với nét vẽ dày và dễ chấp nhận hơn (hợp ngón tay và bút cảm ứng).
- Tab **Luyện viết** mới: vuốt lên xuống như TikTok, mỗi lần một chữ. Chữ tự chạy nét mẫu, chạm vào ô để tự viết, viết đúng sẽ tự sang chữ tiếp theo, có chuỗi đúng liên tiếp. Nguồn chữ chọn ở thanh trên (bài học, HSK, từ đang ôn, của tôi).

## Lưu ý
- Bấm **Nghe đọc** dùng giọng tiếng Trung của iOS. Nếu im lặng, kiểm tra công tắc im lặng bên hông máy và âm lượng.
- Xuất danh sách: máy mở bảng chia sẻ của iOS nếu hỗ trợ, nếu không sẽ hiện hộp “Sao chép” để dán vào Ghi chú hoặc Tệp.
- Tiến độ lưu bằng UserDefaults của iOS (có dự phòng localStorage). Gỡ app sẽ mất tiến độ.
- Nếu Codemagic báo sai tên máy `mac_mini_m2` hoặc phiên bản Xcode/Node, đổi trong `codemagic.yaml` theo danh sách hiện tại của Codemagic.
- Thư mục `.github/workflows` là phương án dự phòng dùng GitHub Actions, có thể xoá.
