# 🗺️ Kiến trúc Thông tin (IA), Luồng Người dùng (User Flows) & Bảng Ánh xạ Nghiệm thu

> **Dự án:** **FPT Alumni Connect** — Nền tảng kết nối Cựu sinh viên & Sinh viên Đại học FPT  
> **Môn học:** PRM323 — Cross-Platform Application Development (Lab 2: Google Stitch → Figma)  
> **Người thực hiện chính (Lead Author):** **Võ Quang Tâm** — MSSV: **SE192621** (Vai trò: **TV1 — UX Research, Persona & User Flow Lead**)  
> **Căn cứ Quy chuẩn:** Bộ chuẩn [PRM393 UI/UX Material](https://prm393-material.web.app/), Quality Gates P0–P5, 11 User Stories cốt lõi (`US-01` → `US-11`), 8 Màn hình cố định (`S01` → `S08`).  
> **Sở hữu Màn hình:** **`S01`** (Login & Google Auth) và **`S02`** (Verify Email & Reset Password).

---

## 1. Kiến trúc Thông tin Toàn diện (Information Architecture — IA 8 Màn hình)

Hệ thống được thiết kế theo cấu trúc phân tầng phân cấp di động tối ưu cho màn hình khổ dọc **360×800 dp**, gồm 3 tầng kiến trúc:
- **Tầng 0 (Authentication & Onboarding Stack):** Cổng xác thực danh tính bảo mật qua email `@fpt.edu.vn` và Google OAuth 2.0.
- **Tầng 1 (Main Navigation — Bottom Navigation Bar 4 Tabs):** Bốn trụ cột trải nghiệm chính (Home Feed, Member Directory, Chat Room, My Profile).
- **Tầng 2 (Detail Views, Modals & System Overlays):** Các màn hình chi tiết, trang cài đặt bảo mật và các hộp thoại xác nhận quan trọng.

### 1.1. Sơ đồ Phân cấp Kiến trúc Thông tin (IA Hierarchy Tree)

```mermaid
graph TD
    %% Entry Point
    START(["Khởi động App (App Launch / Splash)"]) --> AUTH_GATE{"Đã có Phiên Đăng nhập<br>(Valid Token)?"}

    %% Tầng 0: Authentication Stack
    subgraph STACK_0 ["Tầng 0: Authentication & Account Recovery Stack (TV1 Chủ trì)"]
        AUTH_GATE -- "Chưa có / Hết hạn" --> S01["<b>S01: Màn hình Đăng nhập</b><br>• Đăng nhập Email & Pass (US-01)<br>• Đăng nhập Google FPT SSO (US-02)<br>• Điều hướng Quên mật khẩu"]
        S01 -- "Bấm 'Quên mật khẩu' / Chưa xác thực" --> S02["<b>S02: Xác minh Email & Đặt lại Mật khẩu</b><br>• Nhập mã OTP 6 số (US-03)<br>• Form đặt lại mật khẩu mới (US-07)<br>• Nút Gửi lại mã sau 60s"]
        S02 -- "Xác minh / Đổi pass thành công" --> S01
    end

    %% Tầng 1: Bottom Navigation Bar
    AUTH_GATE -- "Đã xác thực" --> MAIN_SHELL["<b>Main Application Shell</b><br>(Bottom Navigation Bar 4 Tabs - Chiều cao 56dp)"]
    S01 -- "Đăng nhập thành công" --> MAIN_SHELL

    subgraph STACK_1 ["Tầng 1: Bottom Navigation Bar (4 Tab Trụ cột)"]
        MAIN_SHELL --> TAB1["<b>Tab 1: S03 Home Feed</b><br>(TV2 phụ trách)<br>• Switcher: Chung / Nhóm ngành<br>• Danh sách bài chia sẻ & Q&A<br>• Nút Đặt câu hỏi (US-22)"]
        MAIN_SHELL --> TAB2["<b>Tab 2: S04 Member Directory</b><br>(TV2 phụ trách)<br>• Ô tìm kiếm theo tên (US-06)<br>• Filter Chips: Campus & Ngành (48dp)<br>• Thẻ Alumni tóm tắt (K15, Công ty)"]
        MAIN_SHELL --> TAB3["<b>Tab 3: S06 Chat Room</b><br>(TV3 phụ trách)<br>• Danh sách hội thoại gần đây (US-14)<br>• Khung chat 1-1 thời gian thực (US-13)<br>• Chỉ báo Seen & Typing (US-16, 17)"]
        MAIN_SHELL --> TAB4["<b>Tab 4: S07 My Profile</b><br>(TV4 phụ trách)<br>• Avatar, Bio, Khóa, Campus (US-04)<br>• Ô nhúng Link Google Calendar (US-11)<br>• Điều hướng Cài đặt Bảo mật"]
    end

    %% Tầng 2: Detail Views & Overlays
    subgraph STACK_2 ["Tầng 2: Màn hình Chi tiết & Hộp thoại Hành động (Detail & Modals)"]
        TAB2 -- "Chọn thẻ Cựu sinh viên" --> S05["<b>S05: Member Public Profile</b><br>(TV3 phụ trách)<br>• Chi tiết Kỹ năng, Kinh nghiệm (US-05)<br>• Nút Chính: 'Book via Google Calendar' (US-11)<br>• Nút Viền: 'Direct Message' (US-12)<br>• Nút Đỏ: 'Block This User' (US-10)"]
        TAB1 -- "Xem bài viết cựu SV" --> S05
        TAB4 -- "Bấm 'Privacy & Security'" --> S08["<b>S08: Privacy & Account Settings</b><br>(TV4 phụ trách)<br>• 5 Công tắc Privacy có nhãn ON/OFF (US-09)<br>• Mục 'Change Password' (US-07)<br>• Danh sách chặn: 'Blocked Users' (US-10)<br>• Nút Đỏ: 'Log Out' (US-08)"]

        S05 -- "Bấm 'Block This User'" --> MODAL_BLOCK["<b>Modal Dialog: Xác nhận Chặn</b><br>• Tiêu đề cảnh báo màu đỏ (#DC2626)<br>• Nút Hủy (Cancel) & Xác nhận Chặn"]
        S05 -- "Bấm 'Book Calendar'" --> MODAL_CAL["<b>Google Calendar Booking Sheet</b><br>• Mở Webview/Link chọn khung giờ 30m<br>• Đồng bộ lịch hẹn vào thiết bị"]
        S08 -- "Bấm 'Log Out'" --> S01
    end

    %% Trạng thái Lỗi & Phục hồi Toàn cục
    subgraph SYSTEM_STATES ["Trạng thái Hệ thống & Phục hồi Lỗi (WCAG & Heuristic #5, #9)"]
        ERR_NET["<b>Network Error Banner</b><br>• Hiển thị đầu màn hình khi mất mạng<br>• Nút 'Retry Save' (48dp) bảo lưu dữ liệu"]
        ERR_VAL["<b>Inline Validation Error</b><br>• Viền đỏ #DC2626 + Icon cảnh báo<br>• Thông báo lỗi tiếng Việt dễ hiểu"]
    end

    STACK_0 -.-> ERR_VAL
    STACK_1 -.-> ERR_NET
    STACK_2 -.-> ERR_NET
```

---

## 2. Chi tiết 3 Luồng Người dùng Chuẩn (3 Detailed User Flows)

---

### 🚀 Flow 1 (`F01`): Khởi tạo Danh tính, Xác thực & Phục hồi Tài khoản (Authentication & Recovery Flow)

> **Màn hình Trọng tâm:** **`S01`** (Login & Google Auth) và **`S02`** (Verify Email & Reset Password) — **Do TV1 (Võ Quang Tâm) làm chủ trì**.  
> **User Stories Bao phủ:** `US-01` (Login Email/Pass), `US-02` (Login Google SSO), `US-03` (Verify Email), `US-07` (Reset Password), `US-08` (Token Invalidation).

#### A. Sơ đồ Luồng Chi tiết `F01` (Mermaid Flowchart)

```mermaid
flowchart TD
    F01_START(["Bắt đầu Flow 1: Mở Ứng dụng"]) --> F01_CHECK{"Đã có phiên<br>đăng nhập hợp lệ?"}

    %% Nhánh Đã đăng nhập
    F01_CHECK -- "Có (Token hợp lệ)" --> F01_HOME["Điều hướng thẳng vào Tab 1: S03 Home Feed"]

    %% Nhánh Vào S01 Login
    F01_CHECK -- "Chưa / Token hết hạn" --> F01_S01["<b>Hiển thị Màn hình S01: Login</b><br>• Form nhập Email & Mật khẩu<br>• Nút 'Sign in with Google' (#FFFFFF, viền xám, cao 48dp)<br>• Liên kết 'Quên mật khẩu?'"]

    %% Lựa chọn phương thức đăng nhập
    F01_S01 --> F01_CHOICE{"Người dùng chọn<br>Phương thức nào?"}

    %% Nhánh 1: Google SSO (Happy Path 1)
    F01_CHOICE -- "Bấm 'Sign in with Google' (US-02)" --> F01_GOOGLE_POPUP["Mở Google OAuth SDK Dialog"]
    F01_GOOGLE_POPUP --> F01_G_VERIFY{"Email có đuôi<br>@fpt.edu.vn?"}
    F01_G_VERIFY -- "Đúng email FPT" --> F01_G_SUCCESS["Cấp Access Token & Khởi tạo Profile mặc định"]
    F01_G_SUCCESS --> F01_HOME
    F01_G_VERIFY -- "Sai tên miền (Gmail thường)" --> F01_G_ERR["<b>Báo lỗi Inline:</b> 'Vui lòng sử dụng tài khoản email sinh viên/cựu sinh viên @fpt.edu.vn'"]
    F01_G_ERR --> F01_S01

    %% Nhánh 2: Đăng nhập Email & Mật khẩu (Happy Path 2)
    F01_CHOICE -- "Nhập Email & Mật khẩu (US-01)" --> F01_VALIDATE_INPUT{"Kiểm tra cú pháp<br>Client-side Regex"}
    F01_VALIDATE_INPUT -- "Sai định dạng email / Pass < 8 ký tự" --> F01_VAL_ERR["Hiển thị Inline Error đỏ #DC2626 ngay dưới TextField"]
    F01_VAL_ERR --> F01_S01

    F01_VALIDATE_INPUT -- "Hợp lệ" --> F01_API_LOGIN["Gửi request Đăng nhập tới Backend Auth Server"]
    F01_API_LOGIN --> F01_AUTH_RES{"Kết quả phản hồi<br>từ Server?"}

    F01_AUTH_RES -- "Thành công & Đã kích hoạt" --> F01_SAVE_TOKEN["Lưu Encrypted Token vào Keystore/Keychain"]
    F01_SAVE_TOKEN --> F01_HOME

    F01_AUTH_RES -- "Tài khoản chưa xác minh email (US-03)" --> F01_NAV_VERIFY["Điều hướng sang <b>S02: Verify Email</b><br>Tự động gửi mã OTP 6 chữ số"]
    
    F01_AUTH_RES -- "Sai mật khẩu" --> F01_CHECK_ATTEMPTS{"Đã sai quá<br>5 lần liên tiếp?"}
    F01_CHECK_ATTEMPTS -- "Chưa quá 5 lần" --> F01_ERR_PASS["Báo lỗi Inline: 'Mật khẩu không chính xác. Còn lại (5 - n) lần thử'"]
    F01_ERR_PASS --> F01_S01
    F01_CHECK_ATTEMPTS -- "Đạt 5 lần (Brute-force)" --> F01_LOCKOUT["<b>Khóa tạm thời 60 giây:</b> Vô hiệu hóa nút Đăng nhập + Đồng hồ đếm ngược"]
    F01_LOCKOUT --> F01_S01

    %% Nhánh 3: Xác minh Email (S02)
    F01_NAV_VERIFY --> F01_S02_OTP["<b>Hiển thị S02 (Chế độ Verify Email):</b><br>• Hiển thị email nhận mã: tr***@fpt.edu.vn<br>• 6 ô nhập mã OTP tách rời (Auto-focus, paste OTP)<br>• Bộ đếm ngược 'Gửi lại mã sau 60s'"]
    F01_S02_OTP --> F01_SUBMIT_OTP{"Kiểm tra mã OTP"}
    F01_SUBMIT_OTP -- "OTP chính xác" --> F01_OTP_OK["Kích hoạt tài khoản thành công! Toast thông báo xanh #10B981"]
    F01_OTP_OK --> F01_HOME
    F01_SUBMIT_OTP -- "OTP sai / Hết hạn" --> F01_OTP_ERR["Báo lỗi: 'Mã OTP không đúng hoặc đã hết hiệu lực. Vui lòng bấm Gửi lại mã'"]
    F01_OTP_ERR --> F01_S02_OTP

    %% Nhánh 4: Quên & Đặt lại Mật khẩu (S02 - US-07)
    F01_CHOICE -- "Bấm 'Quên mật khẩu?'" --> F01_S02_RESET["<b>Hiển thị S02 (Chế độ Reset Password):</b><br>• Ô nhập Email @fpt.edu.vn<br>• Nút 'Gửi mã khôi phục' (48dp, màu cam #F27024)"]
    F01_S02_RESET --> F01_SEND_RESET["Nhập email và bấm gửi"]
    F01_SEND_RESET --> F01_INPUT_NEW_PASS["Mở form: Nhập OTP khôi phục + Mật khẩu mới + Nhập lại mật khẩu"]
    F01_INPUT_NEW_PASS --> F01_CHECK_NEW_PASS{"Mật khẩu mới thỏa<br>≥8 ký tự, hoa, thường, số?"}
    F01_CHECK_NEW_PASS -- "Hợp lệ" --> F01_RESET_DONE["Đặt lại mật khẩu thành công! Hủy toàn bộ token cũ"]
    F01_RESET_DONE --> F01_S01
    F01_CHECK_NEW_PASS -- "Không hợp lệ / Không khớp" --> F01_NEW_PASS_ERR["Báo lỗi Inline: 'Mật khẩu xác nhận không khớp'"]
    F01_NEW_PASS_ERR --> F01_INPUT_NEW_PASS
```

#### B. Đặc tả Kỹ thuật & Trạng thái Phục hồi Lỗi (Error Handling in `F01`)
1. **Lỗi Mất kết nối Mạng (Network Offline State):** Khi bấm Đăng nhập hoặc Xác thực OTP mà thiết bị mất mạng, hiển thị **Banner cảnh báo đỏ nhạt (`#FDF3F2`, viền `#DC2626`)** ở đầu màn hình với thông báo *"Không có kết nối Internet. Vui lòng kiểm tra Wifi/4G"* và giữ nguyên toàn bộ nội dung email/mật khẩu người dùng đã gõ trong form (không làm mất dữ liệu của sinh viên).
2. **Cơ chế Chống Brute-Force (Rate Limiting):** Sau 5 lần nhập sai liên tiếp, nút Đăng nhập bị vô hiệu hóa (disabled xám `#E5E7EB`), hiển thị nhãn đếm ngược *"Thử lại sau 59s... 58s..."* để ngăn chặn tấn công dò mật khẩu.
3. **Tiêu chuẩn Tiếp cận (A11y):** Các ô nhập mật khẩu luôn có biểu tượng con mắt **(Show/Hide Password)** với vùng chạm tối thiểu **48×48 dp** để người dùng kiểm tra ký tự trước khi gửi.

---

### 🔍 Flow 2 (`F02`): Khám phá Thành viên & Đặt lịch Tư vấn 1-1 qua Google Calendar (Discovery & Booking Flow)

> **Màn hình Trọng tâm:** `S03` (Home Feed), `S04` (Directory), `S05` (Member Public Profile) — **Do TV2 & TV3 phụ trách phối hợp**.  
> **User Stories Bao phủ:** `US-05` (View Profile), `US-06` (Member Directory & Filter), `US-11` (Google Calendar Link).

#### A. Sơ đồ Luồng Chi tiết `F02` (Mermaid Flowchart)

```mermaid
flowchart TD
    F02_START(["Bắt đầu Flow 2: Sinh viên tìm kiếm Mentor"]) --> F02_ENTRY{"Điểm bắt đầu<br>trong ứng dụng"}

    %% Điểm vào 1: Từ Home Feed
    F02_ENTRY -- "Đang xem Tab 1: S03 Home Feed" --> F02_HOME_CARD["Thấy bài chia sẻ kinh nghiệm của Đặng Minh Tuấn (K15 SE)"]
    F02_HOME_CARD --> F02_TAP_AUTHOR["Chạm vào Avatar hoặc Tên tác giả"]

    %% Điểm vào 2: Từ Directory
    F02_ENTRY -- "Chuyển sang Tab 2: S04 Directory (US-06)" --> F02_DIRECTORY["<b>Mở S04 Member Directory:</b><br>• Ô tìm kiếm theo tên cựu sinh viên<br>• Thanh cuộn Filter Chips ngang: Campus & Ngành học"]
    F02_DIRECTORY --> F02_APPLY_FILTER["Chọn Filter Chips: '✓ 📍 HCMC' và '✓ 💻 Software Engineering'"]
    F02_APPLY_FILTER --> F02_QUERY_RES{"Tìm thấy kết quả<br>phù hợp?"}

    %% Nhánh Empty State
    F02_QUERY_RES -- "Không có thành viên thỏa điều kiện" --> F02_EMPTY["<b>Hiển thị S04 Empty State:</b><br>• Minh họa vector 'Không tìm thấy kết quả'<br>• Dòng chữ: 'Thử xóa bớt bộ lọc Campus hoặc Ngành'<br>• Nút phụ: 'Xóa tất cả bộ lọc' (48dp)"]
    F02_EMPTY -- "Bấm Xóa bộ lọc" --> F02_DIRECTORY

    %% Nhánh Xem Profile S05
    F02_QUERY_RES -- "Hiển thị danh sách thẻ Alumni" --> F02_SELECT_CARD["Bấm vào Thẻ Cựu SV: Đặng Minh Tuấn (K15 • Grab)"]
    F02_TAP_AUTHOR --> F02_S05
    F02_SELECT_CARD --> F02_S05["<b>Mở S05: Member Public Profile (US-05):</b><br>• Avatar + Tích xanh Verified Alumni + Khóa K15 SE<br>• Bio tóm tắt & Kỹ năng (Go, Kubernetes, Cloud)<br>• <b>Nút CTA Chính duy nhất: 'Book via Google Calendar' (Cam #F27024, 48dp)</b><br>• Nút Phụ Viền: 'Direct Message' (Xanh đen #1B365D)<br>• Nút Viền Đỏ Cuối Trang: '🚫 Block This User' (#DC2626)"]

    %% Đặt lịch qua Google Calendar
    F02_S05 --> F02_TAP_CALENDAR["Sinh viên bấm 'Book via Google Calendar' (US-11)"]
    F02_TAP_CALENDAR --> F02_CHECK_CAL_LINK{"Mentor có cài đặt<br>link Google Calendar hợp lệ?"}

    F02_CHECK_CAL_LINK -- "Có link Calendar" --> F02_OPEN_CALENDAR["Mở Custom Chrome Tab / Webview nhúng lịch Google Calendar của Mentor"]
    F02_OPEN_CALENDAR --> F02_PICK_SLOT["Sinh viên tự chọn ca rảnh 30 phút (Ví dụ: 09:30 AM Thứ Bảy)"]
    F02_PICK_SLOT --> F02_CONFIRM_BOOKING["Xác nhận đặt hẹn & Nhập ghi chú câu hỏi cần tư vấn OJT"]
    F02_CONFIRM_BOOKING --> F02_CAL_SUCCESS["<b>Thành công!</b><br>• Tự động thêm sự kiện vào Google Calendar của sinh viên<br>• Gửi email thông báo xác nhận tới cả 2 bên"]

    F02_CHECK_CAL_LINK -- "Mentor chưa cập nhật link" --> F02_CAL_EMPTY["Hiển thị Tooltip thông báo: 'Mentor hiện chưa mở lịch hẹn trực tuyến. Vui lòng gửi tin nhắn Direct Message'"]
    F02_CAL_EMPTY --> F02_S05
```

---

### 🛡️ Flow 3 (`F03`): Quản lý Hồ sơ, Kiểm soát Quyền riêng tư & Cơ chế Chặn Quấy rối (Profile, Privacy & Safety Flow)

> **Màn hình Trọng tâm:** `S07` (My Profile), `S08` (Privacy & Blocked List), `S05` (Block Action) — **Do TV3 & TV4 phụ trách phối hợp**.  
> **User Stories Bao phủ:** `US-04` (Edit Profile), `US-07` (Change Password), `US-08` (Logout), `US-09` (Privacy Toggles), `US-10` (Block User), `US-11` (Add Calendar Link).

#### A. Sơ đồ Luồng Chi tiết `F03` (Mermaid Flowchart)

```mermaid
flowchart TD
    F03_START(["Bắt đầu Flow 3: Quản lý Tài khoản & An toàn Cá nhân"]) --> F03_CHOICE{"Người dùng muốn<br>thực hiện tác vụ gì?"}

    %% Nhánh 1: Cập nhật Profile & Link Calendar
    F03_CHOICE -- "Chỉnh sửa Hồ sơ (US-04, US-11)" --> F03_S07["<b>Mở Tab 4: S07 My Profile:</b><br>• Form chỉnh sửa Họ tên, Khóa, Campus, Bio<br>• <b>Ô nhập: 'Google Calendar Booking Link' (US-11)</b><br>• Nút 'Save Changes' (48dp, Cam #F27024)"]
    F03_S07 --> F03_INPUT_CAL["Dán đường dẫn lịch cá nhân (calendar.google.com/...)"]
    F03_INPUT_CAL --> F03_SAVE_PROFILE["Bấm 'Save Changes'"]

    F03_SAVE_PROFILE --> F03_CHECK_URL{"Kiểm tra định dạng<br>URL Google Calendar"}
    F03_CHECK_URL -- "Sai định dạng (Không phải URL Google Calendar)" --> F03_URL_ERR["<b>Inline Validation Error (Phát hiện C06 & Quyết định D06):</b><br>• Viền ô chuyển màu đỏ #DC2626<br>• Dòng chữ lỗi: 'Vui lòng nhập đúng liên kết calendar.google.com hợp lệ'<br>• Nút 'Retry Sync'"]
    F03_URL_ERR --> F03_S07

    F03_CHECK_URL -- "Đúng định dạng" --> F03_CHECK_NET{"Kiểm tra kết nối mạng"}
    F03_CHECK_NET -- "Mất kết nối Internet" --> F03_NET_ERR["<b>Network Error Banner (Đỉnh màn hình):</b><br>• Nền đỏ nhạt #FDF3F2, viền đỏ #DC2626<br>• Thông báo: 'Mất kết nối mạng khi lưu. Dữ liệu hồ sơ của bạn được bảo lưu an toàn'<br>• Nút '↻ Retry Save' (48dp)"]
    F03_NET_ERR --> F03_S07
    F03_CHECK_NET -- "Thành công" --> F03_SAVE_OK["Cập nhật Profile thành công! Badge chuyển sang 'Active & Synced'"]

    %% Nhánh 2: Cài đặt Riêng tư (Privacy)
    F03_CHOICE -- "Quản lý Quyền riêng tư (US-09)" --> F03_S08["<b>Mở S08: Privacy & Security Settings:</b><br>• Khối 'Privacy & Profile Visibility' chứa 5 Switch:<br>  1. Hiển thị Số điện thoại cá nhân<br>  2. Hiển thị Email liên hệ<br>  3. Cho phép nhận tin nhắn từ người lạ<br>  4. Hiển thị Lịch trống công khai<br>  5. Hiển thị trạng thái Online<br>• <b>Mọi công tắc có nhãn chữ tường minh [ON] / [OFF] (WCAG 1.4.1)</b>"]
    F03_S08 --> F03_TOGGLE_SWITCH["Gạt công tắc tắt 'Hiển thị Số điện thoại' sang [OFF]"]
    F03_TOGGLE_SWITCH --> F03_PRIVACY_SYNC["Lưu tùy chọn Privacy tức thì (Không cần bấm Save)"]

    %% Nhánh 3: Chặn tài khoản quấy rối (US-10)
    F03_CHOICE -- "Chặn người dùng quấy rối (US-10)" --> F03_OPEN_OFFENDER["Mở trang cá nhân S05 của đối tượng quấy rối"]
    F03_OPEN_OFFENDER --> F03_CLICK_BLOCK["Kéo xuống cuối trang, bấm nút <b>'🚫 Block This User'</b> (Viền đỏ #DC2626, cao 48dp)"]
    F03_CLICK_BLOCK --> F03_MODAL_CONFIRM["<b>Hiển thị Modal Confirmation Dialog (Quyết định D01):</b><br>• Tiêu đề: 'Chặn Đặng Minh Tuấn?'<br>• Nội dung: 'Người này sẽ không thể gửi tin nhắn hoặc đặt lịch hẹn trên Google Calendar của bạn nữa.'<br>• 2 Nút: 'Hủy bỏ' (Outlined 48dp) và <b>'Xác nhận Chặn' (Đỏ đặc #DC2626, 48dp)</b>"]
    F03_MODAL_CONFIRM --> F03_USER_DECISION{"Người dùng chọn?"}
    F03_USER_DECISION -- "Bấm 'Hủy bỏ'" --> F03_CANCEL_BLOCK["Đóng Modal, giữ nguyên trạng thái"]
    F03_USER_DECISION -- "Bấm 'Xác nhận Chặn'" --> F03_EXECUTE_BLOCK["1. Thêm ID người dùng vào 'Blocked Users List' trong S08<br>2. Hủy các cuộc hội thoại chat đang có<br>3. Điều hướng người dùng quay trở lại S04 Directory kèm Toast xác nhận"]
    F03_EXECUTE_BLOCK --> F03_BLOCKED_MANAGEMENT["<b>Mục Quản lý Chặn trong S08:</b><br>Người dùng có thể vào S08 bấm 'Bỏ chặn (Unblock)' bất kỳ lúc nào"]
```

---

## 3. Bảng Ánh xạ Nghiệm thu Toàn diện (Full Traceability Matrix)

Bảng đối chiếu đảm bảo **100% các User Stories cốt lõi** đều có màn hình sở hữu, luồng thực thi và tuân thủ các cửa kiểm định chất lượng (Quality Gates P0–P5 theo đề Lab 2):

| Mã US | Mô tả User Story | Thuộc Luồng | Màn hình Trực tiếp | Trạng thái Ngoại lệ / Xử lý Lỗi (Error State) | Tiêu chuẩn Khả năng Tiếp cận (WCAG AA & Ergonomics) | Thành viên Sở hữu |
| :---: | :--- | :---: | :---: | :--- | :--- | :---: |
| **`US-01`** | Đăng nhập bằng Email (@fpt.edu.vn) & Mật khẩu | **`F01`** | **`S01`** | - Báo lỗi sai mật khẩu kèm số lần còn lại.<br>- Khóa 60s khi sai 5 lần liên tiếp (Rate limit). | - Touch target nút Đăng nhập đạt **48dp**.<br>- Chữ phụ xám `#595959` tương phản đạt **5.4:1**. | **TV1 (Tâm)** |
| **`US-02`** | Đăng nhập bằng Google SSO FPT | **`F01`** | **`S01`** | Báo lỗi khi chọn tài khoản Gmail cá nhân không phải tên miền giáo dục FPT. | Nút Google có icon chuẩn, viền `#E5E7EB`, chiều cao tối thiểu **48dp**. | **TV1 (Tâm)** |
| **`US-03`** | Xác minh Email sinh viên qua mã OTP 6 số | **`F01`** | **`S02`** | - Báo lỗi OTP sai / hết hạn.<br>- Bộ đếm ngược 60 giây chống bấm gửi lại liên tục. | 6 ô OTP tách rời, tự động nhảy con trỏ phím (Auto-focus), hỗ trợ paste OTP nhanh. | **TV1 (Tâm)** |
| **`US-04`** | Cập nhật hồ sơ cá nhân (Bio, Campus, Khóa, Ngành) | **`F03`** | **`S07`** | Báo lỗi để trống các trường bắt buộc; lưu tạm form khi mất kết nối mạng. | Chiều cao ô nhập text tối thiểu **48dp**, cỡ chữ nhãn và giá trị **≥ 14sp**. | **TV4 (Tín)** |
| **`US-05`** | Xem hồ sơ công khai của thành viên khác | **`F02`** | **`S05`** | Báo lỗi người dùng không tồn tại hoặc đã bị khóa tài khoản. | Phân cấp thị giác rõ ràng: Tên **20sp Bold**, Thẻ kỹ năng padding **8dp/16dp**. | **TV3 (Khải)** |
| **`US-06`** | Tìm kiếm & Lọc danh bạ thành viên theo Campus/Ngành | **`F02`** | **`S04`** | Hiển thị màn hình rỗng (**Empty State**) kèm nút 'Xóa tất cả bộ lọc' khi không có kết quả. | Filter Chips cao đúng **48dp**, có dấu tích `✓` khi chọn (không chỉ dựa vào màu sắc). | **TV2 (Trung)** |
| **`US-07`** | Đổi mật khẩu & Đặt lại mật khẩu khi quên | **`F01, F03`** | **`S02, S08`** | Báo lỗi mật khẩu mới không đủ độ phức tạp hoặc mật khẩu xác nhận không khớp. | Có nút ẩn/hiện mật khẩu (Show/Hide Toggle) kích thước vùng chạm **48×48 dp**. | **TV1 & TV4** |
| **`US-08`** | Đăng xuất an toàn & Hủy phiên token | **`F01, F03`** | **`S01, S08`** | Hộp thoại xác nhận đăng xuất tránh bấm nhầm; xóa sạch session trong bộ nhớ an toàn. | Nút Đăng xuất màu đỏ cam có icon chỉ dẫn rõ ràng. | **TV1 & TV4** |
| **`US-09`** | Cài đặt quyền riêng tư & Ẩn/Hiện thông tin cá nhân | **`F03`** | **`S08`** | Báo lỗi đồng bộ cài đặt khi mất mạng kèm nút Retry. | Toàn bộ các công tắc Switch đều có nhãn chữ tường minh **`[ON]` / `[OFF]`** cạnh switch. | **TV4 (Tín)** |
| **`US-10`** | Chặn người dùng làm phiền & Quản lý danh sách chặn | **`F02, F03`** | **`S05, S08`** | Hộp thoại xác nhận chặn (**Confirmation Dialog**) với nút Hủy và nút Xác nhận Chặn màu đỏ `#DC2626`. | Nút 'Block This User' đặt ở vị trí an toàn cuối trang, cao **48dp**, không giấu trong menu nhỏ. | **TV3 & TV4** |
| **`US-11`** | Nhúng link Google Calendar để liên hệ & đặt lịch 1-1 | **`F02, F03`** | **`S05, S07`** | Inline Error viền đỏ `#DC2626` khi dán sai URL Google Calendar kèm nút 'Retry Sync'. | Nút chính **'Book via Google Calendar'** màu cam FPT `#F27024` cao **48dp** nổi bật nhất màn hình. | **TV3 & TV4** |

---

## 4. Tóm tắt Giá trị Bàn giao cho Figma & Flutter của TV1

Tài liệu này xác lập nền móng kỹ thuật chuẩn xác để TV1 triển khai:
1. **Lên Figma Page 01:** Vẽ lại sơ đồ cây IA và 3 User Flows bằng các frame trực quan để người chấm thấy toàn cảnh luồng đi của người dùng.
2. **Thiết kế Màn hình `S01` & `S02` (Figma Page 02 & 03):** Đã có đầy đủ danh sách trạng thái bắt buộc:
   - `S01`: Default, Focused, Password Visible, Error (Sai pass, Khóa 60s), Loading (Đang xác thực Google).
   - `S02`: Verify Email OTP (Default, OTP Filled, OTP Error, Resend Cooldown), Reset Password (Form nhập pass mới, Inline Validation Error, Success Dialog).
3. **Bàn giao Flutter (`flutter-handoff.md`):** Chuẩn bị sẵn logic regex email FPT, bộ đếm ngược 60s cho OTP và xử lý mã trạng thái HTTP Auth.
