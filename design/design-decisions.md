# ⚖️ Design Decisions, Token Rationale, Accessibility QA & UI Simplicity Checklist (`design-decisions.md`)

> **Môn học:** PRM323 — Cross-Platform Application Development  
> **Bài thực hành:** Lab 2 — Thiết kế UI/UX có hỗ trợ AI (Google Stitch → Figma)  
> **Dự án:** **FPT Alumni Connect** — Nền tảng kết nối Cựu sinh viên & Sinh viên Đại học FPT  
> **Căn cứ Quy chuẩn:** Bộ tiêu chuẩn [PRM393 UI/UX Material](https://prm393-material.web.app/) (Chuẩn chung `C1`–`C8`, Quality Gates `G0`–`G5`, UI Simplicity Checklist 20 tiêu chí).  
> **Nhóm tác giả phối hợp:**  
> - **Nguyễn Phương Khải (`SE183590` — TV3):** Quyết định Kiến trúc Design Tokens, 9 Reusable Components & Màn hình `S05`, `S06`  
> - **Trương Chí Trung (`SE193059` — TV2):** Quyết định Phản biện AI (`D01` – `D07`) từ Google Stitch  
> - **Đỗ Trọng Tín (`SE190218` — TV4):** Kiểm định Khả năng tiếp cận (WCAG 2.2 AA), UI Simplicity Checklist 20 tiêu chí & Đa kích thước màn hình (`360dp`, `412dp`, `≥600dp`)

---

## 1. Bảng Tổng hợp Quyết định Thiết kế từ Phản biện AI (`D01` – `D07` — Chuẩn `C3`)

Mỗi đề xuất từ bước phản biện AI (`C01` – `C07` trong [`ai/ai-design-log.md`](../ai/ai-design-log.md)) đã được nhóm phân tích, đối chiếu với **Persona Trương Chí Trung (`P01`)**, **Persona Đặng Minh Tuấn (`P02`)**, **10 Usability Heuristics của Jakob Nielsen** và **Phạm vi 11 User Stories (`US-01` → `US-11`)** để đưa ra quyết định **ACCEPT**, **MODIFY** hoặc **REJECT**:

| Mã QĐ | Màn hình & Component Liên quan | Vấn đề trên Bản `v0` & Đề xuất của AI | Quyết định | Lý do Lập luận & Cách Hiện thực hóa trong Figma / Design System |
| :---: | :--- | :--- | :---: | :--- |
| **`D01`** | **`S05` Alumni Detail**<br>(`CMP-05 App Bar`, `CMP-01 Button`, `CMP-06 Dialog`) | Bản `v0` có 2 thanh điều hướng Back chồng lên nhau (`← Alumni Detail` và `< Directory`) gây tốn 56dp chiều dọc; hành động Chặn (`US-10`) bị giấu trong icon `⋮` nhỏ. | ✅ **ACCEPT** | **Heuristic #4 (Consistency) & #8 (Minimalist Design) + `US-10`:** Xóa bỏ thanh `< Directory` thừa; chuẩn hóa `CMP-05 App Bar` chỉ có 1 thanh duy nhất cao `56dp`. Đưa nút **`🚫 Chặn người dùng này (Block This User)`** (`CMP-01 Variant=Destructive-Outlined`, cao `48dp`, viền `#DC2626`) xuống khu vực *Safety & Community Standards* cuối trang `S05` và bắt buộc mở `CMP-06 Confirmation Dialog` trước khi chặn. |
| **`D02`** | **`S03` Home Feed**<br>(`CMP-01 Button`, `CMP-03 Card`) | Nút nổi FAB `+ Ask Alumni` đè lên nút `Reply` ở góc dưới phải; nút hành động trong thẻ bài viết cũng tô màu cam đặc (`#F27024`), tranh chấp với CTA chính. | 🔧 **MODIFY** | **Heuristic #8 & Quy tắc Thứ bậc Thị giác:** Chấp nhận đổi nút hành động bên trong `CMP-03 Feed-Post-Card` sang kiểu `Secondary-Outlined (#1B365D)` với nhãn **`💬 Nhắn tin trao đổi`**. Thay vì chỉ dịch chuyển FAB như AI gợi ý, nhóm quyết định **bỏ hẳn FAB nổi** và tích hợp thanh tạo câu hỏi lên đầu danh sách bài viết để không bao giờ che khuất nội dung khi thao tác một tay. |
| **`D03`** | **`S07/S08` My Profile & Security**<br>(`CMP-03 Card`) | Google Stitch in nguyên các mã đặc tả kỹ thuật (`US-04`, `US-11`, `US-09`, `(US-08)`) lên tiêu đề/nút bấm và thiếu mục Đổi mật khẩu (`US-07`), Danh sách chặn (`US-10`). | ✅ **ACCEPT** | **Heuristic #2 (Match Between System & Real World):** Loại bỏ toàn bộ nhãn mã kỹ thuật nội bộ khỏi UI; sửa đúng thông tin sinh viên `Cohort K19 • SE193059`; bổ sung thẻ **Security & Account** chứa dòng điều hướng **Change Password (`US-07`)** và **Blocked Users (`US-10`)**. |
| **`D04`** | **`S04` Directory & `S07` Profile**<br>(`CMP-02 Filter Chip`, `Typography`, `Color Tokens`) | Filter Chips (`CAMPUS`, `MAJOR`) chỉ cao ~28dp và bị cắt chữ; tag kỹ năng chữ quá nhỏ (~10sp); chữ phụ màu `#9CA3AF` chỉ đạt tương phản 2.6:1 (< 4.5:1). | ✅ **ACCEPT** | **WCAG 2.2 AA (SC 1.4.3 & Target Size):** Chuẩn hóa chiều cao `CMP-02 Filter Chip` lên đúng **`48dp`**; nâng toàn bộ cỡ chữ nội dung và tag kỹ năng lên tối thiểu **`14sp Inter`**; đổi biến `color/text/secondary` sang **`#595959`** (đạt tương phản **7.0:1** trên nền `#FFFFFF`), giúp Persona cận thị đọc rõ dưới nắng. |
| **`D05`** | **`S04` Directory & `S08` Privacy**<br>(`CMP-02 Filter Chip & Switch`) | Trạng thái đang chọn của Filter Chip và trạng thái Bật/Tắt của công tắc Privacy chỉ phân biệt bằng màu sắc. | ✅ **ACCEPT** | **WCAG 2.2 SC 1.4.1 (Use of Color):** Bổ sung icon dấu tích **`✓`** bên trái nhãn của mọi Filter Chip đang chọn (`✓ 📍 Hola`, `✓ 💻 Software Engineering`) và gắn badge văn bản **`[ON]` / `[OFF]`** ngay cạnh mọi công tắc Switch (`US-09`). |
| **`D06`** | **`S07` My Profile & `S06` Chat**<br>(`CMP-09 Error State`, `CMP-02 Text Field`) | Hoàn toàn thiếu trạng thái báo lỗi nhập sai định dạng URL Google Calendar (`US-11`) và thiếu cơ chế phục hồi khi mất mạng lúc bấm lưu. | ✅ **ACCEPT** | **Heuristic #5 (Error Prevention) & #9 (Error Recovery):** Thiết kế đầy đủ `CMP-09 Network Error Banner` (nền `#FDF3F2`, viền `#DC2626`, giữ nguyên dữ liệu đã nhập trong form kèm nút **`↻ Retry Save`** cao `48dp`) và trạng thái `Inline Validation Error` dưới ô nhập URL Google Calendar. |
| **`D07`** | **`S05` Alumni Detail**<br>(Scope & Cultural Fit) | AI đề xuất làm hệ thống xếp lịch tự động, chấm điểm 5 sao công khai cho từng Alumni và thu phí dịch vụ. | ❌ **REJECT** | **Gắn với Persona `P02`, Văn hóa FPT & Ràng buộc Phạm vi Giảng viên:**<br>1. Giảng viên và phạm vi môn học không làm dịch vụ thương mại hay xếp lịch phức tạp.<br>2. Cựu sinh viên FPT chia sẻ kinh nghiệm hoàn toàn tự nguyện; việc chấm sao công khai hay xếp lịch ràng buộc tạo áp lực không phù hợp.<br>3. Nhóm giữ nguyên nút chính là **`💬 Nhắn tin trao đổi (Direct Message)`** và định vị `US-11` đúng bản chất là nút phụ **`📅 Xem Lịch rảnh tham khảo (Google Calendar)`** để sinh viên biết giờ rảnh mà nhắn tin liên hệ. |

---

## 2. Quyết định Kiến trúc Design System & Component (Do TV3 Nguyễn Phương Khải Chủ trì)

| Mã QĐ | Chủ đề Quyết định | Giải pháp Kỹ thuật trong Figma (`Page 04` & `Page 05`) | Lợi ích cho Người dùng & Lập trình viên Flutter |
| :---: | :--- | :--- | :--- |
| **`CD-01`** | **Tách biệt 2 tầng Token (`Primitive` → `Semantic`)** | Khai báo bảng màu gốc (`Navy #1B365D`, `Orange #F27024`, `Emerald #10B981`, `Crimson #DC2626`) rồi ánh xạ sang biến ngữ nghĩa (`color/brand/primary-navy`, `color/brand/cta-orange`, `color/text/secondary`). | Đảm bảo không có bất kỳ Frame nào trên **Page 03** gán cứng mã màu; ánh xạ 1-1 sang `ColorScheme` và `ThemeExtension` trong Flutter. |
| **`CD-02`** | **Chuẩn hóa Thanh Điều hướng Dưới 4 Tab (`CMP-04`)** | Thống nhất thanh `BottomNavigationBar` gồm đúng **4 Tab cốt lõi**: `Home` (`S03`), `Directory` (`S04`), `Chat` (`S06`), `Profile` (`S07`). Mỗi tab rộng `90dp × 64dp` (vượt chuẩn `48×48dp`). | Khớp 100% với sơ đồ Kiến trúc Thông tin (IA) của TV1 trong [`ux/user-flow.md`](../ux/user-flow.md) và bao phủ trọn vẹn 8 màn hình (`S01`–`S08`). |
| **`CD-03`** | **Đặt `Direct Message` làm Primary CTA và `Google Calendar` làm Nút Xem Lịch Tham khảo trên `S05`** | Trên `S05 Alumni Detail`, nút **`💬 Nhắn tin trao đổi (Direct Message)`** là Primary CTA (`#F27024`, cao `56dp`), trong khi **`📅 Xem Lịch rảnh tham khảo (Google Calendar)`** dùng kiểu `Secondary-Outlined (#1B365D)` cao `48dp` ngay bên dưới. | Tuân thủ đúng yêu cầu của giảng viên (chỉ chia sẻ link lịch cá nhân US-11), giúp sinh viên xem lịch rảnh tham khảo qua `url_launcher` rồi nhắn tin trao đổi trực tiếp tại `S06 Chat Room`. |
| **`CD-04`** | **Tích hợp Ngữ cảnh Mentor & Đính kèm CV vào Phòng Chat (`S06`)** | Trong màn hình `S06 Chat Room`, thêm thẻ **Context Banner (`Chủ đề trao đổi: Hướng dẫn CV OJT & Microservices`)** dưới App Bar, thẻ đính kèm file **`CV_TruongChiTrung_K19_SE.pdf`** và bộ gợi ý câu hỏi mở đầu (*Icebreaker Chips* cao `48dp`). | Giải quyết nỗi sợ "không biết bắt đầu nhắn gì cho anh/chị khóa trên" của sinh viên năm 3 (`P01`) và kết nối xuyên suốt giữa `S05` và `S06`. |
| **`CD-05`** | **Bảo vệ An toàn Cộng đồng 2 chiều (`US-10`) trên cả `S05`, `S06` và `S08`** | Tính năng **Chặn người dùng (`US-10`)** có mặt ở cả cuối hồ sơ công khai (`S05`) và trong menu hành động của phòng chat (`S06`), dùng chung một component `CMP-06 Block Confirmation Dialog` và đồng bộ trạng thái về danh sách `Blocked Users` ở `S08`. | Đảm bảo tính nhất quán dữ liệu và tuân thủ *Heuristic #3 (User Control and Freedom)*: người dùng có thể chặn khi bị làm phiền và bỏ chặn bất cứ lúc nào tại `S08`. |

---

## 3. Kiểm định Khả năng Tiếp cận & Đa Kích thước Màn hình (Accessibility & Responsive QA — Chuẩn `C4`)

Bảng kiểm tra dưới đây xác nhận toàn bộ 8 màn hình (`S01` – `S08`) và 9 nhóm Component đạt chuẩn **WCAG 2.2 Level AA** và **Chuẩn chung `C4`**:

| STT | Tiêu chí Kiểm định (WCAG 2.2 AA & Chuẩn `C4`) | Ngưỡng Bắt buộc | Kết quả Thực tế trên Thiết kế | Màn hình / Component Minh chứng |
| :-: | :--- | :---: | :---: | :--- |
| **01** | Độ tương phản chữ nội dung thường (Normal Text Contrast) | **≥ 4.5 : 1** | **7.00 : 1** (`#595959` trên `#FFFFFF`) & **17.74 : 1** (`#111827` trên `#FFFFFF`) | Toàn bộ 8 màn hình (`S01`–`S08`), đã khắc phục hoàn toàn lỗi chữ xám nhạt `#9CA3AF` từ vòng lặp `I02`. |
| **02** | Độ tương phản chữ lớn & Thành phần điều khiển (UI Controls) | **≥ 3.0 : 1** | **3.12 : 1 – 12.15 : 1** | Các nút `CMP-01 Button`, viền ô nhập lỗi `#DC2626` (`4.83:1`), chip đang chọn `#1B365D` (`12.15:1`). |
| **03** | Kích thước vùng chạm tối thiểu (Minimum Touch Target) | **≥ 48 × 48 dp** | **48 × 48 dp – 56 dp** | Nút Back AppBar (`48×48dp`), Filter Chips (`cao 48dp`), Nút gửi tin nhắn Chat (`48×48dp`), Công tắc Switch (`cao 48dp`), các nút CTA (`48–56dp`). |
| **04** | Cỡ chữ nội dung tối thiểu (Minimum Body Text Size) | **≥ 14 sp** | **14 sp – 16 sp (`Inter`)** | Bio, mô tả kinh nghiệm, tin nhắn chat, tag kỹ năng (`Golang`, `Microservices`, `OJT Prep`) đều đạt `14sp`. |
| **05** | Không chỉ dùng màu sắc để truyền đạt thông tin (Non-Color-Only Status) | **Màu + Icon + Text** | **Đạt 100%** | Chip đang chọn có dấu tích `✓`; Switch có badge `[ON]`/`[OFF]`; Lỗi URL có icon `❗` + viền đỏ + chữ mô tả; Verified Alumni có icon tích + nhãn chữ. |
| **06** | Đáp ứng kích thước `360 × 800 dp` (Compact Android/iOS) | Không tràn lề, không cắt chữ | **Đạt 100%** | Tất cả Frame chính trên **Page 03** dựng ở `360×800dp`, lề ngang `16dp`, vùng nội dung `328dp`. |
| **07** | Đáp ứng kích thước `412 × 915 dp` (Expanded Mobile) | Co giãn bằng Auto Layout | **Đạt 100%** | Kiểm tra kéo rộng Frame lên `412dp`: các Card (`Fill Container`) tự giãn ra `380dp`, các chip kỹ năng (`Wrap`) tự dồn dòng mượt mà. |
| **08** | Định hướng bố cục `≥ 600 dp` (Tablet / Foldable) | Chuyển đổi bố cục hợp lý | **Đạt 100%** | Đã đặc tả rõ cơ chế chuyển `BottomNavigationBar` thành `NavigationRail` và chia 2 cột `Master–Detail` (`320dp` + `Expanded`) cho `S04/S05` và `S06`. |
| **09** | Đầy đủ các trạng thái Giao diện (UI States Coverage — Chuẩn `C6`) | Default, Loading, Empty, Error, Success | **Đạt 100%** | Đặt các Frame biến thể trạng thái (`Skeleton Loading`, `Empty Search`, `Network/Validation Error`, `Block Confirmation Dialog`) ngay cạnh màn hình chính trên **Page 03**. |

---

## 4. Bảng Đánh Giá 20 Tiêu Chí UI Simplicity & Visual Density Checklist (100/100 Điểm)

> **Căn cứ:** [PRM393 UI Simplicity Checklist (`/ui-checklist.html`)](https://prm393-material.web.app/ui-checklist.html) — 20 tiêu chí chia làm 5 nhóm (5 điểm/tiêu chí). Bao gồm 3 điều kiện **CRITICAL** bắt buộc đạt.

| Mã | Nhóm & Nội dung Tiêu chí Đánh giá | Mức độ | Điểm | Minh chứng Thực tế trên 8 Màn hình (`S01` – `S08`) |
| :-: | :--- | :---: | :---: | :--- |
| **A1** | **Text & Content:** Tiêu đề và nhãn ngắn, trực tiếp, dễ quét. | Chuẩn | **5 / 5** | AppBar và tiêu đề Card ngắn gọn (`Hồ sơ Cựu sinh viên`, `Alumni Directory`, `Privacy & Security`). |
| **A2** | **Text & Content:** Không có đoạn giới thiệu hoặc giải thích dài không cần thiết. | Chuẩn | **5 / 5** | Bio và mô tả giới hạn tối đa 2–3 dòng (`14sp`, line-height `20sp`), dễ đọc nhanh khi di chuyển. |
| **A3** | **Text & Content:** Thông tin ít sử dụng được chuyển vào chi tiết hoặc mở rộng. | Chuẩn | **5 / 5** | Trên `S04 Directory`, thẻ chỉ hiện 3–4 kỹ năng chính; lịch sử công việc chi tiết được đưa vào trang con `S05`. |
| **A4** | **Text & Content:** Không lặp lại cùng một nội dung ở nhiều card. | Chuẩn | **5 / 5** | Đã xóa thanh `< Directory` trùng lặp trên `S05` (`D01`) và xóa các mã `US-xx` thừa trên `S07/S08` (`D03`). |
| **B1** | **Color Discipline:** Có một màu nhấn chủ đạo và bảng màu trung tính nhất quán. | Chuẩn | **5 / 5** | Màu nhấn CTA duy nhất `#F27024` (FPT Orange), màu thương hiệu `#1B365D` (Navy), nền trung tính `#F8FAFC` / `#FFFFFF`. |
| **B2** | **Color Discipline:** Không dùng màu khác nhau chỉ để trang trí card. | Chuẩn | **5 / 5** | Toàn bộ Card thông thường dùng nền `#FFFFFF` và viền `#E2E8F0` thống nhất. |
| **B3** | **Color Discipline:** Màu success, warning, error có ý nghĩa nhất quán. | Chuẩn | **5 / 5** | `#10B981` chỉ dùng cho Verified/Online/ON; `#DC2626` chỉ dùng cho Error/Block/Logout. |
| **B4** | **Color Discipline:** Màu không phải phương tiện duy nhất truyền đạt trạng thái (kèm icon/nhãn chữ). | 🔴 **CRITICAL** | **5 / 5** | Chip đang chọn có dấu **`✓`**; Switch có nhãn **`[ON]` / `[OFF]`**; lỗi nhập liệu có icon **`❗`** + thông báo chữ (`D05`). |
| **C1** | **Density & Whitespace:** Mật độ thông tin phù hợp với tác vụ và thiết bị. | Chuẩn | **5 / 5** | Mỗi màn hình hiển thị tối đa 2–3 khối thẻ chính, khoảng trắng chiếm > 25% diện tích viewport `360×800dp`. |
| **C2** | **Density & Whitespace:** Khoảng cách các thành phần tuân theo spacing scale. | Chuẩn | **5 / 5** | Tuân thủ nghiêm ngặt lưới `8dp` (`4dp`, `8dp`, `12dp`, `16dp`, `24dp`, `32dp`). |
| **C3** | **Density & Whitespace:** Các mục liên quan được nhóm, không chia thành quá nhiều card. | Chuẩn | **5 / 5** | Trên `S08`, 5 công tắc quyền riêng tư được gom gọn vào 1 Card `Field-Level Privacy Controls`; mục bảo mật gom vào 1 Card `Security & Moderation`. |
| **C4** | **Density & Whitespace:** Bảng, form và nút dễ đọc, dễ thao tác: vùng chạm ≥ 48×48 dp, chữ nội dung ≥ 14 sp. | 🔴 **CRITICAL** | **5 / 5** | Mọi Button, Chip, Input, Switch đều cao `48dp`–`56dp`; toàn bộ chữ nội dung đạt `14sp`–`16sp` (`D04`). |
| **D1** | **Visual Hierarchy:** Nội dung quan trọng được nhận ra ngay. | Chuẩn | **5 / 5** | Tên, trạng thái `✓ Verified Alumni` và vị trí công việc của Mentor nổi bật ngay đầu thẻ ở `S04` và `S05`. |
| **D2** | **Visual Hierarchy:** Có hành động chính nổi bật, hành động phụ không cạnh tranh. | Chuẩn | **5 / 5** | Trên `S05`, nút `💬 Nhắn tin trao đổi` tô cam `#F27024` (`56dp`), còn nút `📅 Xem Lịch rảnh tham khảo` dùng viền `#1B365D` (`48dp`). |
| **D3** | **Visual Hierarchy:** Các cấp tiêu đề, nội dung, nhãn phụ phân biệt rõ ràng. | Chuẩn | **5 / 5** | Phân cấp rõ ràng giữa `Hanken Grotesk Bold 18–24sp` (Heading) và `Inter Regular/SemiBold 14–16sp` (Body). |
| **D4** | **Visual Hierarchy:** Các phần trang có thứ tự, không tranh giành sự chú ý. | Chuẩn | **5 / 5** | Đã loại bỏ nút nổi FAB đè lên bài viết trên `S03` (`D02`); đưa nút `Block This User` xuống cuối trang `S05` (`D01`). |
| **E1** | **Consistency & Accessibility:** Component, font, spacing và icon dùng nhất quán. | Chuẩn | **5 / 5** | Toàn bộ 8 màn hình dùng chung 9 nhóm Component (`CMP-01` → `CMP-09`) và bộ biến màu trên Figma. |
| **E2** | **Consistency & Accessibility:** Tương phản chữ thường ≥ 4.5:1; chữ lớn và thành phần điều khiển ≥ 3:1 (WCAG 2.2 AA). | 🔴 **CRITICAL** | **5 / 5** | Chữ chính `#111827` đạt **17.74:1**; chữ phụ `#595959` đạt **7.00:1**; thành phần điều khiển đạt **3.12:1 – 12.15:1**. |
| **E3** | **Consistency & Accessibility:** Thành phần điều khiển có chỉ báo trạng thái dễ nhận biết. | Chuẩn | **5 / 5** | Ô OTP đang nhập có viền cam `2dp #F27024` (`S02`); tab đang chọn có gạch cam và chữ đậm (`CMP-04`). |
| **E4** | **Consistency & Accessibility:** Các trạng thái loading, empty, success, error rõ ràng. | Chuẩn | **5 / 5** | Có đủ Skeleton Loading, Empty Search, Inline Validation Error và Network Error Banner kèm nút `↻ Retry Save` (`S07`). |
| **TỔNG** | **Kết quả Tự đánh giá UI Simplicity Checklist** | **Đạt 3/3 Critical** | **100 / 100** | ✅ **ĐẠT XUẤT SẮC (Pass Quality Gate G4)** |
