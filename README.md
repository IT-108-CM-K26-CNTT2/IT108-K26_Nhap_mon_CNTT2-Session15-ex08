# Bài 8: Kiểm tra vì sao dữ liệu khách hàng CampusPay bị rò rỉ ra ngoài

## Phần 1: Phân tích lỗ hổng

**1. Lỗ hổng do phạm vi và quyền**

- **Cấu hình sai:** phạm vi là **Anyone with the link**, quyền cấp là **Editor**.
- **Vì sao nguy hiểm:** link dễ bị lan ra ngoài. Người được mời có link thì người bên ngoài cũng có thể có, và họ có thể sửa nội dung, gây hại cho chủ file.
- **Người lạ xin quyền, chủ file lỡ duyệt:** người lạ có link nên gửi yêu cầu cấp quyền truy cập. Chủ file nhận thông báo qua Gmail và bấm chấp nhận mà không để ý email người xin.

**2. Các đường rò rỉ khác**

- **File nằm trong thư mục dùng chung:** file được lưu trong thư mục đã chia sẻ sẵn cho nhiều người. Dù file chỉ gán riêng cho Email B, người lạ vẫn xem được do kế thừa quyền từ thư mục cha.
- **Email B chia sẻ tiếp cho người lạ:** người gửi không tắt tùy chọn cho phép người chỉnh sửa thay đổi quyền và chia sẻ, nên B có quyền mời thêm người khác.

**Cách thu hồi link cũ:**
1. Nhấp chuột phải vào file, chọn **Chia sẻ**.
2. Ở mục quyền truy cập chung, đổi từ **Anyone with the link** sang **Restricted**.
3. Nhấn **OK**. Mọi người có link cũ nhưng không nằm trong danh sách email được cấp quyền sẽ bị chặn.

**Cấu hình đúng cho từng file**

- `campuspay_beta_users.xlsx`: quyền **Editor**, phạm vi **Restricted**.
- `campuspay_api_keys.txt`: quyền **Viewer**, phạm vi **Restricted**.
- `slide_pitch_v9.pptx`: quyền **Commenter**, phạm vi **Anyone with the link**.

## Phần 2: Sửa lỗi và nghiệm thu

**3. Giải thích cấu hình**

- `campuspay_beta_users.xlsx`: chứa dữ liệu cá nhân của người dùng nên phải để **Restricted**. Chỉ người được mời qua email mới vào được, link không thể lan ra ngoài.
- `campuspay_api_keys.txt`: chứa nội dung cần bảo mật cao nên để **Restricted** để tránh người lạ chiếm quyền. Chỉ cấp **Viewer** vì file này chỉ cần xem, không cần sửa.
- `slide_pitch_v9.pptx`: là bài thuyết trình, không chứa dữ liệu nhạy cảm. Quyền **Commenter** cho phép người xem góp ý mà không sửa được nội dung gốc. Phạm vi **Anyone with the link** giúp giáo viên dễ xem và nhận xét.

**4. Khi nào dùng Set Expiration Date, khi nào thu hồi thủ công**

- **Dùng Set Expiration Date khi:**
  - Làm việc ngắn hạn với đối tác, ví dụ chia sẻ tài liệu cho khách hàng theo hợp đồng có thời hạn.
  - Cần tránh quên thu hồi quyền, ví dụ người rời dự án nhưng vẫn giữ quyền truy cập tài liệu nội bộ nhiều tháng sau đó.
- **Thu hồi thủ công khi:** xảy ra sự cố khẩn cấp như rò rỉ dữ liệu, link gửi nhầm nhóm hoặc nhân sự thôi việc đột xuất, cần ngắt truy cập ngay lập tức.
