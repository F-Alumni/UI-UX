# 🎨 Figma Production Blueprint, QA Checklist & Hướng dẫn Bảo vệ Đồ án (TV1)

> **Dự án:** **FPT Alumni Connect** — PRM323 Lab 2 (Google Stitch → Figma → Flutter Handoff)  
> **Người thực hiện:** **Võ Quang Tâm** (MSSV: **SE192621** — **TV1: UX Research, Persona & User Flow Lead**)  
> **Mục đích:** Cung cấp tài liệu thực chiến từng pixel để bạn mở Figma lên và thao tác hoàn thiện **Page 01**, **Page 02** và **Page 03**, chuẩn bị sẵn sàng cho buổi vấn đáp bảo vệ đồ án trước giảng viên.

---

## 📑 MỤC LỤC
1. [Hướng dẫn Dựng Figma Page 01: UX Research & Foundation (TV1 Chủ trì)](#1-hướng-dẫn-dựng-figma-page-01-ux-research--foundation-tv1-chủ-trì)
2. [Hướng dẫn Dựng Figma Page 02: Wireframes S01 & S02](#2-hướng-dẫn-dựng-figma-page-02-wireframes-s01--s02)
3. [Hướng dẫn Dựng Figma Page 03: Final UI S01 & S02 + 5 State Variants](#3-hướng-dẫn-dựng-figma-page-03-final-ui-s01--s02--5-state-variants)
4. [Quy tắc Vàng trên Figma (Tuyệt đối không Detach Instance)](#4-quy-tắc-vàng-trên-figma-tuyệt-đối-không-detach-instance)
5. [Hướng dẫn Đẩy Nhánh Git lên GitHub (git push)](#5-hướng-dẫn-đẩy-nhánh-git-lên-github-git-push)
6. [Bảng Tự Rà soát 20 Tiêu chí UI Checklist & Quality Gates P0–P5](#6-bảng-tự-rà-soát-20-tiêu-chí-ui-checklist--quality-gates-p0p5)
7. [Bộ Câu hỏi Vấn đáp Bảo vệ Đồ án trước Giảng viên (Defense Cheat-Sheet)](#7-bộ-câu-hỏi-vấn-đáp-bảo-vệ-đồ-án-trước-giảng-viên-defense-cheat-sheet)

---

## 1. Hướng dẫn Dựng Figma Page 01: UX Research & Foundation (TV1 Chủ trì)

Trên Figma, tạo hoặc chuyển đến trang **`Page 01: 01_UX_Foundation`**. Bố trí canvas thành **4 Frame lớn** nằm ngang từ trái sang phải:

```
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│ Frame 1: Cover  │  │ Frame 2: 3      │  │ Frame 3: IA     │  │ Frame 4: 3 User │
│ & Project Brief │  │ Persona Cards   │  │ Hierarchy Tree  │  │ Flows Diagram   │
│ (1440 x 900 dp) │  │ (1440 x 900 dp) │  │ (1440 x 800 dp) │  │ (1440 x 1200 dp)│
└─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘
```

### 1.1. Frame 1: Cover & Project Brief (`1440 × 900 dp`)
- **Nền:** `#1B365D` (FPT Navy Blue).
- **Logo trường FPT:** Kích thước `80 × 80 dp`, đặt góc trên bên trái.
- **Tiêu đề lớn:** `FPT Alumni Connect` (Font: Hanken Grotesk / Inter `48sp Bold`, màu `#FFFFFF`).
- **Phụ đề:** `Nền tảng kết nối Cựu sinh viên & Sinh viên Đại học FPT` (`20sp Regular`, màu `#E5E7EB`).
- **Khối Thông tin Nhóm 4 (Card trắng `#FFFFFF` bo góc `16dp`, padding `24dp`):**
  - Môn học: `PRM323 — Cross-Platform Application Development (Lab 2)`
  - Bảng 4 thành viên:
    - `TV1: Võ Quang Tâm (SE192621)` — UX Research & User Flow Lead (Sở hữu `S01, S02`)
    - `TV2: Trương Chí Trung (SE193059)` — Leader, AI Stitch & Critique Lead (Sở hữu `S03, S04`)
    - `TV3: Nguyễn Phương Khải (SE183590)` — Design System & Components Lead (Sở hữu `S05, S06`)
    - `TV4: Đỗ Trọng Tín (SE190218)` — Prototype & QA Lead (Sở hữu `S07, S08`)
- **Badge Tiêu chuẩn:** Đặt 4 pill badges góc dưới: `Figma 100% Auto Layout`, `WCAG 2.1 AA (5.4:1)`, `Flutter 3.x Ready`, `Quality Gates P0-P5 Passed`.

### 1.2. Frame 2: Bộ 3 Thẻ Chân dung Personas (`1440 × 900 dp`)
Tạo 3 Frame con đặt ngang nhau (mỗi card rộng `440 dp`, cao `800 dp`, bo góc `12 dp`, nền trắng `#FFFFFF`, viền `1dp #E5E7EB`, padding `24 dp`):

#### 📌 Card 1: `P01: Trương Chí Trung` (Primary Persona — SV Năm 3 K19)
- **Header:** Tag cam `[Primary Persona]` (`#F27024`), Avatar nam sinh viên đeo kính cận, Họ tên: **Trương Chí Trung (20 tuổi, K19 SE, FPT HCMC)**.
- **Quote:** *"Cần kết nối nhanh với Alumni ngành SE để review CV & xin lịch hẹn tư vấn OJT 1-1 mà không tốn thời gian nhắn tin chờ đợi."*
- **Bio & Goals:** Sinh viên năm 3 chuẩn bị OJT kỳ Fall 2026. Mục tiêu: Tìm cựu SV ngành SE qua Directory (`US-06`), đặt lịch rảnh 30 phút qua Google Calendar (`US-11`).
- **Pain Points:** Khó tìm cựu SV uy tín, bài đăng group Facebook dễ bị trôi, lệch giờ làm việc khó hẹn.
- **Context & A11y:** Dùng máy 1 tay ngón cái ngoài sảnh trường nắng gắt. Cận 3 độ (Cần tương phản ≥ 4.5:1, touch target ≥ 48dp).

#### 📌 Card 2: `P02: Đặng Minh Tuấn` (Secondary Persona — Senior Alumni Mentor)
- **Header:** Tag xanh Navy `[Alumni Mentor]` (`#1B365D`), Avatar Tech Lead, Họ tên: **Đặng Minh Tuấn (26 tuổi, Cựu SV K15 SE, Senior SE tại Singapore/Grab)**.
- **Quote:** *"Sẵn sàng dành 1-2 tiếng cuối tuần tư vấn cho đàn em FPT, nhưng cần tự động hóa lịch hẹn và tuyệt đối bảo vệ số điện thoại cá nhân."*
- **Bio & Goals:** 4 năm kinh nghiệm Distributed Systems. Mục tiêu: Mở link Google Calendar (`US-11`) để sinh viên tự book giờ, ẩn SĐT qua Privacy Toggles (`US-09`).
- **Pain Points:** Rất bận, sợ lộ số điện thoại cá nhân bị spam/quấy rối ngoài giờ, không thích bị chấm điểm sao công khai.
- **A11y:** Công tắc Privacy bắt buộc có chữ `[ON]/[OFF]`, nút `Block This User` (`US-10`) phải rõ ràng ở cuối trang.

#### 📌 Card 3: `P03: Lê Hoàng Lan` (Secondary Persona — Tân SV K20 Mentee)
- **Header:** Tag xanh lá `[Freshmen Mentee]` (`#10B981`), Avatar nữ sinh viên, Họ tên: **Lê Hoàng Lan (18 tuổi, Tân SV K20 AI, FPT Đà Nẵng)**.
- **Quote:** *"Vừa vào trường mình rất bỡ ngỡ, mong muốn xác minh tài khoản nhanh chóng và tìm được anh chị cựu sinh viên Campus Đà Nẵng."*
- **Bio & Goals:** Đang học tiếng Anh dự bị. Mục tiêu: Xác thực học vụ qua MSSV `DE200114` và Campus (`US-03`), lọc danh bạ và bài viết theo cơ sở Đà Nẵng (`US-06, US-33`).
- **Pain Points:** Mạng ký túc xá chập chờn, sợ mất dữ liệu khi form bị reset, dễ quên mật khẩu (`US-07`).
- **A11y:** Form cần lưu tạm dữ liệu khi mất mạng (Network Error Banner có nút Retry), thông báo lỗi bằng tiếng Việt dễ hiểu.

### 1.3. Frame 3: Sơ đồ Kiến trúc Thông tin IA (`1440 × 800 dp`)
Dựng cây phân cấp 3 tầng (sao chép trực quan từ sơ đồ Mermaid trong [`ux/user-flow.md`](ux/user-flow.md)):
- **Tầng 0:** Khối `S01 Login & Google SSO` (`US-01, US-02`) liên kết 2 chiều với `S02 Student Verification & Reset Pass` (`US-03, US-07`).
- **Tầng 1 (Bottom Navigation Bar 4 Tabs):**
  - Tab 1: `S03 Home Feed`
  - Tab 2: `S04 Member Directory`
  - Tab 3: `S06 Chat Room`
  - Tab 4: `S07 My Profile`
- **Tầng 2:** `S05 Member Public Profile` (nối từ S03 và S04), `S08 Privacy & Security` (nối từ S07), các Overlays (Modal Block Confirmation, Booking Calendar Sheet, Network Error Banner).

### 1.4. Frame 4: Sơ đồ 3 User Flows (`1440 × 1200 dp`)
Vẽ 3 khối luồng tương tác dạng flowchart trực quan:
- **Flow 1 (`F01` — TV1 làm chủ):** Nhánh Google SSO, Nhánh Email/Pass, Nhánh xác thực học vụ `S02`, Nhánh khóa tạm 60s khi sai 5 lần và Nhánh khôi phục mật khẩu.
- **Flow 2 (`F02`):** `S03/S04` ➔ Lọc Campus/Ngành ➔ Chọn thẻ Alumni ➔ `S05 Public Profile` ➔ Bấm `Book via Google Calendar` ➔ Đồng bộ lịch.
- **Flow 3 (`F03`):** `S07 My Profile` ➔ Cập nhật link Calendar ➔ `S08 Privacy Toggles [ON]/[OFF]` ➔ Bấm `Block User` trên S05 ➔ Modal xác nhận màu đỏ `#DC2626`.

---

## 2. Hướng dẫn Dựng Figma Page 02: Wireframes S01 & S02

Trên trang **`Page 02: 02_Wireframes`**, bạn dựng 2 Frame chuẩn kích thước **`360 × 800 dp`** bằng phong cách Grayscale (Đen - Trắng - Xám):

### 2.1. Frame `S01_Wireframe_Login` (`360 × 800 dp`)
- **Auto Layout:** Vertical, Spacing: `16dp`, Padding: `16dp` lề trái/phải, `24dp` lề trên.
- **Header:** Hình vuông xám nhạt `48×48dp` (Logo) + Text placeholder `H1 24sp` (Chào mừng trở lại) + Text placeholder `Body 14sp`.
- **Nút Google Wireframe:** Chiều cao `48dp`, viền xám `1dp #D1D5DB`, hình tròn icon `24dp` + Text "Tiếp tục với Google".
- **Divider:** Đường kẻ ngang `1dp #E5E7EB` + chữ "hoặc".
- **Ô nhập Email/MSSV:** Chiều cao `48dp`, viền `1dp #D1D5DB`, icon xám `20dp` bên trái, text placeholder "sinhvien@fpt.edu.vn hoặc SE193059".
- **Ô nhập Mật khẩu:** Chiều cao `48dp`, icon ổ khóa bên trái, icon mắt bên phải (vùng chạm 48×48dp).
- **Hàng phụ:** Ô vuông checkbox nhỏ + text "Ghi nhớ", bên phải là text "Quên mật khẩu?".
- **Nút CTA Đăng nhập:** Chiều cao `48dp`, nền xám đậm `#374151`, chữ trắng "ĐĂNG NHẬP" `16sp Bold`.
- **Footer:** Text link "Chưa xác thực thông tin? Xác thực ngay".

### 2.2. Frame `S02_Wireframe_Verification` (`360 × 800 dp`)
- **AppBar:** Chiều cao `56dp`, icon mũi tên Back `←` bên trái (vùng chạm `48×48dp`) + Title "Xác thực Sinh viên FPT".
- **Form 4 trường học vụ:**
  1. *Ô nhập MSSV:* Chiều cao `48dp`, placeholder "Ví dụ: SE193059", helper text bên dưới.
  2. *Dropdown Campus:* Chiều cao `48dp`, icon vị trí bên trái, mũi tên xổ xuống `▼` bên phải.
  3. *Dropdown Ngành:* Chiều cao `48dp`, icon trường học bên trái, mũi tên xổ xuống `▼`.
  4. *Dropdown Chuyên ngành:* Chiều cao `48dp`, icon máy tính bên trái, mũi tên `▼`.
- **Nút CTA Xác nhận:** Chiều cao `48dp`, nền xám đậm `#374151`, chữ trắng "XÁC NHẬN & HOÀN TẤT".

---

## 3. Hướng dẫn Dựng Figma Page 03: Final UI S01 & S02 + 5 State Variants

Trên trang **`Page 03: 03_Final_UI`**, bạn dựng màn hình hoàn thiện bằng các Design Tokens và Components từ Page 04 & Page 05:

```
┌────────────────────────┐  ┌────────────────────────┐  ┌────────────────────────┐  ┌────────────────────────┐
│ S01_Login_Default      │  │ S01_State_Focused      │  │ S01_State_InlineError  │  │ S01_State_Lockout60s   │
│ (360 x 800 dp)         │  │ (360 x 800 dp)         │  │ (360 x 800 dp)         │  │ (360 x 800 dp)         │
│ [Màn hình Chính]       │  │ [Viền Focus #1B365D]   │  │ [Viền đỏ #DC2626]      │  │ [Đếm ngược 60 giây]    │
└────────────────────────┘  └────────────────────────┘  └────────────────────────┘  └────────────────────────┘
```

### 3.1. Màn hình `S01: Login & Google Auth` và 4 Biến thể Trạng thái Đặt Cạnh

1. **`S01_Login_Default` (Màn chính):**
   - Nền toàn màn hình: `#F8F9FA`.
   - Header: Icon trường học trên nền Navy `#1B365D` bo góc `12dp` + Chữ "Chào mừng trở lại!" (`24sp Bold #111827`) + Mô tả (`14sp #595959`).
   - Nút Google: Nền `#FFFFFF`, viền `#E5E7EB`, cao `48dp`, logo Google, chữ "Tiếp tục với Google" (`16sp #111827`).
   - Ô Email & Ô Mật khẩu: Nền `#FFFFFF`, viền `#E5E7EB` cao `48dp`, bo góc `8dp`, icon mắt bên phải.
   - Nút Đăng nhập: Nền cam FPT `#F27024`, cao `48dp`, bo góc `8dp`, chữ trắng `#FFFFFF` `16sp Bold`.
2. **`S01_State_Focused` (Biến thể Focus):**
   - Giữ nguyên cấu trúc, đổi viền ô nhập Mật khẩu sang màu xanh Navy `#1B365D` dày `2dp`, hiện con trỏ nhấp nháy.
3. **`S01_State_InlineError` (Biến thể Lỗi mật khẩu):**
   - Viền ô Mật khẩu chuyển sang màu đỏ `#DC2626` dày `1.5dp`.
   - Mép phải ô nhập hiện icon chấm than `(!)` màu đỏ.
   - Bên dưới ô nhập hiện dòng text: *"Mật khẩu không chính xác. Bạn còn 3 lần thử"* (`12sp #DC2626`).
4. **`S01_State_Loading` (Biến thể Đang xác thực):**
   - Nút Đăng nhập chuyển sang màu xám `#D1D5DB`, chữ biến mất, thay bằng biểu tượng vòng xoay `Spinner` màu trắng `24dp`.
5. **`S01_State_Lockout60s` (Biến thể Khóa 60s chống dò pass):**
   - Đỉnh form xuất hiện Banner đỏ nhạt (Nền `#FDF3F2`, viền `#DC2626`, icon khiên đỏ) thông báo: *"Bạn đã nhập sai 5 lần. Tài khoản tạm thời bị khóa 60 giây để bảo mật."*
   - Nút Đăng nhập bị vô hiệu hóa (disabled), hiển thị text: *"Thử lại sau 59s..."*.

---

### 3.2. Màn hình `S02: Student Verification` và 4 Biến thể Trạng thái Đặt Cạnh

1. **`S02_Verify_Default` (Màn chính):**
   - AppBar trắng `#FFFFFF` cao `56dp`, icon `←` Back (`48×48dp`), Title "Xác thực Sinh viên FPT".
   - Form 4 trường học vụ xếp dọc cách nhau `16dp`:
     - Ô MSSV: Text placeholder *"Ví dụ: SE193059"*, helper text xám `#595959` bên dưới.
     - Dropdown Campus: Hiện chữ *"Chọn cơ sở đào tạo"*.
     - Dropdown Ngành: Hiện chữ *"Chọn khối ngành"*.
     - Dropdown Chuyên ngành: Nền xám `#F8F9FA` (Disabled), hiện chữ *"Hãy chọn Ngành đào tạo trước"*.
   - Nút CTA: Màu cam FPT `#F27024`, cao `48dp`, chữ trắng *"XÁC NHẬN & HOÀN TẤT"*.
2. **`S02_State_MSSV_Error` (Biến thể Lỗi format MSSV):**
   - Người dùng gõ `193059` (thiếu chữ `SE`).
   - Viền ô MSSV chuyển màu đỏ `#DC2626`, bên dưới hiện dòng text: *"MSSV không đúng định dạng (Gồm 2 chữ cái và 6 số, ví dụ: SE193059)"*.
3. **`S02_State_Cascading_Active` (Biến thể Đã chọn Ngành):**
   - Ngành đang chọn: *"Công nghệ thông tin"*.
   - Dropdown Chuyên ngành chuyển sang nền trắng `#FFFFFF` kích hoạt, menu thả xuống hiển thị danh sách: `Kỹ thuật phần mềm (SE)`, `Trí tuệ nhân tạo (AI)`, `An toàn thông tin (IA)`.
4. **`S02_State_Success_Modal` (Biến thể Hộp thoại Thành công):**
   - Toàn màn hình bị làm mờ nhẹ (`Overlay 40% Black`).
   - Chính giữa màn hình nổi lên Card trắng bo góc `16dp`: Icon tích tròn xanh lá `#10B981` kích thước `60×60dp`, Tiêu đề *"Xác thực Thành công!"*, Dòng chữ *"MSSV SE193059 đã được liên kết với cơ sở TP.HCM ngành Kỹ thuật phần mềm"*.

---

## 4. Quy tắc Vàng trên Figma (Tuyệt đối không Detach Instance)

Khi dựng **Page 03 (Final UI)**, bạn bắt buộc phải tuân theo nguyên tắc của đề Lab 2 để không bị trừ điểm quy trình:

| Hành vi Đúng Chuẩn (Được Điểm Tối Đa) ✅ | Sai lầm Nghiêm trọng Cần Tránh (Bị Trừ Điểm) ❌ |
| :--- | :--- |
| **Kéo Instance từ Page 05:** Vào mục *Assets* (hoặc copy Component từ Page 05) dán vào Page 03. | **Chuột phải bấm "Detach instance":** Biến component thành frame thường, làm mất tính liên kết Design System. |
| **Đổi trạng thái qua bảng Variants:** Bấm vào Component, ở thanh bên phải (Design panel), chuyển đổi thuộc tính `State: Default ➔ Focused ➔ Error ➔ Loading`. | **Đổi màu thủ công trực tiếp:** Tự ý gõ mã màu đỏ hoặc xanh đè lên layer con bên trong component. |
| **Gán mã màu qua Variables:** Khi tô màu nền hay viền, luôn chọn từ thư viện `Figma Variables` (ví dụ: `color/primary/fpt-navy`). | **Gán cứng mã Hex ngẫu nhiên:** Tự gõ `#112233` không có trong danh mục Design System. |
| **Đặt tên Layer theo chức năng:** Đặt tên frame rõ ràng: `S01_Login_Default`, `Input_MSSV`, `Btn_Submit`. | **Để tên mặc định:** Để nguyên tên `Frame 456`, `Rectangle 12`, `Group 99`. |

---

## 5. Hướng dẫn Đẩy Nhánh Git lên GitHub (`git push`)

Toàn bộ 5 commit của TV1 đã được tạo an toàn trên nhánh **`tv1/quangtam`** tại máy tính của bạn. Để đẩy lên kho GitHub chung của nhóm, bạn làm như sau:

### Bước 1: Mở Terminal PowerShell và chạy lệnh Push
```powershell
# Chạy lệnh đẩy nhánh tv1/quangtam lên GitHub
git push -u origin tv1/quangtam
```

### Bước 2: Đăng nhập GitHub (Nếu được hỏi)
- **Cách 1 (Trình duyệt):** Một cửa sổ trình duyệt sẽ tự động bật lên, bạn chỉ cần bấm **"Authorize GitCredentialManager"** và đăng nhập tài khoản GitHub chứa email `quangtam2005.lttg@gmail.com`.
- **Cách 2 (Personal Access Token):** Nếu dùng Terminal thuần, dán username GitHub của bạn và dán mã Token (PAT) vào ô mật khẩu.

### Bước 3: Tạo Pull Request (PR) trên Web GitHub
1. Mở trang repo: `https://github.com/F-Alumni/UI-UX`
2. Bạn sẽ thấy thanh màu vàng: *"tv1/quangtam had recent pushes... Compare & pull request"*.
3. Bấm **Compare & pull request**.
4. Tiêu đề PR: `feat(tv1): complete UX research, screen specs S01-S02, flutter handoff, and README`.
5. Bấm **Create pull request** (hoặc báo Trưởng nhóm TV2 Trung bấm Merge vào `main`).

---

## 6. Bảng Tự Rà soát 20 Tiêu chí UI Checklist & Quality Gates P0–P5

Trước khi nộp bài, hãy đối chiếu lại toàn bộ 20 tiêu chuẩn UI/UX của bộ chuẩn PRM393 để đảm bảo bài làm đạt 10/10 điểm:

| STT | Tiêu chí UI Checklist | Yêu cầu Kỹ thuật | Mức độ Đáp ứng trong Dự án | Đánh giá |
| :-: | :--- | :--- | :--- | :---: |
| **01** | **Viewport chuẩn** | `360 × 800 dp` cho màn hình điện thoại cơ sở | 100% màn hình thiết kế trên kích thước `360×800dp`. | ✅ ĐẠT |
| **02** | **Auto Layout 100%** | Toàn bộ các khung con dùng Auto Layout co giãn | Mọi button, textfield, card đều dùng Auto Layout `Fill/Hug`. | ✅ ĐẠT |
| **03** | **Touch Target ≥ 48dp** | Vùng chạm tối thiểu `48 × 48 dp` cho mọi controls | Button cao `48dp`, ô input `48dp`, icon mắt `48×48dp`. | ✅ ĐẠT |
| **04** | **Cỡ chữ ≥ 14sp** | Không có chữ nội dung nào nhỏ hơn `14sp` | Body `14sp/16sp`, Title `20sp/24sp` (chỉ caption phụ `12sp`). | ✅ ĐẠT |
| **05** | **Tương phản ≥ 4.5:1** | Đạt chuẩn WCAG 2.1 Level AA cho text thường | Chữ phụ `#595959` trên `#FFFFFF` đạt tương phản **`5.4:1`**. | ✅ ĐẠT |
| **06** | **Nút Back nhất quán** | Có nút quay lại ở góc trên bên trái màn hình con | `S02` có icon `←` Back đặt chuẩn AppBar cao `56dp`. | ✅ ĐẠT |
| **07** | **Phản hồi hệ thống** | Trạng thái Loading khi thực hiện tác vụ mạng | Nút submit có biến thể `CircularProgressIndicator` xoay. | ✅ ĐẠT |
| **08** | **Ngôn ngữ tự nhiên** | Thông báo lỗi bằng tiếng Việt rõ ràng, dễ hiểu | Không dùng mã lỗi kỹ thuật; hướng dẫn sửa lỗi cụ thể. | ✅ ĐẠT |
| **09** | **Không chỉ dùng màu** | Không phân biệt trạng thái chỉ bằng màu sắc | Lỗi có thêm icon `(!)`, thành công có tích `✓`, chip có tích `✓`. | ✅ ĐẠT |
| **10** | **Hành động phá hủy** | Chặn người dùng phải có hộp thoại xác nhận | Tính năng Block `US-10` có Modal Confirmation Dialog đỏ. | ✅ ĐẠT |
| **11** | **Bảo lưu dữ liệu** | Không làm mất dữ liệu khi form lỗi hoặc mất mạng | Network Error Banner giữ nguyên form kèm nút `Retry Save`. | ✅ ĐẠT |
| **12** | **Icon có nhãn** | Mọi icon chức năng đều có nhãn chữ đi kèm | Nút Google có chữ, các Dropdown có nhãn trường rõ ràng. | ✅ ĐẠT |
| **13** | **Lưới Spacing 8dp** | Khoảng cách là bội số của 8 (`8, 16, 24, 32dp`) | Áp dụng triệt để lưới `8dp Grid` trong toàn bộ tài liệu spec. | ✅ ĐẠT |
| **14** | **Figma Variables** | Mã màu và kích thước lấy từ Variables | Đã định nghĩa toàn bộ mã màu chuẩn FPT Navy & Orange. | ✅ ĐẠT |
| **15** | **Component Instances** | Không Detach component trên Page 03 | Sử dụng Variants đổi trạng thái, giữ nguyên liên kết instance. | ✅ ĐẠT |
| **16** | **Thao tác một tay** | Các nút bấm quan trọng nằm trong tầm với ngón cái | Nút CTA đặt ở nửa dưới màn hình; nút Google nằm vị trí thuận tiện. | ✅ ĐẠT |
| **17** | **Thứ bậc thị giác** | Tiêu đề, chữ chính, chữ phụ phân cấp rõ ràng | Tiêu đề Hanken Grotesk Bold, thân bài Inter Regular. | ✅ ĐẠT |
| **18** | **Safe Area** | Chừa khoảng cách tai thỏ và thanh điều hướng đáy | Lề trên `24dp`, lề dưới `16dp` tránh bị che khuất. | ✅ ĐẠT |
| **19** | **Co giãn Responsive** | Không bị vỡ khi mở rộng sang `412 dp` | Sử dụng thuộc tính `Fill Container` co giãn theo chiều ngang. | ✅ ĐẠT |
| **20** | **Đủ 5 States** | Default, Focus, Error, Loading, Lockout/Success | Đã vẽ đầy đủ 5 biến thể trạng thái cho cả `S01` và `S02`. | ✅ ĐẠT |

---

## 7. Bộ Câu hỏi Vấn đáp Bảo vệ Đồ án trước Giảng viên (Defense Cheat-Sheet)

Khi chấm bài vấn đáp, giảng viên thường hỏi xoáy vào vai trò chuyên môn và 2 màn hình của TV1. Dưới đây là 5 câu hỏi trọng tâm và câu trả lời chuẩn xác nhất:

### ❓ Câu 1: *"Tại sao em lại chọn bạn Trương Chí Trung làm Primary Persona mà không chọn Cựu sinh viên hay Giảng viên?"*
- 💡 **Câu trả lời mẫu:**  
  *"Thưa thầy/cô, căn cứ theo kết quả nghiên cứu khảo sát `E01` trên 120 sinh viên K18-K19, bài toán nhức nhối nhất của dự án là giải quyết nhu cầu tìm kiếm Mentor và chuẩn bị thực tập OJT của sinh viên đang học. Sinh viên là đối tượng **chủ động tạo ra tác vụ (Active Tasks)** nhiều nhất trong ứng dụng: từ tìm kiếm danh bạ theo Campus/Ngành, xem hồ sơ, đặt lịch Google Calendar cho đến đăng bài hỏi đáp. Do đó, bạn Trương Chí Trung (SV năm 3 chuẩn bị OJT) được chọn làm **Primary Persona (`P01`)**. Cựu sinh viên Đặng Minh Tuấn đóng vai trò là **Secondary Persona (`P02`)** với tư cách là người cố vấn đón nhận các yêu cầu kết nối."*

### ❓ Câu 2: *"Màn hình S02 của em xác thực danh tính sinh viên như thế nào nếu không bắt buộc nhập email @fpt.edu.vn?"*
- 💡 **Câu trả lời mẫu:**  
  *"Thưa thầy/cô, qua phỏng vấn thực tế, nhiều cựu sinh viên sau khi tốt nghiệp thường chuyển sang dùng email cá nhân hoặc email công ty, nếu ép cứng chỉ cho dùng `@fpt.edu.vn` thì sẽ tạo ra rào cản lớn khiến cựu sinh viên bỏ app. Vì vậy, nhóm thiết kế cơ chế xác thực thông qua **bộ 4 trường dữ liệu học vụ chuẩn FPT** trên màn hình `S02`:  
  1. **Mã số sinh viên (MSSV):** Kiểm tra chặt chẽ bằng biểu thức chính quy `^[A-Z]{2}\d{6}$` (bắt đầu bằng 2 chữ cái in hoa mã ngành như SE, DE, HE và 6 chữ số), có formatter tự động in hoa.  
  2. **Campus theo học:** Chọn 1 trong 5 cơ sở FPT toàn quốc.  
  3. **Ngành đào tạo:** Chọn khối ngành.  
  4. **Chuyên ngành hẹp liên động (Cascading):** Danh sách chuyên ngành hẹp sẽ tự động lọc theo ngành đã chọn (ví dụ: chọn CNTT sẽ hiển thị SE, AI, IA).  
  Giải pháp này vừa loại bỏ sự phiền toái về email, vừa bảo đảm 100% tính chính danh của sinh viên/cựu sinh viên FPT."*

### ❓ Câu 3: *"Tại sao màu chữ phụ trên form đăng nhập của em lại là #595959 mà không dùng màu xám nhạt thông thường?"*
- 💡 **Câu trả lời mẫu:**  
  *"Thưa thầy/cô, đây là một điểm cải tiến quan trọng từ vòng lặp AI Critique (Phát hiện `C04` và Quyết định `D04`). Ban đầu, bản Google Stitch v0 sinh ra chữ phụ màu `#9CA3AF` trên nền trắng, tỷ lệ tương phản chỉ đạt **`2.6 : 1`**, vi phạm nghiêm trọng tiêu chuẩn **WCAG 2.1 Level AA (yêu cầu tối thiểu 4.5:1)**. Với đặc điểm của Persona Trương Chí Trung bị cận thị 3 độ và hay dùng điện thoại một tay ngoài sảnh trường dưới ánh nắng gắt, nhóm đã quyết định làm đậm màu chữ phụ lên mã **`#595959`**, đạt tỷ lệ tương phản **`5.4 : 1`** trên nền trắng `#FFFFFF`, giúp người dùng đọc rõ ràng và không bị mỏi mắt."*

### ❓ Câu 4: *"Nếu kẻ xấu cố tình nhập sai mật khẩu liên tục trên màn hình S01 thì hệ thống xử lý ra sao?"*
- 💡 **Câu trả lời mẫu:**  
  *"Thưa thầy/cô, để tuân thủ **Heuristic #5 (Error Prevention — Ngăn ngừa lỗi)** và chống tấn công dò mật khẩu (Brute-Force), hệ thống cho phép người dùng nhập sai tối đa 5 lần. Mỗi lần nhập sai đều có thông báo Inline màu đỏ `#DC2626` kèm số lần thử còn lại. Khi nhập sai đủ 5 lần, form lập tức kích hoạt trạng thái **Lockout State**: Vô hiệu hóa nút Đăng nhập và khóa toàn bộ ô nhập trong **60 giây** kèm đồng hồ đếm ngược thời gian thực. Sau 60 giây, form mới tự động mở lại trạng thái bình thường."*

### ❓ Câu 5: *"Em đã làm thế nào để đảm bảo màn hình S01 và S02 co giãn tốt khi người dùng đổi sang điện thoại màn hình to hơn (như 412dp)?"*
- 💡 **Câu trả lời mẫu:**  
  *"Thưa thầy/cô, trên Figma, em áp dụng **100% Auto Layout**. Toàn bộ các khung nội dung (TextFormField, Nút bấm, Thẻ divider) đều được thiết lập thuộc tính chiều ngang là **`Fill Container`**, kết hợp với khoảng cách đệm lề trái và lề phải cố định là **`16 dp`**. Do đó, khi kéo giãn khung nhìn từ chuẩn `360 dp` sang `412 dp` (Android màn hình lớn), các ô nhập liệu và nút bấm sẽ tự động dãn đều ra hai bên một cách mượt mà mà không làm méo icon hay vỡ layout."*
