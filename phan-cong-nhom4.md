# 📋 Phân công Công việc Nhóm 4 — PRM323 Lab 2 (Google Stitch → Figma)

> **Dự án:** **FPT Alumni Connect** — Nền tảng kết nối Cựu sinh viên & Sinh viên Đại học FPT  
> **Căn cứ:** Đề Lab 2 (12 trang) + Bộ chuẩn [PRM393 UI/UX Material](https://prm393-material.web.app/) (Persona Guideline 12 trường, Quality Gates P0–P5, UI Checklist 20 tiêu chí) + 11 User Stories (`US-01` → `US-11`).

---

## 1. Bảng Tóm tắt Phân công (4 Thành viên — Cân bằng 25%/người)

Mỗi thành viên phụ trách **1 mảng chuyên môn xuyên suốt (Lead)** theo đúng Rubric **VÀ** sở hữu **2 màn hình cố định** (từ Wireframe → Final UI → Screen Spec → Flutter Handoff). Nhờ đó **cả 4 thành viên đều có lịch sử đóng góp trên Git và trên Figma**.

| Thành viên | Vai trò Chuyên môn (Lead) | 2 Màn hình Sở hữu | Trang Figma Tham gia | File Repository Phụ trách |
| :--- | :--- | :---: | :---: | :--- |
| **TV1: Võ Quang Tâm**<br>*(MSSV: SE192621)* | **UX Research, Persona & User Flow Lead**<br>- Viết bộ Persona chuẩn 12 trường lõi (1 Primary + 2 Secondary) & Coverage Matrix<br>- Vẽ IA 8 màn hình, 3 User Flows & bảng ánh xạ<br>- Quản lý `README.md` | **`S01`** (Login & Google Auth - `US-01, US-02, US-08`)<br>**`S02`** (Verify Email & Reset Pass - `US-03, US-07`) | **Page 01** (Chủ trì)<br>**Page 02, 03** (`S01, S02`) | - `ux/persona.md`<br>- `ux/user-flow.md`<br>- `README.md`<br>- Phần `S01, S02` trong `screen-spec.md` & `flutter-handoff.md` |
| **TV2: Trương Chí Trung**<br>*(MSSV: SE193059 — Trưởng nhóm)* | **AI Generation (Google Stitch), Iterations & UX Critique Lead**<br>- Viết Prompt Stitch nguyên văn, chụp 4 màn hình `v0`<br>- Thực hiện 3 vòng lặp có ý nghĩa (Before/After)<br>- Chạy AI Critique (7 phát hiện theo 10 Heuristics + WCAG) & bảng quyết định Accept/Modify/Reject | **`S03`** (Home Feed Chung & Nhóm ngành)<br>**`S04`** (Member Search & Directory - `US-06`) | **Page 02, 03** (`S03, S04` + áp dụng cải tiến từ AI log) | - `ai/ai-design-log.md` *(Đã hoàn thành 100%)*<br>- `assets/stitch/*` *(Đã hoàn thành 100%)*<br>- Mục Quyết định trong `design/design-decisions.md`<br>- Phần `S03, S04` trong `screen-spec.md` & `flutter-handoff.md` |
| **TV3: Nguyễn Phương Khải**<br>*(MSSV: SE183590)* | **Design System & Reusable Components Lead**<br>- Soạn `design/DESIGN.md` (Tokens & Component Rules)<br>- Tạo Figma Variables/Styles (Color, Type, Spacing, Radius, Elevation)<br>- Dựng 9 Component bắt buộc bằng Auto Layout + Variants<br>- Lập 2 bảng Mapping chung trong Handoff | **`S05`** (Member Public Profile + Calendar/Block - `US-05, US-10, US-11`)<br>**`S06`** (Direct & Group Chat Room) | **Page 04, 05** (Chủ trì)<br>**Page 02, 03** (`S05, S06`) | - `design/DESIGN.md`<br>- 2 bảng Mapping trong `handoff/flutter-handoff.md`<br>- Phần `S05, S06` trong `screen-spec.md` & `flutter-handoff.md` |
| **TV4: Đỗ Trọng Tín**<br>*(MSSV: SE190218)* | **Interactive Prototype, Responsive QA & Handoff Lead**<br>- Kiểm tra Accessibility (Contrast, 48×48dp, ≥14sp) & Responsive ở 360dp, 412dp, mô tả ≥600dp<br>- Nối dây Prototype 3 flows (Starting points, Overlay Dialog, Loading→Result, Back)<br>- Xuất ảnh `assets/figma/` | **`S07`** (My Profile, Edit & Google Calendar Link - `US-04, US-11`)<br>**`S08`** (Privacy, Security & Blocked List - `US-07, US-08, US-09, US-10`) | **Page 06** (Chủ trì)<br>**Page 02, 03** (`S07, S08`) | - Checklist A11y trong `design/design-decisions.md`<br>- Tổng hợp `design/screen-spec.md` & `handoff/flutter-handoff.md`<br>- `assets/figma/*`<br>- Phần `S07, S08` trong `screen-spec.md` & `flutter-handoff.md` |

---

## 2. Thứ tự Phối hợp trên Git & Figma (Tránh bị trừ điểm Quy trình)

1. **Làm việc độc lập/song song:** Cả 4 thành viên đã có sẵn khung 8 màn hình (`S01`–`S08`), 3 Flows (`F01`–`F03`) và bộ Design Tokens chuẩn trong kho tài liệu này nên ai cũng có thể làm ngay phần của mình.
2. **Thứ tự trên Figma:**
   - **TV3** tạo xong **Figma Variables (Page 04)** và **9 Components (Page 05)** để các thành viên kéo **Instances** vào **Page 03 (Final UI)** (tuyệt đối không detach instance, không gán cứng mã màu).
   - **Cả 4 TV** hoàn thiện 2 màn hình của mình trên **Page 02 (Wireframe)** và **Page 03 (Final UI 360×800dp + các biến thể trạng thái đặt cạnh)**.
   - **TV4** nối dây **Page 06 (Prototype)** cho cả 3 Flows sau khi Page 03 đã có đủ 8 màn hình.
