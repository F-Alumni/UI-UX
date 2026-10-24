# 🕹️ Hướng dẫn Chi tiết & Ma trận Nối dây Interactive Prototype (Page 06)

> **Dự án:** **FPT Alumni Connect** — PRM323 Lab 2 (Google Stitch → Figma → Flutter Handoff)  
> **Người thực hiện chính:** **Đỗ Trọng Tín** — MSSV: **SE190218** (Vai trò: **TV4 — Interactive Prototype, Responsive QA & Handoff Lead**)  
> **Căn cứ Quy chuẩn:** Đề Lab 2 Mục 7 (Page 06 Prototype), Tiêu chí Usability Rubric (Zero Dead-Ends, Overlay Dialog, Skeleton Loading to Result Transition).

---

## 1. Cấu trúc Bố trí Canvas Page 06: `06_Interactive_Prototype`

Trên Figma Page 06, toàn bộ 8 màn hình chính (`S01` → `S08`) và 2 Frame biến thể trạng thái tương tác được sắp xếp theo đúng thứ tự luồng trải nghiệm người dùng từ trái sang phải:

```
┌─────────┐   ┌─────────┐   ┌─────────┐   ┌───────────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌───────────────────────┐
│ S01     │ ➔ │ S02     │ ➔ │ S03     │ ➔ │ S04_skeleton  │ ➔ │ S04     │ ➔ │ S05     │ ➔ │ S06     │ ➔ │ S07     │ ➔ │ S08     │ ➔ │ S08_overlay_dialog    │
│ (Login) │   │ (Verify)│   │ (Home)  │   │ (Search Load) │   │ (Direc) │   │ (Mentor)│   │ (Chat)  │   │ (Profil)│   │ (Privac)│   │ (Xác nhận Bỏ chặn)    │
└─────────┘   └─────────┘   └─────────┘   └───────────────┘   └─────────┘   └─────────┘   └─────────┘   └─────────┘   └─────────┘   └───────────────────────┘
```

---

## 2. Ma trận 3 User Flows Chi tiết & Thông số Nối dây (Wiring Matrix)

### 🚀 Flow 1: Khởi tạo Danh tính & Xác thực Học vụ (Auth & Onboarding)
* **Flow Starting Point 1:** Đặt tại **Frame `S01`** (Tên: `Flow 1: Auth & Onboarding`).

| Điểm Bắt đầu (Source Node) | Màn hình Nguồn | Thao tác (Trigger) | Hành động (Action) | Đích đến (Destination Frame) | Hiệu ứng Chuyển cảnh (Animation) | Ý đồ Thiết kế & Trợ năng |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| Nút `Tiếp tục với Google (@fpt / @fe)` | `S01` | On tap | Navigate to | `S03 - Home Feed` | Instant | Đăng nhập SSO 1 chạm cho sinh viên FPT |
| Nút màu cam `Đăng nhập →` | `S01` | On tap | Navigate to | `S03 - Home Feed` | Instant | Đăng nhập truyền thống bằng email/pass |
| Text link `Chưa có tài khoản? Đăng ký...` | `S01` | On tap | Navigate to | `S02 - Verify Identity` | Smart Animate (300ms) | Chuyển sang luồng nhập MSSV & OTP xác thực |
| Mũi tên `←` (AppBar) | `S02` | On tap | Back | `S01 - Login` | Slide Right (200ms) | Trở về màn trước, không gây ngõ cụt |
| Nút `Xác nhận & Hoàn tất` | `S02` | On tap | Navigate to | `S03 - Home Feed` | Smart Animate (300ms) | Hoàn tất học vụ, cấp quyền vào ứng dụng |

---

### 🔍 Flow 2: Khám phá Cựu sinh viên & Xem Lịch Google Calendar (Discovery & Mentorship Connect)
* **Flow Starting Point 2:** Đặt tại **Frame `S03`** (Tên: `Flow 2: Mentor Search & Calendar`).

| Điểm Bắt đầu (Source Node) | Màn hình Nguồn | Thao tác (Trigger) | Hành động (Action) | Đích đến (Destination Frame) | Hiệu ứng Chuyển cảnh (Animation) | Ý đồ Thiết kế & Trợ năng |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| Icon `Directory (Tìm kiếm)` ở Bottom Bar | `S03` | On tap | Navigate to | `S04_loading_skeleton` | Instant | Chuyển sang tìm kiếm, kích hoạt skeleton |
| **Cả Frame `S04_loading_skeleton`** | `S04_skeleton` | **After delay (800ms)** | **Navigate to** | **`S04 - Alumni Directory`** | **Dissolve (300ms)** | **Tiêu chí Rubric: Chuyển từ Skeleton sang kết quả thật** |
| Thẻ `Dang Minh Tuan` (hoặc nút `Xem Hồ sơ →`) | `S04` | On tap | Navigate to | `S05 - Hồ sơ Cựu sinh viên` | Smart Animate (300ms) | Mở hồ sơ chi tiết Tech Lead VNG |
| Nút phụ `📅 Xem Lịch rảnh tham khảo` | `S05` | On tap | Open link | `https://calendar.google.com/` | External URL | Xem lịch rảnh của Mentor (US-11) |
| Nút chính `💬 Nhắn tin trao đổi` | `S05` | On tap | Navigate to | `S06 - Chat Room` | Smart Animate (300ms) | Mở phòng chat trao đổi và gửi file CV |
| Mũi tên `←` (AppBar) | `S06` | On tap | Back | `S05` | Slide Right | Quay về hồ sơ cựu sinh viên |
| Mũi tên `←` (AppBar) | `S05` | On tap | Back | `S04` | Slide Right | Quay về danh sách tìm kiếm |

---

### 🛡️ Flow 3: Quản lý Hồ sơ, Bảo mật & Chặn Người dùng (Profile, Privacy & Safety — TV4 Chủ trì)
* **Flow Starting Point 3:** Đặt tại **Frame `S07`** (Tên: `Flow 3: Profile & Privacy Safety`).

| Điểm Bắt đầu (Source Node) | Màn hình Nguồn | Thao tác (Trigger) | Hành động (Action) | Đích đến (Destination Frame) | Hiệu ứng Chuyển cảnh (Animation) | Ý đồ Thiết kế & Trợ năng |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| Nút `↻ Retry Save` trên Network Banner | `S07` | On tap | Smart Animate | `S07` (Ẩn banner lỗi) | Dissolve (200ms) | Phục hồi lỗi rớt mạng, giữ nguyên form (D06) |
| Nút pill `Security →` (AppBar) | `S07` | On tap | Navigate to | `S08 - Privacy & Security` | Smart Animate (300ms) | Mở trang 5 công tắc quyền riêng tư |
| **Nút `Manage & Unblock →` trên S08** | **`S08`** | **On tap** | **Open overlay** | **`S08_overlay_dialog`** | **Centered, Dim 40% Background** | **Tiêu chí Rubric: Mở Dialog dạng Overlay** |
| Nút `Hủy` trong Dialog | `Dialog` | On tap | Close overlay | Quay về `S08` | Instant | Đóng popup, giữ nguyên danh sách chặn |
| Nút đỏ `Bỏ chặn` trong Dialog | `Dialog` | On tap | Close overlay | Quay về `S08` | Instant | Bỏ chặn người dùng, cập nhật số lượng |
| Nút đỏ `🚫 Chặn người dùng này` trên `S05` | `S05` | On tap | Open overlay | `S08_overlay_dialog` | Centered, Dim 40% | Hộp thoại xác nhận chặn đối tượng quấy rối |
| Nút viền đỏ `🚪 Log Out of Account` | `S08` | On tap | Navigate to | `S01 - Login` | Slide Left (300ms) | Đăng xuất an toàn, quay về màn hình đầu |
| Mũi tên `←` (AppBar) | `S08` | On tap | Back | `S07 - My Profile` | Slide Right | Quay lại trang cá nhân |

---

## 3. Bảng Kiểm tra Không Ngõ cụt (Zero Dead-Ends Verification)

Mỗi màn hình trong ứng dụng đều được kiểm tra nghiêm ngặt ít nhất 2 đường thoát (Escape routes):

- [x] **`S01 Login`:** Có thể đăng nhập vào `S03` hoặc chuyển sang xác thực `S02`.
- [x] **`S02 Verify`:** Có nút Back `←` quay lại `S01` và nút Xác nhận vào `S03`.
- [x] **`S03 Home`:** Có thể chuyển sang tab `S04` (Directory), tab `S06` (Chat), tab `S07` (Profile) hoặc mở trực tiếp bài viết `S05`.
- [x] **`S04 Directory`:** Có nút Back về `S03`, chạm thẻ sang `S05`, chuyển tab Bottom Bar sang `S06`, `S07`.
- [x] **`S05 Member Detail`:** Có nút Back về `S04`, nút `Nhắn tin trao đổi` sang `S06`, nút `Chặn` mở Dialog overlay.
- [x] **`S06 Chat Room`:** Có nút Back về `S05`, nút Lịch `📅` xem Google Calendar, menu `⋮` chặn người dùng.
- [x] **`S07 My Profile`:** Có nút `Security →` sang `S08`, nút thử lại `↻ Retry Save`, các tab Bottom Bar.
- [x] **`S08 Privacy & Security`:** Có nút Back về `S07`, nút mở Modal Unblock Dialog, nút `Log Out` quay về `S01`.
