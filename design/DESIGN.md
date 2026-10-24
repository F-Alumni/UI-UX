# 🎨 Design System Specification (`DESIGN.md`) — FPT Alumni Connect

> **Môn học:** PRM323 — Cross-Platform Application Development  
> **Bài thực hành:** Lab 2 — Thiết kế UI/UX có hỗ trợ AI (Google Stitch → Figma)  
> **Dự án:** **FPT Alumni Connect** — Nền tảng kết nối Cựu sinh viên & Sinh viên Đại học FPT  
> **Người thực hiện chính (Lead Author):** **Nguyễn Phương Khải** — MSSV: **SE183590** (Vai trò: **TV3 — Design System & Reusable Components Lead**)  
> **Đồng bộ Figma:** **Figma Page 04 (`04_Design_System`)** & **Figma Page 05 (`05_Components`)**  
> **Chuỗi truy vết (Traceability Chain):** `P01..P03 (Personas) → US-01..US-11 (User Stories) → I01..I03 (Stitch Iterations) → D01..D07 (Decisions) → Token & Component System`

---

## 1. Triết lý Thiết kế & Định hướng Thị giác (Design Philosophy & Visual Direction)

Hệ thống thiết kế (**Design System**) của **FPT Alumni Connect** được xây dựng theo phong cách **"Modern Academic & Executive Mentorship"** — kết hợp sự tin cậy, chuẩn mực học thuật của **FPT Navy Blue (`#1B365D`)** với năng lượng kết nối, nhiệt huyết của **FPT Signature Orange (`#F27024`)**.

Mọi quy tắc thị giác trong tài liệu này được dẫn xuất trực tiếp từ nhu cầu thực tế của **Primary Persona Trương Chí Trung (`P01`)** và kết quả tinh chỉnh sau 3 vòng lặp AI trên Google Stitch (`I01`–`I03`):
1. **Khả năng đọc ngoài trời nắng gắt (High-Contrast Outdoor Readability):** Sinh viên thường tra cứu mentor, xem lịch rảnh tham khảo và nhắn tin trao đổi khi đang di chuyển ở sảnh trường hoặc trên xe buýt. Do đó, nền ứng dụng sử dụng tông sáng sạch (`#F8F9FA` / `#FFFFFF`), chữ chính đạt tương phản tối đa (`#111827` — **17.7:1**) và chữ phụ được làm đậm lên `#595959` (**7.0:1**, vượt xa ngưỡng tối thiểu 4.5:1 của WCAG 2.1 AA theo quyết định `D04`).
2. **Thao tác một tay bằng ngón cái (One-Handed Thumb Ergonomics):** Toàn bộ nút bấm, chip lọc (`Campus`, `Major`), công tắc bật/tắt (`Switch`) và mục điều hướng đều tuân thủ nghiêm ngặt vùng chạm tối thiểu **`48 × 48 dp`** trên lưới không gian **`8dp`** (quyết định `D04`).
3. **Thứ bậc hành động đơn nhất (Single Dominant CTA Hierarchy):** Mỗi màn hình chỉ có duy nhất một loại hành động chính mang màu cam đặc (`#F27024`), các hành động thứ cấp bắt buộc dùng kiểu viền (`Outlined`) hoặc nền nhạt (`Tonal`) để tránh cạnh tranh thị giác (quyết định `D02`).
4. **Không truyền đạt trạng thái chỉ bằng màu sắc (Zero Color-Only Indicators — WCAG SC 1.4.1):** Mọi trạng thái đang chọn, bật/tắt, đã xác thực hoặc báo lỗi đều kết hợp **Màu sắc + Biểu tượng (Icon) + Nhãn văn bản tường minh** (`✓`, `[ON]`/`[OFF]`, `Verified`, `Validation Error`) theo quyết định `D05`.

---

## 2. Hệ thống Biến Màu sắc & Kiểm chứng Tương phản (Color Tokens & WCAG 2.1 AA Matrix)

Toàn bộ màu sắc được khai báo dưới dạng **Figma Color Variables** trên **Page 04 (`04_Design_System`)**. Tuyệt đối không gán mã màu Hex tự do (hard-code) bên ngoài bảng Token này.

### 2.1. Bảng Danh mục Color Tokens (Primitive & Semantic Tokens)

| Nhóm Token | Tên Biến Figma (Figma Variable) | Mã Hex | Vai trò Ngữ nghĩa & Quy tắc Sử dụng trên Giao diện |
| :--- | :--- | :---: | :--- |
| **Brand Primary** | `color/brand/primary-navy` | `#1B365D` | Màu chủ đạo học thuật: Tiêu đề phần, thanh điều hướng, Filter Chip đang chọn, nút phụ viền (Secondary Outlined), huy hiệu xác thực trường. |
| **Brand Primary Tint** | `color/brand/navy-tint` | `#EEF2FF` | Nền nút Tonal (`Direct Message`, `Quick DM`), nền hộp thông tin xác thực Alumni, nền tag kỹ năng/chủ đề cố vấn. |
| **Brand Primary CTA** | `color/brand/cta-orange` | `#F27024` | Màu hành động chính (Primary CTA): Nút `💬 Nhắn tin trao đổi`, `View Profile`, `Save Changes`, và biểu tượng tab đang kích hoạt. |
| **Brand CTA Tint** | `color/brand/orange-tint` | `#FFF7ED` | Nền thông báo mùa OJT, nền huy hiệu `Recruiter Alumni` hoặc `Present`. |
| **Status — Verified / Success** | `color/status/verified-emerald` | `#10B981` | Biểu tượng tích xanh `Verified Alumni`, chấm trạng thái `Open for Mentoring`, trạng thái bật (`[ON]`) của công tắc Switch. |
| **Status — Verified Surface** | `color/status/emerald-tint` | `#D1FAE5` | Nền của pill trạng thái `OPEN FOR MENTORING` và nhãn `[ON]` cạnh Switch (chữ màu `#065F46`). |
| **Status — Error / Destructive** | `color/status/error-crimson` | `#DC2626` | Viền và chữ lỗi nhập liệu (`Inline Validation Error`), nút hành động nguy hiểm (`Block This User`, `Confirm Block`, `Retry Save`). |
| **Status — Error Surface** | `color/status/error-tint` | `#FDF3F2` | Nền của `Network Error Banner` và hộp thông báo lỗi đồng bộ lịch Google Calendar (`US-11`). |
| **Surface — App Background** | `color/surface/background` | `#F8F9FA` | Nền tổng thể của các màn hình (`Scaffold` background) giúp tách biệt rõ các thẻ nội dung màu trắng. |
| **Surface — Card / Modal** | `color/surface/card` | `#FFFFFF` | Nền của tất cả thẻ thành viên, thẻ bài viết, AppBar, Bottom Navigation Bar và hộp thoại Dialog. |
| **Surface — Input / Muted** | `color/surface/input-fill` | `#F3F4F6` | Nền mặc định của ô tìm kiếm, ô nhập dữ liệu form và chip chưa được chọn. |
| **Text — Primary** | `color/text/primary` | `#111827` | Chữ tiêu đề, họ tên thành viên, nội dung bài viết, giá trị đang nhập trong Text Field. |
| **Text — Secondary** | `color/text/secondary` | `#595959` | Chữ mô tả phụ, thông tin khóa/ngành/cơ sở, nhãn thời gian, chú thích quyền riêng tư (đã nâng cấp từ `#9CA3AF` ở vòng lặp `I02`). |
| **Text — Inverse** | `color/text/on-dark` | `#FFFFFF` | Chữ và icon đặt trên nền đậm (`#1B365D`, `#DC2626`, `#F27024`). |
| **Border & Divider** | `color/border/default` | `#E5E7EB` | Đường viền thẻ (`1dp`), đường phân cách danh sách và viền mặc định của ô nhập liệu. |

---

### 2.2. Ma trận Kiểm chứng Độ tương phản theo W3C WCAG 2.1 Level AA

| Cặp Màu (Foreground trên Background) | Mã Hex (Chữ/Icon vs. Nền) | Tỷ lệ Tương phản (Contrast Ratio) | Ngưỡng Yêu cầu WCAG 2.1 AA | Kết quả Đánh giá | Ngữ cảnh Áp dụng Thực tế |
| :--- | :---: | :---: | :---: | :---: | :--- |
| `text/primary` trên `surface/card` | `#111827` trên `#FFFFFF` | **17.74 : 1** | ≥ 4.5 : 1 (Chữ thường) | ✅ **PASS (AAA)** | Tên Alumni, tiêu đề bài viết, nội dung bio, tin nhắn chat. |
| `text/secondary` trên `surface/card` | `#595959` trên `#FFFFFF` | **7.00 : 1** | ≥ 4.5 : 1 (Chữ thường) | ✅ **PASS (AAA)** | Dòng phụ đề Campus/Major/Cohort, mô tả dưới công tắc Privacy (`D04`). |
| `text/secondary` trên `surface/background` | `#595959` trên `#F8F9FA` | **6.64 : 1** | ≥ 4.5 : 1 (Chữ thường) | ✅ **PASS (AA+)** | Nhãn phân mục phụ ngoài thẻ, số lượng kết quả tìm kiếm. |
| `text/on-dark` trên `brand/primary-navy` | `#FFFFFF` trên `#1B365D` | **12.15 : 1** | ≥ 4.5 : 1 (Chữ thường) | ✅ **PASS (AAA)** | Filter Chip đang chọn (`✓ HCMC`, `✓ Software Engineering`), nút Navy. |
| `brand/primary-navy` trên `brand/navy-tint` | `#1B365D` trên `#EEF2FF` | **10.68 : 1** | ≥ 4.5 : 1 (Chữ thường) | ✅ **PASS (AAA)** | Nút `Direct Message`, tag kỹ năng (`Go`, `Kubernetes`, `System Architecture`). |
| `status/error-crimson` trên `surface/card` | `#DC2626` trên `#FFFFFF` | **4.83 : 1** | ≥ 4.5 : 1 (Chữ thường) | ✅ **PASS (AA)** | Thông báo lỗi dưới ô Google Calendar URL, nút `Block This User` (`D01`, `D06`). |
| `status/error-crimson` trên `status/error-tint` | `#991B1B` / `#DC2626` trên `#FDF3F2` | **4.52 : 1 – 7.40 : 1** | ≥ 4.5 : 1 (Chữ thường) | ✅ **PASS (AA)** | Nội dung cảnh báo trong `Network Error Banner` (`S07`). |
| `text/on-dark` trên `brand/cta-orange` | `#FFFFFF` (Bold 15–16sp) trên `#F27024` | **3.12 : 1** *(UI Control / Bold Label)* | ≥ 3.0 : 1 (UI Component & Large/Bold Text) | ✅ **PASS (AA)** | Nút `Xem Lịch Google Calendar` kèm icon lịch rõ nét (`S05`). |

---

## 3. Hệ thống Kiểu chữ & Phân cấp Thông tin (Typography System)

Dự án sử dụng cặp phông chữ hiện đại, hỗ trợ đầy đủ tiếng Việt và tối ưu độ rõ nét trên màn hình mật độ điểm ảnh cao của Android/iOS:
- **Font Tiêu đề (Heading Font):** `Hanken Grotesk` — tạo cá tính công nghệ, gọn gàng, dễ quét nhanh bằng mắt.
- **Font Nội dung & Điều khiển (Body & UI Font):** `Inter` — độ mở chữ (aperture) lớn, dễ đọc ở kích thước nhỏ.
- **Quy tắc Bắt buộc (Hard Constraint):** Toàn bộ văn bản nội dung (Body text), thẻ kỹ năng (Skill tags), thông tin khóa/ngành và nhãn điều khiển đều **≥ `14sp`** (tuyệt đối không dùng chữ 10–11sp cho nội dung đọc theo quyết định `D04`).

| Tên Style (Figma Text Style) | Font Family | Trọng lượng (Weight) | Kích thước (`sp`) | Chiều cao dòng (`Line Height`) | Khoảng cách chữ (`Letter Spacing`) | Mục đích Sử dụng |
| :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| `type/display-profile` | `Hanken Grotesk` | Bold (`700`) | **`24sp`** | `32sp` (`4×8dp`) | `-0.3px` | Tên cựu sinh viên / sinh viên ở đầu trang `S05 Alumni Detail` & `S07 My Profile`. |
| `type/heading-h1` | `Hanken Grotesk` | Bold (`700`) | **`20sp`** | `28sp` | `-0.2px` | Tiêu đề màn hình chính (`S01 Login`), tiêu đề bài viết nổi bật trên `S03 Home`. |
| `type/heading-h2` | `Hanken Grotesk` | Bold (`700`) | **`18sp`** | `24sp` (`3×8dp`) | `-0.1px` | Tiêu đề AppBar (`Alumni Detail`, `Chat Room`), tiêu đề các khối thẻ (`About & Mentorship`, `Career History`). |
| `type/title-card` | `Inter` | SemiBold (`600`) | **`16sp`** | `24sp` (`3×8dp`) | `0px` | Tên thành viên trong thẻ `Directory` (`S04`), chức danh công việc (`Tech Lead at VNG Corporation`). |
| `type/body-large` | `Inter` | Regular (`400`) | **`16sp`** | `24sp` (`3×8dp`) | `0px` | Nội dung nhập trong Text Field, nội dung tin nhắn dài trên `S06 Chat`. |
| `type/body-medium` *(Chuẩn)* | `Inter` | Regular (`400`) | **`14sp`** | `20sp` | `0px` | Nội dung bài viết, tiểu sử (Bio), mô tả kinh nghiệm làm việc, dòng thông tin Campus/Major/Cohort. |
| `type/label-button` | `Inter` | SemiBold (`600`) | **`14sp` – `15sp`** | `20sp` | `+0.1px` | Nhãn trên tất cả các Button, Filter Chip (`48dp`), Segmented Switcher và nhãn trạng thái `[ON]`/`[OFF]`. |
| `type/label-tag` | `Inter` | Medium (`500`) | **`14sp`** | `20sp` | `0px` | Nhãn kỹ năng (`Go`, `Kubernetes`, `System Architecture`), huy hiệu khóa (`K12 Alumni`, `K19 Student`). |

---

## 4. Hệ thống Lưới Không gian, Bo góc & Responsive (8dp Grid, Radius & Breakpoints)

### 4.1. Thang Khoảng cách Lưới `8dp` (Spacing Variables)

Mọi khoảng cách lề (`padding`), khoảng cách giữa các phần tử (`gap`/`itemSpacing`) và kích thước khung đều là bội số của **`8dp`** (cho phép mức vi chỉnh `4dp` duy nhất cho khoảng cách giữa icon và nhãn bên trong một badge):

| Tên Biến Figma (`spacing/*`) | Giá trị (`dp`) | Quy tắc Áp dụng trong Auto Layout |
| :--- | :---: | :--- |
| `space/050` | **`4dp`** | Khoảng cách nội bộ giữa chấm trạng thái/icon nhỏ và nhãn chữ trong cùng một badge. |
| `space/100` | **`8dp`** | Khoảng cách giữa các dòng thông tin trong cùng một nhóm; khoảng cách giữa các chip kỹ năng (`Wrap gap`). |
| `space/150` | **`12dp`** | Khoảng cách dọc giữa các phần tử con bên trong một Card; khoảng cách giữa 2 nút đặt cạnh nhau. |
| `space/200` | **`16dp`** | **Padding ngang tiêu chuẩn của toàn màn hình (`Screen Horizontal Padding`)**; padding bên trong mọi Card (`16dp`); khoảng cách dọc giữa các Card. |
| `space/300` | **`24dp`** | Khoảng cách phân tách các nhóm chức năng lớn trên màn hình (`Section Gap`) và padding của Dialog. |
| `space/400` | **`32dp`** | Khoảng đệm cuối trang (`Bottom Scroll Safe Padding`) trước thanh điều hướng. |
| `space/600` | **`48dp`** | **Kích thước vùng chạm tối thiểu bắt buộc (`Minimum Touch Target Height = 48dp`)** cho mọi Button, Filter Chip, Input, Switch Row và Icon Action. |

### 4.2. Quy tắc Bo góc (Corner Radius Variables) & Đổ bóng (Elevation)

| Tên Biến Figma (`radius/*`) | Giá trị (`dp`) | Thành phần Áp dụng |
| :--- | :---: | :--- |
| `radius/sm` | **`8dp`** | Tất cả các loại Button (`Primary`, `Secondary`, `Destructive`), Text Field, Dropdown và Banner cảnh báo. |
| `radius/md` | **`12dp`** | Tất cả các loại Card (`Directory Card`, `Post Card`, `Profile Section Card`) và hộp thông tin xác thực. |
| `radius/lg` | **`16dp`** | Hộp thoại Modal Dialog (`Confirmation Dialog`) và Bottom Sheet. |
| `radius/full` | **`999dp`** | Filter Chips (`Campus`, `Major`), pill trạng thái (`OPEN FOR MENTORING`) và Avatar tròn. |

- **Elevation 1 (`shadow/card`):** `0dp 1dp 3dp rgba(17, 24, 39, 0.06)` kết hợp viền `1dp solid #E5E7EB` — giúp thẻ nổi nhẹ trên nền `#F8F9FA` mà không gây rối mắt.
- **Elevation 2 (`shadow/sticky-bar`):** `0dp -2dp 10dp rgba(17, 24, 39, 0.08)` — dùng cho thanh Bottom Navigation Bar và thanh nhập tin nhắn (`Chat Input Bar`).
- **Elevation 3 (`shadow/dialog`):** `0dp 12dp 24dp rgba(17, 24, 39, 0.18)` trên lớp phủ mờ `Scrim rgba(17, 24, 39, 0.50)` — dùng cho Dialog xác nhận.

### 4.3. Quy tắc Đáp ứng Đa Kích thước Màn hình (Responsive Breakpoints)

1. **`360 × 800 dp` (Compact Mobile — Chuẩn cơ sở thiết kế):**
   - Lề ngang `16dp` hai bên → vùng nội dung rộng `328dp`.
   - Các thẻ xếp 1 cột dọc (`Single-column vertical stack`); các dải Filter Chips (`CAMPUS`, `MAJOR`) cuộn ngang mượt mà với chiều cao mỗi chip cố định `48dp`.
2. **`412 × 915 dp` (Expanded Android Mobile — Kiểm chứng co giãn trực tiếp trên Figma):**
   - Toàn bộ các Frame màn hình dùng **Auto Layout `Fill Container`** cho chiều ngang của Card, Input và Button → tự động giãn vùng nội dung lên `380dp` (`412 - 32`).
   - Các thẻ kỹ năng (`Mentorship Topics`, `Skill Tags`) dùng chế độ **Auto Layout `Wrap`** nên tự động dồn thêm tag lên cùng một hàng, giảm chiều dài cuộn dọc.
3. **`≥ 600 dp` (Tablet / Foldable Screen Adaptation):**
   - Thanh điều hướng chuyển từ `Bottom Navigation Bar` sang `NavigationRail` bên trái (`80dp` width).
   - Màn hình `S04 Directory` + `S05 Alumni Detail` và `S06 Chat` chuyển sang bố cục **2 cột Master–Detail** (cột danh sách bên trái `320dp`, cột chi tiết hồ sơ hoặc khung chat bên phải `Expanded`, giới hạn độ rộng dòng văn bản tối đa `640dp` để mắt dễ đọc).

---

## 5. Đặc tả 9 Nhóm Component Tái sử dụng Bắt buộc (Figma Page 05 — `05_Components`)

Tất cả **9 nhóm Component** dưới đây được dựng bằng **100% Figma Auto Layout**, sử dụng **Component Properties & Variants** và liên kết trực tiếp tới bảng biến ở Mục 2 – Mục 4.

### 5.1. `CMP-01`: Button (Nút hành động)
- **Auto Layout:** Horizontal, ` Alignment: Center`, `Padding: 16dp (horizontal) × 12dp (vertical)`, `Item Spacing: 8dp`, `Min Height: 48dp` (riêng Primary CTA có phụ đề hai dòng trên `S05` cao `56dp`), `Corner Radius: 8dp`.
- **Các Variants bắt buộc:**
  1. `Variant=Primary-CTA`: Nền `#F27024`, chữ `#FFFFFF` (`15sp SemiBold`), icon trái/phải `20dp`. Dùng cho CTA chính duy nhất trên màn hình (`💬 Nhắn tin trao đổi`, `View Profile`, `Save Changes`).
  2. `Variant=Secondary-Navy`: Nền `#1B365D`, chữ `#FFFFFF`. Dùng cho hành động kết nối đồng cấp (`Connect as Peer`, `View Program Details`).
  3. `Variant=Secondary-Outlined`: Nền `#FFFFFF`, viền `1.5dp #1B365D`, chữ `#1B365D`. Dùng cho nút phụ viền `📅 Xem Lịch rảnh tham khảo` trên `S05` hoặc nút phụ trong thẻ bài viết trên `S03 Home` (theo quyết định `D02 & D07`).
  4. `Variant=Secondary-Tonal`: Nền `#EEF2FF`, chữ `#1B365D`, icon `20dp`. Dùng cho nút `Direct Message` (`S05`) và `Message` (`S04`).
  5. `Variant=Destructive-Outlined`: Nền `#FFFFFF`, viền `1.5dp #DC2626`, chữ `#DC2626`, icon `🚫`. Dùng cho nút `Block This User` (`US-10` trên `S05`).
  6. `Variant=Destructive-Solid`: Nền `#DC2626`, chữ `#FFFFFF`. Dùng cho nút `Confirm Block` trong Dialog và nút `Retry Save` trong Error Banner (`S07`).
- **States:** `Default`, `Pressed` (độ mờ lớp phủ 12%), `Disabled` (nền `#E5E7EB`, chữ `#595959`), `Loading` (hiển thị vòng xoay `18dp` + chữ `"Saving..."`).

### 5.2. `CMP-02`: Text Field, Filter Chip & Selection Controls (Điều khiển nhập liệu & lọc)
- **A. Text Field / Input Box:**
  - **Auto Layout:** Vertical stack (`Label 14sp SemiBold #111827` → `Input Container 48dp min-height, radius 8dp, padding 12dp×16dp` → `Helper/Error Text 14sp`).
  - **Variants theo State:**
    - `State=Default`: Nền `#F3F4F6`, viền `1dp #E5E7EB`, chữ `#111827`.
    - `State=Focused`: Nền `#FFFFFF`, viền `2dp #1B365D`.
    - `State=Error` *(Theo quyết định `D06`)*: Nền `#FDF3F2`, viền `2dp #DC2626`, icon lỗi bên trái, dòng thông báo đỏ `#DC2626` kèm icon `ⓘ` bên dưới (ví dụ: *"Invalid Google Calendar URL format. Please enter a valid calendar.google.com link"*).
- **B. Filter Chip (`CAMPUS` & `MAJOR` — `US-06`):**
  - **Auto Layout:** Horizontal, **`Height: 48dp` cố định** (theo quyết định `D04`), `Padding: 0dp×16dp`, `Gap: 8dp`, `Radius: 999dp`.
  - **Variants:**
    - `Selected=True` *(Theo quyết định `D05`)*: Nền `#1B365D`, chữ `#FFFFFF` (`14sp SemiBold`), **bắt buộc hiển thị icon dấu tích `✓` (`16dp`)** bên trái để không phụ thuộc màu sắc.
    - `Selected=False`: Nền `#F3F4F6`, viền `1dp #E5E7EB`, chữ `#111827` (`14sp Medium`).
- **C. Accessible Toggle Switch (`US-09`):**
  - **Auto Layout:** Hàng ngang cao tối thiểu `48dp`, bên phải gồm **Nhãn trạng thái văn bản tường minh `[ON]` (nền `#D1FAE5`, chữ `#065F46`) hoặc `[OFF]` (nền `#F3F4F6`, chữ `#595959`)** đặt ngay cạnh công tắc Switch `52×32dp` (theo quyết định `D05`).

### 5.3. `CMP-03`: Card (Thẻ nội dung đa năng)
- **Auto Layout:** Vertical, `Width: Fill Container (328dp ở màn 360dp)`, `Padding: 16dp`, `Gap: 12dp`, `Radius: 12dp`, `Background: #FFFFFF`, `Border: 1dp #E5E7EB`.
- **Các Variants:**
  1. `Type=Alumni-Directory-Card` (`S04`): Header (Avatar `56dp` + chấm online + Tên `16sp SemiBold` + Badge `✓ Verified` + Khóa/Chức danh + Trường/Ngành) → Dải Skill Tags (`Wrap`, chữ `14sp`) → Hộp thông tin lịch (`Google Calendar Link Available`) → Hàng 2 nút (`View Profile` cam + `Message` tonal).
  2. `Type=Feed-Post-Card` (`S03`): Status pill trên cùng → Thông tin tác giả → Nội dung bài chia sẻ (`14sp`) → Hộp tóm tắt buổi tư vấn → Hàng nút hành động (`Xem chi tiết` kiểu Outlined `#1B365D` + `💬 Nhắn tin trao đổi`) → Footer tương tác (`Like`, `Comment`, thời gian).
  3. `Type=Profile-Section-Card` (`S05`, `S07`): Tiêu đề mục có icon `20dp` + `Hanken Grotesk 18sp Bold` → Nội dung chi tiết bên trong (Bio, Mentorship Topics, Career History timeline, hoặc Form fields).

### 5.4. `CMP-04`: Navigation (Thanh điều hướng dưới & Bộ chuyển không gian)
- **A. Bottom Navigation Bar (`4 Tabs`):**
  - **Auto Layout:** Horizontal `Fill Container`, `Height: 64dp`, nền `#FFFFFF`, viền trên `1dp #E5E7EB`. Mỗi tab chiếm 1/4 chiều ngang (`90dp` trên màn `360dp` ≥ `48×48dp` touch target).
  - **4 Tabs:** `Home` (`S03`), `Directory` (`S04`), `Chat` (`S06` — có badge chấm cam thông báo tin nhắn mới), `Profile` (`S07`).
  - **Trạng thái Active:** Icon tô đặc màu cam `#F27024` + Nhãn chữ `12sp/14sp Bold #F27024` + gạch chỉ báo phía trên.
- **B. Segmented Space Switcher (`S03 Home`):**
  - Khung chuyển đổi 2 tab cao `48dp` giữa `🌐 General Campus` và `● Major Space (SE) K18`.

### 5.5. `CMP-05`: App Bar (Thanh tiêu đề trên cùng)
- **Quy tắc Cốt lõi (Quyết định `D01`):** Mỗi màn hình chỉ sử dụng **duy nhất 1 thanh App Bar** cao `56dp`, tuyệt đối không xếp chồng 2 thanh điều hướng lùi.
- **Các Variants:**
  1. `Variant=Top-Level-Brand`: Logo FPT Alumni Connect + Tên phân mục (`Home`, `Directory`, `Profile`) bên trái; nút Chuông thông báo (`48×48dp` hit area) + Avatar người dùng bên phải.
  2. `Variant=Detail-Back` (`S05 Alumni Detail`): Nút `← Back` (`48×48dp` touch target) + Logo nhỏ + Tiêu đề `Alumni Detail` (`18sp Bold`) + Avatar người dùng.
  3. `Variant=Chat-Room-Header` (`S06 Chat`): Nút `← Back` (`48×48dp`) + Avatar người nhận (`40dp` kèm chấm trạng thái `Online`) + Tên & Chức danh rút gọn (`Dang Minh Tuan • Tech Lead @ VNG`) + Nút mở nhanh lịch `📅` (`48×48dp`) và Menu `⋮` (`48×48dp`).

### 5.6. `CMP-06`: Dialog / Confirmation Modal (Hộp thoại xác nhận)
- **Mục đích:** Ngăn ngừa lỗi thao tác không thể hoàn tác theo *Nielsen Heuristic #5 (Error Prevention)*, đặc biệt cho tính năng **Chặn tài khoản (`US-10`)** trên `S05` và `S06`.
- **Cấu trúc Auto Layout:** Card rộng `328dp`, `Padding: 24dp`, `Gap: 16dp`, `Radius: 16dp`, nền `#FFFFFF`, đặt giữa lớp phủ `Scrim rgba(17,24,39,0.5)`:
  - Icon cảnh báo tròn đỏ (`48×48dp`, nền `#FDF3F2`, icon `🚫 #DC2626`).
  - Tiêu đề (`18sp Bold #111827`): *"Block Dang Minh Tuan?"*
  - Nội dung giải thích hậu quả rõ ràng (`14sp Regular #595959`): *"They will no longer be able to send you direct messages, see your online status, or view your shared Google Calendar schedule. You can unblock them anytime in Profile > Security & Account."*
  - Hàng 2 nút cao `48dp`: Nút phụ `Cancel` (Outlined `#1B365D`) bên trái và nút chính `Confirm Block` (Solid `#DC2626`) bên phải.

### 5.7. `CMP-07`: Loading State (Trạng thái đang tải dữ liệu)
- **Variants:**
  1. `Variant=Skeleton-Card`: Khung thẻ giữ nguyên kích thước thật của `Alumni Directory Card` hoặc `Alumni Detail Header` với các khối xám bo góc (`#E5E7EB` → `#F3F4F6` hiệu ứng shimmer) cho Avatar tròn, 3 thanh văn bản và 2 nút bấm. Giúp bố cục không bị giật (Zero Layout Shift) khi mạng chậm.
  2. `Variant=Inline-Spinner`: Vòng xoay `20dp` kèm nhãn *"Loading verified alumni..."* hoặc *"Opening Google Calendar..."* (`14sp #595959`).

### 5.8. `CMP-08`: Empty State (Trạng thái rỗng không có dữ liệu)
- **Cấu trúc Auto Layout:** Căn giữa theo trục dọc/ngang, `Padding: 32dp×24dp`, `Gap: 16dp`:
  - Minh họa icon kính lúp / hộp thư trống (`64×64dp`, nền `#EEF2FF`, màu `#1B365D`).
  - Tiêu đề (`18sp Bold #111827`): *"No Matching Alumni Found"* (trên `S04`) hoặc *"Start a Mentorship Conversation"* (trên `S06`).
  - Mô tả hướng dẫn (`14sp Regular #595959`): Giải thích lý do (ví dụ bộ lọc `Campus + Major` quá hẹp) hoặc gợi ý mẫu câu chào hỏi mentor.
  - Nút hành động phục hồi (`48dp` height): **`Reset All Filters`** (trên `S04`) hoặc **`Send Icebreaker Greeting`** (trên `S06`).

### 5.9. `CMP-09`: Error State (Trạng thái báo lỗi & phục hồi)
- **Tuân thủ *Nielsen Heuristic #9* & Quyết định `D06`:**
  1. `Variant=Network-Error-Banner`: Khung cảnh báo đầu trang (`#FDF3F2` background, `1.5dp #DC2626` border, `12dp` radius, `16dp` padding) có icon `!`, tiêu đề đậm *"Network connection lost while saving"*, giải thích *"Your profile changes are preserved locally. Please check your connection and retry."* và nút **`↻ Retry Save`** cao **`48dp`** màu `#DC2626`.
  2. `Variant=Inline-Field-Error`: Viền ô input đổi sang `2dp #DC2626` kèm dòng hướng dẫn sửa lỗi cụ thể ngay bên dưới và hộp trạng thái `⚠️ Validation Error • Not Synced` có nút `Retry Sync`.
  3. `Variant=Chat-Message-Failed`: Bong bóng tin nhắn hiển thị nhãn đỏ `⚠️ Failed to send • Tap to retry` (`48dp` hit target) khi rớt mạng trong phòng chat `S06`.
