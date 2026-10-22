# 👤 Bộ Chân dung Người dùng (User Personas) & Ma trận Bao phủ Tính năng

> **Dự án:** **FPT Alumni Connect** — Nền tảng kết nối Cựu sinh viên & Sinh viên Đại học FPT  
> **Môn học:** PRM323 — Cross-Platform Application Development (Lab 2: Google Stitch → Figma)  
> **Người thực hiện chính (Lead Author):** **Võ Quang Tâm** — MSSV: **SE192621** (Vai trò: **TV1 — UX Research, Persona & User Flow Lead**)  
> **Căn cứ Quy chuẩn:** Bộ tiêu chuẩn [PRM393 UI/UX Material](https://prm393-material.web.app/), Persona Guideline 12 trường cốt lõi, Quality Gates P0–P5, 11 User Stories trọng tâm (`US-01` → `US-11`) trên tổng thể 50 User Stories của nền tảng.  
> **Chuỗi truy vết (Traceability Chain):** `E01..E04 (Evidence) → P01..P03 (Personas) → US-01..US-11 (Core Stories) → F01..F03 (User Flows) → S01..S08 (Screens) → I01..I03 (AI Iterations) → D01..D07 (Decisions)`

---

## 1. Cơ sở Bằng chứng Nghiên cứu Người dùng (Research Evidence Base)

Bộ Persona được xây dựng từ 4 nguồn bằng chứng nghiên cứu định tính và định lượng thực tế tại Đại học FPT:

| Mã Bằng chứng | Phương pháp Nghiên cứu | Quy mô / Đối tượng Khảo sát | Phát hiện Trọng tâm (Key Insights) | Ánh xạ Persona & Giải pháp Thiết kế |
| :---: | :--- | :--- | :--- | :--- |
| **`E01`** | Khảo sát Trực tuyến (Quantitative Survey) | 120 sinh viên K18, K19 (FPTU HCMC & Hà Nội) | - **84.2%** sinh viên chuẩn bị đi OJT gặp khó khăn khi tìm kiếm Mentor cựu sinh viên phù hợp với chuyên ngành.<br>- **78.3%** mong muốn xem được lịch rảnh của Mentor qua link Google Calendar để tiện chủ động liên hệ. | Hình thành **`P01` (Trương Chí Trung)**. Đưa tính năng nhúng link **Google Calendar (`US-11`)** và tìm kiếm lọc theo Campus/Major (`US-06`) vào luồng chính. |
| **`E02`** | Phỏng vấn Sâu (Qualitative In-depth Interview) | 8 Cựu sinh viên K12–K15 đang đi làm tại các doanh nghiệp công nghệ | - **100%** cựu SV sẵn lòng chia sẻ kinh nghiệm nhưng rất sợ bị lộ số điện thoại cá nhân và bị quấy rối ngoài giờ làm việc.<br>- Yêu cầu bắt buộc phải có cơ chế **Ẩn thông tin riêng tư (`US-09`)** và **Chặn tài khoản quấy rối (`US-10`)**. | Hình thành **`P02` (Đặng Minh Tuấn)**. Bổ sung cụm công tắc Privacy [ON]/[OFF], nút Block người dùng rõ ràng kèm Confirmation Dialog. |
| **`E03`** | Quan sát Ngữ cảnh Thực tế (Contextual Inquiry) | 15 sinh viên sử dụng smartphone tại khuôn viên FPTU HCMC | - Sinh viên chủ yếu dùng điện thoại **bằng một tay (ngón cái)** trong lúc di chuyển giữa các ca học hoặc đi xe bus.<br>- Ánh sáng mặt trời tại sảnh trường rất mạnh gây lóa màn hình; nhiều sinh viên bị cận thị. | Xác lập ràng buộc công thái học: Viewport chuẩn **360×800 dp**, vùng chạm tối thiểu **48×48 dp**, độ tương phản chữ phụ **≥ 4.5:1** (đạt 5.4:1 với màu `#595959`). |
| **`E04`** | Khảo sát Sinh viên Mới (Freshmen Onboarding Survey) | 45 tân sinh viên K20 các cơ sở | - Tân sinh viên bỡ ngỡ với quy trình xác thực email trường `@fpt.edu.vn`, dễ quên mật khẩu, có nhu cầu tìm đọc kinh nghiệm định hướng trên Forum theo đúng Campus của mình. | Hình thành **`P03` (Lê Hoàng Lan)**. Tối ưu luồng xác minh email bằng OTP (`US-03`), luồng đặt lại mật khẩu (`US-07`) và lọc bài viết/danh bạ theo Campus (`US-06, US-33`). |

---

## 2. Bộ 3 Personas Chuẩn 12 Trường Dữ liệu (Core Personas Specification)

---

### 🌟 2.1. Primary Persona (`P01`): Trương Chí Trung — Sinh viên Năm 3 Tìm kiếm Mentor OJT

> Chân dung người dùng mục tiêu chính, đồng bộ 100% với Prompt nguyên văn của Google Stitch và AI Design Log của TV2 (`ai/ai-design-log.md`).

```
┌────────────────────────────────────────────────────────────────────────┐
│  P01: TRƯƠNG CHÍ TRUNG (Primary Persona)                               │
│  "Cần kết nối nhanh với Alumni ngành SE để review CV & xin lịch hẹn     │
│   tư vấn OJT 1-1 mà không tốn thời gian nhắn tin chờ đợi."             │
├────────────────────────────────────────┬───────────────────────────────┤
│  • Tuổi: 20  |  Giới tính: Nam         │  • Vai trò: Sinh viên Năm 3   │
│  • Khóa: K19 |  MSSV: SE193059         │  • Campus: FPT TP. Hồ Chí Minh│
│  • Chuyên ngành: Kỹ thuật Phần mềm (SE)│  • Mục tiêu OJT: Kỳ Fall 2026 │
└────────────────────────────────────────┴───────────────────────────────┘
```

| STT | Trường Dữ liệu Chuẩn PRM393 | Nội dung Đặc tả Chi tiết |
| :-: | :--- | :--- |
| **01** | **Nhận diện & Nhân khẩu học (Demographics & Photo)** | - **Họ và tên:** Trương Chí Trung<br>- **Tuổi:** 20 (Sinh năm 2006).<br>- **Khóa học / Mã số:** K19 — MSSV: `SE193059`.<br>- **Chuyên ngành:** Kỹ thuật Phần mềm (Software Engineering - SE).<br>- **Cơ sở học tập:** Đại học FPT TP. Hồ Chí Minh (Campus Q9).<br>- **Hình ảnh đại diện:** Nam sinh viên mặc áo cam FPT, đeo kính cận, phong thái năng động, tập trung. |
| **02** | **Trích dẫn Tiêu biểu (Archetype Quote)** | *"Kỳ tới mình bắt đầu đi thực tập OJT rồi. Mình cần tìm một anh cựu sinh viên FPT đang làm Senior Backend để xin review CV và xem link lịch Google Calendar của anh ấy để biết lúc nào anh ấy rảnh mà chủ động nhắn tin liên hệ."* |
| **03** | **Bối cảnh & Lý lịch (Academic Background & Bio)** | Trung là sinh viên năm thứ 3 chuyên ngành SE, học lực Khá-Giỏi (GPA 3.2/4.0), đã hoàn thành các môn cơ sở ngành như Cấu trúc dữ liệu, OOP và Java Web. Cậu đang chuẩn bị hồ sơ ứng tuyển thực tập doanh nghiệp (OJT) nhưng còn băn khoăn về yêu cầu thực tế của các công ty công nghệ lớn, chưa biết cách viết CV nổi bật và cần người đi trước định hướng lộ trình chuyên sâu (Backend vs Cloud). |
| **04** | **Mục tiêu & Nhiệm vụ Cốt lõi (User Goals & Core Tasks)** | 1. Đăng nhập nhanh vào ứng dụng bằng tài khoản Google trường `@fpt.edu.vn` (`US-02`).<br>2. Tìm kiếm danh bạ cựu sinh viên lọc chính xác theo **Campus HCMC** và ngành **Software Engineering** (`US-06`).<br>3. Xem hồ sơ công khai của cựu sinh viên (kỹ năng, kinh nghiệm, công ty đang làm) (`US-05`).<br>4. Xem **đường dẫn Google Calendar** trên Profile của mentor để biết khung giờ rảnh/bận và nhắn tin trao đổi (`US-11, US-12`).<br>5. Đăng câu hỏi thắc mắc lên Bảng tin nhóm ngành để cộng đồng cựu SV hỗ trợ (`US-22`). |
| **05** | **Khó khăn & Điểm nghẽn Trải nghiệm (Pain Points & Frustrations)** | - **Khó tiếp cận cựu SV uy tín:** Hỏi trên các nhóm Facebook trường thì bài viết dễ bị trôi hoặc toàn các bài đăng tuyển sinh, nhà trọ spam.<br>- **Không biết thời gian rảnh của tiền bối:** Nhắn tin hỏi cựu SV vào giờ hành chính dễ gây phiền hà nếu không nắm được lịch sinh hoạt/làm việc của họ.<br>- **Mất tập trung bởi giao diện rối rắm:** Ứng dụng nhiều nút bấm cạnh tranh thị giác hoặc nút nổi đè lên nội dung bài viết làm khó đọc tin. |
| **06** | **Động lực Thúc đẩy (Core Motivations)** | - Mong muốn vượt qua kỳ phỏng vấn OJT vào các doanh nghiệp hàng đầu (FPT Software, VNG, KMS, Grab).<br>- Khát khao mở rộng mạng lưới quan hệ (Networking) với các đàn anh khóa trước trong ngành. |
| **07** | **Bối cảnh & Hoàn cảnh Sử dụng (Usage Context & Environment)** | - **Thao tác một tay (One-handed thumb zone):** Thường mở app bằng ngón tay cái khi đang đứng chờ xe bus tuyến 53 hoặc đi bộ vội vã giữa các tòa nhà Alpha và Beta.<br>- **Môi trường ánh sáng mạnh:** Sử dụng điện thoại ngoài sảnh trường dưới ánh nắng gắt miền Nam, dễ bị lóa mắt nếu độ tương phản màu sắc không đạt chuẩn. |
| **08** | **Thiết bị & Năng lực Công nghệ (Device & Tech Literacy)** | - **Thiết bị:** Smartphone Android tầm trung (Samsung Galaxy A-series), độ phân giải viewport chuẩn **360×800 dp**, kết nối 4G Viettel hoặc Wifi trường `FPTU_Student`.<br>- **Trình độ công nghệ:** Rất cao (Thành thạo công nghệ, yêu thích giao diện tối giản, phản hồi tức thì dưới 1 giây). |
| **09** | **Thói quen Mạng xã hội & Giao tiếp (Networking Behavior)** | - Thích tương tác lịch sự, tôn trọng thời gian cá nhân của người khác.<br>- Ưu tiên chuẩn bị sẵn danh sách câu hỏi cụ thể trước khi nhắn tin trao đổi 1-1, không thích tán gẫu vô bổ. |
| **10** | **Khả năng Tiếp cận & Công thái học (Accessibility Constraints)** | - **Thị lực:** Cận thị 3 độ, mắt dễ mỏi khi đọc chữ nhỏ dưới 14sp hoặc chữ màu xám nhạt tương phản thấp.<br>- **Yêu cầu giao diện:** Chữ nội dung tối thiểu **14sp**, độ tương phản chữ phụ với nền đạt **≥ 4.5:1** (chuẩn WCAG AA), mọi nút bấm/filter chip phải có chiều cao tối thiểu **48 dp** để ngón cái chạm chính xác, không bị bấm nhầm. |
| **11** | **Tiêu chí Đánh giá Thành công (Success Metric / Value Metric)** | **Tìm thấy cựu sinh viên phù hợp và mở xem link lịch Google Calendar của họ chỉ sau dưới 2 phút thao tác** để chủ động nhắn tin kết nối đúng lúc. |
| **12** | **Ánh xạ User Stories Trọng tâm (Mapped User Stories)** | `US-01, US-02, US-04, US-05, US-06, US-08, US-11, US-12, US-22, US-26` |

---

### 🛡️ 2.2. Secondary Persona 1 (`P02`): Đặng Minh Tuấn — Cựu Sinh viên Senior Mentor

> Đại diện cho nhóm Cựu sinh viên thành đạt, có thiện chí chia sẻ nhưng vô cùng bận rộn và coi trọng quyền riêng tư. Khớp với nhân vật thực tế trong AI Design Log (`D01, D07, iter1`).

```
┌────────────────────────────────────────────────────────────────────────┐
│  P02: ĐẶNG MINH TUẤN (Secondary Persona — Alumni Mentor)               │
│  "Sẵn sàng dành 1-2 tiếng cuối tuần tư vấn cho đàn em FPT, nhưng cần   │
│   tự động hóa lịch hẹn và tuyệt đối bảo vệ số điện thoại cá nhân."     │
├────────────────────────────────────────┬───────────────────────────────┤
│  • Tuổi: 26  |  Giới tính: Nam         │  • Vai trò: Senior Software Eng│
│  • Khóa: K15 |  Năm tốt nghiệp: 2022   │  • Doanh nghiệp: Grab / Remote│
│  • Chuyên ngành: Kỹ thuật Phần mềm (SE)│  • Cơ sở gốc: FPT TP. Hồ Chí Minh│
└────────────────────────────────────────┴───────────────────────────────┘
```

| STT | Trường Dữ liệu Chuẩn PRM393 | Nội dung Đặc tả Chi tiết |
| :-: | :--- | :--- |
| **01** | **Nhận diện & Nhân khẩu học (Demographics & Photo)** | - **Họ và tên:** Đặng Minh Tuấn<br>- **Tuổi:** 26.<br>- **Cựu sinh viên khóa:** K15 Kỹ thuật Phần mềm (Tốt nghiệp loại Giỏi năm 2022).<br>- **Vị trí hiện tại:** Senior Backend Engineer tại Tech Hub (làm việc Hybrid/Remote cho công ty công nghệ đa quốc gia).<br>- **Nơi sinh sống:** TP. Hồ Chí Minh.<br>- **Hình ảnh đại diện:** Chân dung chuyên nghiệp phong cách Tech Lead, sơ mi chỉn chu, nụ cười thân thiện, có tích xanh Verified Alumni FPT. |
| **02** | **Trích dẫn Tiêu biểu (Archetype Quote)** | *"Mình từng được các anh cựu sinh viên giúp đỡ rất nhiều thời sinh viên nên luôn muốn trả ơn cộng đồng FPT. Tuy nhiên, công việc của mình rất bận rộn, mình chỉ có thể add link Google Calendar cá nhân lên Profile để các em thấy lịch rảnh mà nhắn tin liên hệ đúng lúc, và tuyệt đối không muốn bị lộ số điện thoại cá nhân ra ngoài."* |
| **03** | **Bối cảnh & Lý lịch (Career Background & Bio)** | Sau 4 năm tốt nghiệp, Tuấn đã tích lũy dày dạn kinh nghiệm về hệ thống thanh toán phân tán (Distributed Systems, Go, Kubernetes, Kafka). Anh thường xuyên tham gia đóng góp chuyên môn cho cộng đồng và muốn tìm kiếm các bạn sinh viên FPT xuất sắc để giới thiệu (referral) vào công ty. Tuy nhiên, thời gian làm việc trong tuần rất dày đặc với các cuộc họp sprint xuyên quốc gia. |
| **04** | **Mục tiêu & Nhiệm vụ Cốt lõi (User Goals & Core Tasks)** | 1. Thiết lập hồ sơ công khai chuyên nghiệp, hiển thị kỹ năng và khóa K15 (`US-04, US-05`).<br>2. Add link Google Calendar cá nhân vào Profile để người khác có thể xem lịch và liên hệ khi cần (`US-11`).<br>3. Bật/Tắt công tắc quyền riêng tư (`Privacy Toggles`) để ẩn số điện thoại, ẩn email cá nhân, chỉ giữ phương thức liên hệ qua app (`US-09`).<br>4. Sử dụng tính năng **Chặn người dùng (`US-10`)** ngay lập tức nếu gặp sinh viên có hành vi thiếu lịch sự hoặc spam tin nhắn quấy rối.<br>5. Đăng bài chia sẻ kinh nghiệm kỹ thuật hoặc cơ hội thực tập lên nhóm ngành SE (`US-23`). |
| **05** | **Khó khăn & Điểm nghẽn Trải nghiệm (Pain Points & Frustrations)** | - **Nỗi lo quấy rối & mất an toàn thông tin:** Từng bị sinh viên lạ xin số điện thoại rồi nhắn tin Zalo nửa đêm đòi giải bài tập hộ.<br>- **Tính năng Chặn bị ẩn:** Cảm thấy bất an nếu ứng dụng giấu tính năng Chặn tài khoản vào menu phụ ba chấm quá nhỏ mà không có hộp thoại xác nhận rõ ràng.<br>- **Khó chịu với hệ thống chấm sao công khai:** Phản đối việc sinh viên chấm điểm sao (1–5 sao) cựu sinh viên vì việc hỗ trợ hoàn toàn xuất phát từ sự tự nguyện đóng góp phi lợi nhuận. |
| **06** | **Động lực Thúc đẩy (Core Motivations)** | - Đóng góp cho sự phát triển của phong trào sinh viên FPT (Alumni Giving Back).<br>- Xây dựng thương hiệu cá nhân (Personal Branding) trong cộng đồng kỹ sư phần mềm FPT toàn quốc. |
| **07** | **Bối cảnh & Hoàn cảnh Sử dụng (Usage Context & Environment)** | - Mở app vào giờ nghỉ trưa tại văn phòng hoặc cuối tuần tại nhà riêng trên ghế sofa.<br>- Thao tác nhanh gọn, ưu tiên độ tin cậy và sự riêng tư tuyệt đối. |
| **08** | **Thiết bị & Năng lực Công nghệ (Device & Tech Literacy)** | - **Thiết bị:** iPhone 15 Pro / Google Pixel 8, kết nối mạng cáp quang tốc độ cao.<br>- **Trình độ công nghệ:** Chuyên gia kỹ thuật (Hiểu sâu về cấu trúc hệ thống, API, bảo mật thông tin OAuth 2.0). |
| **09** | **Thói quen Mạng xã hội & Giao tiếp (Networking Behavior)** | - Tác phong làm việc chuyên nghiệp, đúng giờ, chỉ tiếp nhận những yêu cầu tư vấn có chuẩn bị trước nội dung.<br>- Không bao giờ chia sẻ thông tin đời tư cho người mới quen trên mạng. |
| **10** | **Khả năng Tiếp cận & Kiểm soát (Accessibility & Control Needs)** | - **Kiểm soát trạng thái tường minh:** Các công tắc Switch trong cài đặt Privacy bắt buộc phải có nhãn chữ `[ON]` / `[OFF]` rõ ràng, không chấp nhận giao diện chỉ dùng màu sắc để phân biệt trạng thái (tuân thủ WCAG 2.1 SC 1.4.1).<br>- Nút **Block This User** phải nổi bật ở cuối trang cá nhân với màu đỏ cảnh báo (`#DC2626`) đạt chuẩn vùng chạm 48dp. |
| **11** | **Tiêu chí Đánh giá Thành công (Success Metric / Value Metric)** | **Bảo vệ an toàn 100% dữ liệu riêng tư** (không lộ SĐT/Email), cập nhật link Google Calendar lên Profile chỉ sau 30 giây để đàn em tiện theo dõi lịch rảnh. |
| **12** | **Ánh xạ User Stories Trọng tâm (Mapped User Stories)** | `US-04, US-05, US-07, US-08, US-09, US-10, US-11, US-12, US-23` |

---

### 🌱 2.3. Secondary Persona 2 (`P03`): Lê Hoàng Lan — Tân Sinh viên K20 Cần Định hướng

> Đại diện cho nhóm tân sinh viên mới nhập học, bỡ ngỡ với môi trường đại học, kết nối mạng chập chờn và cần các luồng xác thực / trợ giúp lỗi rõ ràng bằng tiếng Việt.

```
┌────────────────────────────────────────────────────────────────────────┐
│  P03: LÊ HOÀNG LAN (Secondary Persona — Freshmen Mentee)               │
│  "Vừa vào trường mình rất mông lung, mong muốn xác minh tài khoản       │
│   nhanh chóng và tìm được anh chị cựu sinh viên Campus Đà Nẵng."       │
├────────────────────────────────────────┬───────────────────────────────┤
│  • Tuổi: 18  |  Giới tính: Nữ          │  • Vai trò: Tân Sinh viên K20 │
│  • Khóa: K20 |  MSSV: DE200114         │  • Campus: FPT Đà Nẵng        │
│  • Chuyên ngành: Trí tuệ Nhân tạo (AI) │  • Trạng thái: Học kỳ Dự bị TA│
└────────────────────────────────────────┴───────────────────────────────┘
```

| STT | Trường Dữ liệu Chuẩn PRM393 | Nội dung Đặc tả Chi tiết |
| :-: | :--- | :--- |
| **01** | **Nhận diện & Nhân khẩu học (Demographics & Photo)** | - **Họ và tên:** Lê Hoàng Lan<br>- **Tuổi:** 18 (Sinh năm 2008).<br>- **Khóa học:** K20 — MSSV: `DE200114`.<br>- **Chuyên ngành:** Trí tuệ Nhân tạo & Khoa học Dữ liệu (AI).<br>- **Cơ sở học tập:** Đại học FPT Đà Nẵng (Khu Đô thị FPT City).<br>- **Hình ảnh đại diện:** Nữ tân sinh viên đeo ba lô, nụ cười rạng rỡ, mang nét rụt rè nhưng ham học hỏi. |
| **02** | **Trích dẫn Tiêu biểu (Archetype Quote)** | *"Em mới chuyển từ Quảng Nam ra Đà Nẵng học, chưa quen ai ở trường cả. Em muốn tìm các anh chị cựu sinh viên ngành AI cơ sở Đà Nẵng để hỏi xem kỳ tới cần chuẩn bị máy tính cấu hình thế nào và cách học tiếng Anh hiệu quả."* |
| **03** | **Bối cảnh & Lý lịch (Academic Background & Bio)** | Lan vừa trúng tuyển học bổng Đại học FPT Đà Nẵng, hiện đang học chương trình tiếng Anh dự bị (Level 4). Lan chưa từng sử dụng các nền tảng networking học đường, mới được cấp email trường đuôi `@fpt.edu.vn` và chưa quen với việc xác thực bảo mật nhiều bước. Lan dễ bị bối rối khi gặp các thông báo lỗi kỹ thuật bằng tiếng Anh. |
| **04** | **Mục tiêu & Nhiệm vụ Cốt lõi (User Goals & Core Tasks)** | 1. Đăng ký tài khoản và nhận mã xác minh OTP 6 số qua email trường một cách mượt mà (`US-01, US-03`).<br>2. Dễ dàng lấy lại mật khẩu hoặc đổi mật khẩu mới khi quên thông tin đăng nhập (`US-07`).<br>3. Lọc danh bạ thành viên theo đúng **Campus Đà Nẵng** (`US-06`).<br>4. Lướt đọc các câu hỏi chia sẻ kinh nghiệm học tập trên Home Feed theo campus (`US-26, US-33`).<br>5. Hỏi đáp với trợ lý ảo AI của trường về các môn học sắp tới (`US-38`). |
| **05** | **Khó khăn & Điểm nghẽn Trải nghiệm (Pain Points & Frustrations)** | - **Mã OTP đến chậm hoặc giao diện báo lỗi khó hiểu:** Rất lo lắng khi bấm đăng ký mà không biết mã gửi về đâu hoặc nhập sai mật khẩu mà không có hướng dẫn khôi phục.<br>- **Mạng Wifi ký túc xá chập chờn:** Khi mạng yếu lúc bấm lưu hồ sơ, nếu ứng dụng xóa sạch dữ liệu form bắt gõ lại từ đầu thì Lan sẽ rất nản lòng và muốn bỏ ứng dụng.<br>- **Tâm lý rụt rè sợ làm phiền:** Ngại nhắn tin trực tiếp cho các anh chị cựu sinh viên lớn tuổi nếu không có lời gợi ý mở đầu thân thiện. |
| **06** | **Động lực Thúc đẩy (Core Motivations)** | - Vượt qua kỳ học tiếng Anh và hòa nhập nhanh chóng vào môi trường học tập quốc tế tại FPTU.<br>- Học hỏi kinh nghiệm học bổng và các câu lạc bộ tại cơ sở Đà Nẵng. |
| **07** | **Bối cảnh & Hoàn cảnh Sử dụng (Usage Context & Environment)** | - Sử dụng điện thoại trong phòng ký túc xá hoặc trên xe buýt đưa đón sinh viên từ trung tâm TP. Đà Nẵng về trường.<br>- Đường truyền mạng 4G không ổn định tại khu vực mới phát triển. |
| **08** | **Thiết bị & Năng lực Công nghệ (Device & Tech Literacy)** | - **Thiết bị:** Smartphone Xiaomi Redmi Note, màn hình 360×800 dp, font hệ thống để mặc định tiếng Việt.<br>- **Trình độ công nghệ:** Mức cơ bản của học sinh mới tốt nghiệp cấp 3 (Quen dùng TikTok, Facebook, chưa quen các app enterprise/networking). |
| **09** | **Thói quen Mạng xã hội & Giao tiếp (Networking Behavior)** | - Thích đọc thông tin thụ động (Lurker) trước khi tham gia tương tác thật.<br>- Ưu tiên giao tiếp bằng tiếng Việt, thích các biểu tượng cảm xúc gần gũi. |
| **10** | **Khả năng Tiếp cận & Trợ giúp Lỗi (Accessibility & Error Recovery)** | - **Trạng thái phục hồi dữ liệu bắt buộc:** Khi mất mạng (Offline), form nhập liệu phải lưu tạm (cache) và hiển thị **Network Error Banner** kèm nút **"Retry"** rõ ràng (Heuristic #5 & #9).<br>- Mọi thông báo lỗi xác thực email / mật khẩu phải hiển thị ngay dưới ô nhập liệu (Inline Validation) bằng tiếng Việt rõ nghĩa, không dùng mã lỗi kỹ thuật như `Err 400` hay `Token Expired`. |
| **11** | **Tiêu chí Đánh giá Thành công (Success Metric / Value Metric)** | **Kích hoạt tài khoản thành công qua OTP trong vòng 60 giây**, và tìm thấy danh sách ít nhất 5 cựu sinh viên Campus Đà Nẵng đang hoạt động. |
| **12** | **Ánh xạ User Stories Trọng tâm (Mapped User Stories)** | `US-01, US-02, US-03, US-06, US-07, US-08, US-26, US-33, US-38` |

---

## 3. Ma trận Bao phủ Tính năng theo Personas (Coverage Matrix)

Bảng ma trận đối chiếu mức độ tương tác và đáp ứng của 3 Personas đối với **11 User Stories Cốt lõi của Lab 2 (Core Mobile Scope)** và các nhóm mở rộng của toàn hệ thống FPT Alumni Connect:

### 3.1. Ma trận 11 User Stories Cốt lõi Lab 2 (`US-01` → `US-11`)

*Ghi chú mức độ ưu tiên theo chuẩn MoSCoW: `[M]` = Must-Have (Bắt buộc); `[S]` = Should-Have (Nên có); `[C]` = Could-Have (Có thể có).*

| Mã US | Tóm tắt Nội dung User Story | `P01` (Trương Chí Trung - SV Năm 3) | `P02` (Đặng Minh Tuấn - Alumni Mentor) | `P03` (Lê Hoàng Lan - Tân SV K20) | Màn hình Đảm nhiệm | TV Phụ trách |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: |
| **`US-01`** | Đăng nhập bằng Email (@fpt.edu.vn) & Mật khẩu | `[M]` Đăng nhập thường xuyên | `[S]` Đăng nhập bằng email alumni | `[M]` Đăng nhập tài khoản mới tạo | **`S01`** | **TV1 (Tâm)** |
| **`US-02`** | Đăng nhập nhanh 1 chạm bằng Google SSO FPT | `[M]` Ưu tiên số 1 để tiết kiệm giờ | `[S]` Đăng nhập bằng Google cá nhân | `[M]` Tiện lợi, không sợ quên pass | **`S01`** | **TV1 (Tâm)** |
| **`US-03`** | Xác minh quyền sở hữu email qua mã OTP 6 số | `[S]` Đã xác minh từ trước | `[S]` Xác minh tài khoản cựu SV | `[M]` Bắt buộc khi mới kích hoạt app | **`S02`** | **TV1 (Tâm)** |
| **`US-04`** | Cập nhật hồ sơ cá nhân (Bio, Campus, Khóa, Ngành) | `[M]` Cập nhật CV, khóa K19 SE | `[M]` Cập nhật công ty, kinh nghiệm | `[M]` Điền thông tin tân sinh viên | **`S07`** | **TV4 (Tín)** |
| **`US-05`** | Xem hồ sơ công khai của thành viên khác | `[M]` Soi kỹ kỹ năng & profile Mentor | `[S]` Xem qua hồ sơ sinh viên hẹn gặp | `[M]` Xem kinh nghiệm cựu SV khóa trước | **`S05`** | **TV3 (Khải)** |
| **`US-06`** | Tìm kiếm & Lọc thành viên theo Campus & Ngành | `[M]` Lọc Campus HCMC + Ngành SE | `[C]` Tìm bạn cùng khóa K15 | `[M]` Lọc Campus Đà Nẵng + Ngành AI | **`S04`** | **TV2 (Trung)** |
| **`US-07`** | Đổi mật khẩu hoặc đặt lại mật khẩu khi quên | `[S]` Đổi mật khẩu định kỳ | `[M]` Bảo mật tài khoản chuyên gia | `[M]` Hay quên mật khẩu, cần Reset | **`S02, S08`** | **TV1 & TV4** |
| **`US-08`** | Đăng xuất an toàn & Hủy phiên token đăng nhập | `[M]` Đăng xuất khi mượn máy | `[M]` Đăng xuất bảo vệ dữ liệu | `[S]` Đăng xuất thông thường | **`S01, S08`** | **TV1 & TV4** |
| **`US-09`** | Tùy chỉnh ẩn/hiện thông tin cá nhân (Privacy Switch) | `[S]` Giữ liên hệ mở để kết nối | `[M]` Ẩn SĐT, ẩn email, chỉ mở app | `[S]` Ẩn thông tin cá nhân cơ bản | **`S08`** | **TV4 (Tín)** |
| **`US-10`** | Chặn tài khoản quấy rối & Quản lý danh sách chặn | `[S]` Chặn tài khoản spam bài | `[M]` Chặn sinh viên quấy rối ngoài giờ | `[S]` Chặn tin nhắn làm phiền | **`S05, S08`** | **TV3 & TV4** |
| **`US-11`** | Add link lịch Google Calendar vào Profile để liên hệ khi cần | `[M]` Xem link Calendar của Mentor để biết giờ liên hệ | `[M]` Thêm link Calendar cá nhân vào Profile | `[S]` Xem lịch anh chị khóa trước | **`S05, S07`** | **TV3 & TV4** |

---

### 3.2. Ánh xạ Mở rộng sang Toàn Hệ sinh thái Dự án (50 User Stories Context)

- **Nhóm Chat Thời gian thực (`US-12` → `US-21` — Màn hình `S06` do TV3 phụ trách):**
  - Cả `P01` và `P02` sử dụng tính năng nhắn tin trao đổi 1-1 qua tính năng Chat trong app (`US-12, US-13`), kiểm tra trạng thái đã xem (`US-16`) và gửi ảnh CV hoặc sơ đồ kiến trúc (`US-18`).
- **Nhóm Bảng tin Forum & Hỏi đáp (`US-22` → `US-37` — Màn hình `S03` do TV2 phụ trách):**
  - `P01` (Trung) tạo bài hỏi đáp chuẩn bị OJT (`US-22, US-23`).
  - `P02` (Tuấn) đăng bài chia sẻ tuyển dụng và kinh nghiệm hệ thống lớn (`US-23, US-26`).
  - `P03` (Lan) lọc các câu hỏi theo Campus Đà Nẵng và xem các câu trả lời đã giải đáp (`US-33, US-37`).
- **Nhóm Trợ lý AI RAG & Quản trị Admin (`US-38` → `US-50`):**
  - `P03` (Lan) hỏi đáp với AI về kinh nghiệm học tập FPT (`US-38..US-41`).
  - Đội ngũ Quản trị viên FPTU theo dõi người dùng và kiểm duyệt nội dung vi phạm (`US-42..US-50`).

---

## 4. Chuỗi Truy vết Giá trị Người dùng (Traceability Matrix)

Sơ đồ quan hệ bảo chứng tính liêm chính học thuật và tính truy vết từ nghiên cứu đến thiết kế:

```
[Bằng chứng Nghiên cứu E01..E04]
        │
        ▼
[3 Personas Chuẩn P01..P03]
        │
        ▼
[11 User Stories Cốt lõi US-01..US-11]
        │
        ▼
[3 User Flows F01..F03] ──► [8 Màn hình Chuẩn S01..S08]
                                    │
                                    ▼
                        [Design Tokens & Components]
                                    │
                                    ▼
                        [Kiểm tra WCAG AA & 48dp Touch]
```

Bộ Persona này cung cấp đầy đủ luận cứ để TV1 tiến hành xây dựng sơ đồ Kiến trúc Thông tin và Luồng Người dùng trong tài liệu kế tiếp (`ux/user-flow.md`).
