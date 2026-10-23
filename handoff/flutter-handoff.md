# 📱 Tài liệu Bàn giao Kỹ thuật Lập trình Flutter (Flutter Developer Handoff)

> **Dự án:** **FPT Alumni Connect** — Nền tảng kết nối Cựu sinh viên & Sinh viên Đại học FPT  
> **Môn học:** PRM323 — Cross-Platform Application Development (Lab 2: Google Stitch → Figma → Flutter)  
> **Người thực hiện chính (Lead Author):** **Võ Quang Tâm** — MSSV: **SE192621** (Vai trò: **TV1 — UX Research, Persona & User Flow Lead**)  
> **Màn hình Bàn giao Trực tiếp:** **`S01`** (Login & Google Auth) và **`S02`** (Student Verification & Password Reset).  
> **Bộ tiêu chuẩn:** Flutter 3.x / Dart 3.x, Kiến trúc BLoC / Clean Architecture, Khả năng tiếp cận TalkBack/VoiceOver Semantics, Viewport chuẩn **360×800 dp**.

---

## 1. Bảng Ánh xạ Design Tokens sang Mã nguồn Flutter (Token Mapping)

Bảng đối chiếu trực tiếp giữa Figma Variables/Tokens sang các lớp hằng số Dart trong dự án Flutter:

```dart
// lib/core/theme/app_colors.dart
import 'package:flutter/material.dart';

class AppColors {
  AppColors._();

  // Primary & Brand Colors
  static const Color fptNavy = Color(0xFF1B365D);      // AppBar, Headings, Focus Border
  static const Color fptOrange = Color(0xFFF27024);    // Primary CTA Button, Active Tab
  static const Color emeraldGreen = Color(0xFF10B981); // Verified Badge, Success State
  static const Color dangerRed = Color(0xFFDC2626);    // Inline Error, Block Button
  static const Color dangerRedLight = Color(0xFFFDF3F2);// Network Error Banner Background

  // Neutral & Typography Colors
  static const Color textPrimary = Color(0xFF111827);  // H1, H2, Form Input Text
  static const Color textSecondary = Color(0xFF595959);// Subtitle, Placeholder (Contrast 5.4:1)
  static const Color surfaceWhite = Color(0xFFFFFFFF); // Card Surface, Input Background
  static const Color backgroundLight = Color(0xFFF8F9FA);// Screen Background
  static const Color borderGray = Color(0xFFE5E7EB);   // Default Input Border, Dividers
  static const Color disabledGray = Color(0xFFD1D5DB); // Disabled Button Background
}
```

```dart
// lib/core/theme/app_typography.dart
import 'package:flutter/material.dart';
import 'app_colors.dart';

class AppTypography {
  AppTypography._();

  // Headings (Hanken Grotesk / Inter fallback)
  static const TextStyle h1Bold = TextStyle(
    fontFamily: 'Hanken Grotesk',
    fontSize: 24.0,
    fontWeight: FontWeight.w700,
    height: 32.0 / 24.0,
    color: AppColors.textPrimary,
  );

  static const TextStyle h2Bold = TextStyle(
    fontFamily: 'Hanken Grotesk',
    fontSize: 20.0,
    fontWeight: FontWeight.w700,
    height: 28.0 / 20.0,
    color: AppColors.textPrimary,
  );

  // Body & Buttons (Inter)
  static const TextStyle bodyLarge = TextStyle(
    fontFamily: 'Inter',
    fontSize: 16.0,
    fontWeight: FontWeight.w400,
    height: 24.0 / 16.0,
    color: AppColors.textPrimary,
  );

  static const TextStyle buttonPrimary = TextStyle(
    fontFamily: 'Inter',
    fontSize: 16.0,
    fontWeight: FontWeight.w700,
    height: 24.0 / 16.0,
    color: AppColors.surfaceWhite,
  );

  static const TextStyle bodyMedium = TextStyle(
    fontFamily: 'Inter',
    fontSize: 14.0,
    fontWeight: FontWeight.w400,
    height: 20.0 / 14.0,
    color: AppColors.textSecondary,
  );

  static const TextStyle bodyMediumSemiBold = TextStyle(
    fontFamily: 'Inter',
    fontSize: 14.0,
    fontWeight: FontWeight.w600,
    height: 20.0 / 14.0,
    color: AppColors.textPrimary,
  );

  static const TextStyle caption = TextStyle(
    fontFamily: 'Inter',
    fontSize: 12.0,
    fontWeight: FontWeight.w400,
    height: 16.0 / 12.0,
    color: AppColors.textSecondary,
  );

  static const TextStyle captionError = TextStyle(
    fontFamily: 'Inter',
    fontSize: 12.0,
    fontWeight: FontWeight.w500,
    height: 16.0 / 12.0,
    color: AppColors.dangerRed,
  );
}
```

```dart
// lib/core/theme/app_dimens.dart
import 'package:flutter/material.dart';

class AppDimens {
  AppDimens._();

  // Spacings
  static const double space4 = 4.0;
  static const double space8 = 8.0;
  static const double space12 = 12.0;
  static const double space16 = 16.0;
  static const double space24 = 24.0;
  static const double space32 = 32.0;

  // Touch Target & Heights
  static const double minTouchTarget = 48.0;
  static const double buttonHeight = 48.0;
  static const double inputHeight = 48.0;

  // Border Radii
  static const BorderRadius radius8 = BorderRadius.all(Radius.circular(8.0));
  static const BorderRadius radius12 = BorderRadius.all(Radius.circular(12.0));
}
```

---

## 2. Bàn giao Kỹ thuật Màn hình `S01`: Đăng nhập & Google SSO

### 2.1. Cây Cấu trúc Widget Flutter (Widget Tree Structure)

```
Scaffold (backgroundColor: AppColors.backgroundLight)
└── SafeArea
    └── SingleChildScrollView (keyboardDismissBehavior: onDrag)
        └── Padding (EdgeInsets.symmetric(horizontal: 16.0, vertical: 24.0))
            └── Form (key: _formKey)
                └── Column (crossAxisAlignment: CrossAxisAlignment.stretch)
                    ├── HeaderSection (Logo 48x48, H1 Title, Subtitle)
                    ├── SizedBox(height: 24.0)
                    ├── GoogleSignInButton (Height: 48.0, Outlined, Icon G + Text)
                    ├── SizedBox(height: 20.0)
                    ├── OrDividerRow (1dp Divider + Text caption)
                    ├── SizedBox(height: 20.0)
                    ├── EmailOrIdInputField (TextFormField, 48dp)
                    ├── SizedBox(height: 16.0)
                    ├── PasswordInputField (TextFormField, 48dp, Suffix Eye Icon)
                    ├── SizedBox(height: 12.0)
                    ├── RememberAndForgotRow (Checkbox + TextButton 'Quên mật khẩu?')
                    ├── SizedBox(height: 24.0)
                    ├── PrimaryLoginButton (ElevatedButton, Height: 48.0, Orange)
                    ├── SizedBox(height: 24.0)
                    └── OnboardingFooterLink (Text.rich -> Navigator to S02)
```

### 2.2. Mã Nguồn Mẫu Hoàn chỉnh cho Form `S01` (Production-Ready Dart)

```dart
// lib/features/auth/presentation/screens/login_screen.dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import '../../../../core/theme/app_colors.dart';
import '../../../../core/theme/app_dimens.dart';
import '../../../../core/theme/app_typography.dart';

class LoginScreen extends StatefulWidget {
  const LoginScreen({super.key});

  @override
  State<LoginScreen> createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  final _formKey = GlobalKey<FormState>();
  final _emailOrIdController = TextEditingController();
  final _passwordController = TextEditingController();

  final _emailFocusNode = FocusNode();
  final _passwordFocusNode = FocusNode();

  bool _obscurePassword = true;
  bool _rememberMe = false;
  bool _isLoading = false;
  int _failedAttempts = 0;
  int _lockoutCountdown = 0;

  @override
  void dispose() {
    _emailOrIdController.dispose();
    _passwordController.dispose();
    _emailFocusNode.dispose();
    _passwordFocusNode.dispose();
    super.dispose();
  }

  void _handleLogin() {
    if (_lockoutCountdown > 0) return;
    if (!_formKey.currentState!.validate()) return;

    setState(() => _isLoading = true);

    // Giả lập xử lý xác thực đăng nhập
    Future.delayed(const Duration(milliseconds: 1500), () {
      if (!mounted) return;
      setState(() => _isLoading = false);

      // Mô phỏng kiểm tra sai mật khẩu / khóa brute-force
      final pass = _passwordController.text;
      if (pass != 'Fpt@2026') {
        setState(() {
          _failedAttempts++;
          if (_failedAttempts >= 5) {
            _lockoutCountdown = 60;
            _startLockoutTimer();
          }
        });
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(
            backgroundColor: AppColors.dangerRed,
            content: Text(
              _failedAttempts >= 5
                  ? 'Bạn đã nhập sai 5 lần. Vui lòng đợi 60 giây.'
                  : 'Mật khẩu không đúng. Còn lại ${5 - _failedAttempts} lần thử.',
            ),
          ),
        );
      } else {
        // Đăng nhập thành công -> Điều hướng sang S03 Home Feed
        Navigator.pushReplacementNamed(context, '/home');
      }
    });
  }

  void _startLockoutTimer() {
    Future.doWhile(() async {
      await Future.delayed(const Duration(seconds: 1));
      if (!mounted) return false;
      setState(() {
        if (_lockoutCountdown > 0) _lockoutCountdown--;
      });
      return _lockoutCountdown > 0;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: AppColors.backgroundLight,
      body: SafeArea(
        child: Center(
          child: SingleChildScrollView(
            padding: const EdgeInsets.symmetric(horizontal: AppDimens.space16, vertical: AppDimens.space24),
            child: Form(
              key: _formKey,
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.stretch,
                children: [
                  // Logo & Brand Header
                  Center(
                    child: Container(
                      width: 56.0,
                      height: 56.0,
                      decoration: BoxDecoration(
                        color: AppColors.fptNavy,
                        borderRadius: AppDimens.radius12,
                      ),
                      child: const Icon(Icons.school, color: AppColors.surfaceWhite, size: 32.0),
                    ),
                  ),
                  const SizedBox(height: AppDimens.space16),
                  const Text('Chào mừng trở lại!', style: AppTypography.h1Bold, textAlign: TextAlign.center),
                  const SizedBox(height: AppDimens.space4),
                  const Text('Nền tảng kết nối Cựu sinh viên & Sinh viên ĐH FPT', style: AppTypography.bodyMedium, textAlign: TextAlign.center),
                  const SizedBox(height: AppDimens.space24),

                  // Nút Google SSO 1 chạm (US-02)
                  OutlinedButton.icon(
                    onPressed: _isLoading ? null : () {},
                    icon: const Icon(Icons.g_mobiledata, size: 28.0, color: AppColors.dangerRed),
                    label: const Text('Tiếp tục với Google', style: AppTypography.bodyLarge),
                    style: OutlinedButton.styleFrom(
                      minimumSize: const Size(double.infinity, AppDimens.buttonHeight),
                      backgroundColor: AppColors.surfaceWhite,
                      side: const BorderSide(color: AppColors.borderGray, width: 1.0),
                      shape: const RoundedRectangleBorder(borderRadius: AppDimens.radius8),
                    ),
                  ),
                  const SizedBox(height: AppDimens.space20),

                  // Đường kẻ phân cách
                  Row(
                    children: const [
                      Expanded(child: Divider(color: AppColors.borderGray)),
                      Padding(
                        padding: EdgeInsets.symmetric(horizontal: AppDimens.space8),
                        child: Text('hoặc bằng tài khoản sinh viên', style: AppTypography.caption),
                      ),
                      Expanded(child: Divider(color: AppColors.borderGray)),
                    ],
                  ),
                  const SizedBox(height: AppDimens.space20),

                  // Ô nhập Email / MSSV
                  const Text('Email hoặc Mã số sinh viên', style: AppTypography.bodyMediumSemiBold),
                  const SizedBox(height: AppDimens.space8),
                  TextFormField(
                    controller: _emailOrIdController,
                    focusNode: _emailFocusNode,
                    decoration: InputDecoration(
                      hintText: 'sinhvien@fpt.edu.vn hoặc SE193059',
                      hintStyle: AppTypography.caption,
                      prefixIcon: const Icon(Icons.person_outline, size: 20.0, color: AppColors.textSecondary),
                      contentPadding: const EdgeInsets.symmetric(horizontal: AppDimens.space16, vertical: 14.0),
                      filled: true,
                      fillColor: AppColors.surfaceWhite,
                      border: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.borderGray)),
                      enabledBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.borderGray)),
                      focusedBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.fptNavy, width: 2.0)),
                      errorBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.dangerRed, width: 1.5)),
                    ),
                    validator: (value) {
                      if (value == null || value.trim().isEmpty) return 'Vui lòng không để trống trường này';
                      return null;
                    },
                  ),
                  const SizedBox(height: AppDimens.space16),

                  // Ô nhập Mật khẩu
                  const Text('Mật khẩu', style: AppTypography.bodyMediumSemiBold),
                  const SizedBox(height: AppDimens.space8),
                  TextFormField(
                    controller: _passwordController,
                    focusNode: _passwordFocusNode,
                    obscureText: _obscurePassword,
                    decoration: InputDecoration(
                      hintText: 'Nhập mật khẩu của bạn',
                      hintStyle: AppTypography.caption,
                      prefixIcon: const Icon(Icons.lock_outline, size: 20.0, color: AppColors.textSecondary),
                      suffixIcon: IconButton(
                        icon: Icon(_obscurePassword ? Icons.visibility_off : Icons.visibility, color: AppColors.textSecondary),
                        onPressed: () => setState(() => _obscurePassword = !_obscurePassword),
                      ),
                      contentPadding: const EdgeInsets.symmetric(horizontal: AppDimens.space16, vertical: 14.0),
                      filled: true,
                      fillColor: AppColors.surfaceWhite,
                      border: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.borderGray)),
                      enabledBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.borderGray)),
                      focusedBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.fptNavy, width: 2.0)),
                      errorBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.dangerRed, width: 1.5)),
                    ),
                    validator: (value) {
                      if (value == null || value.isEmpty) return 'Vui lòng nhập mật khẩu';
                      return null;
                    },
                  ),
                  const SizedBox(height: AppDimens.space12),

                  // Checkbox Ghi nhớ & Quên mật khẩu
                  Row(
                    mainAxisAlignment: MainAxisAlignment.spaceBetween,
                    children: [
                      Row(
                        children: [
                          Checkbox(
                            value: _rememberMe,
                            activeColor: AppColors.fptNavy,
                            onChanged: (val) => setState(() => _rememberMe = val ?? false),
                          ),
                          const Text('Ghi nhớ đăng nhập', style: AppTypography.bodyMedium),
                        ],
                      ),
                      TextButton(
                        onPressed: () => Navigator.pushNamed(context, '/verify-reset', arguments: {'mode': 'reset'}),
                        child: const Text('Quên mật khẩu?', style: TextStyle(color: AppColors.fptNavy, fontWeight: FontWeight.w600)),
                      ),
                    ],
                  ),
                  const SizedBox(height: AppDimens.space16),

                  // Nút Đăng nhập Chính (Primary CTA - US-01)
                  ElevatedButton(
                    onPressed: (_isLoading || _lockoutCountdown > 0) ? null : _handleLogin,
                    style: ElevatedButton.styleFrom(
                      minimumSize: const Size(double.infinity, AppDimens.buttonHeight),
                      backgroundColor: AppColors.fptOrange,
                      disabledBackgroundColor: AppColors.disabledGray,
                      shape: const RoundedRectangleBorder(borderRadius: AppDimens.radius8),
                      elevation: 1.0,
                    ),
                    child: _isLoading
                        ? const SizedBox(width: 24.0, height: 24.0, child: CircularProgressIndicator(color: AppColors.surfaceWhite, strokeWidth: 2.5))
                        : Text(
                            _lockoutCountdown > 0 ? 'Thử lại sau ${_lockoutCountdown}s' : 'ĐĂNG NHẬP',
                            style: AppTypography.buttonPrimary,
                          ),
                  ),
                  const SizedBox(height: AppDimens.space24),

                  // Chuyển sang màn hình Xác thực sinh viên S02
                  Center(
                    child: TextButton(
                      onPressed: () => Navigator.pushNamed(context, '/verify-reset', arguments: {'mode': 'verify'}),
                      child: const Text.rich(
                        TextSpan(
                          text: 'Chưa xác thực thông tin? ',
                          style: AppTypography.bodyMedium,
                          children: [
                            TextSpan(text: 'Xác thực sinh viên ngay →', style: TextStyle(color: AppColors.fptNavy, fontWeight: FontWeight.w600)),
                          ],
                        ),
                      ),
                    ),
                  ),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```

---

## 3. Bàn giao Kỹ thuật Màn hình `S02`: Xác thực Học vụ Sinh viên (`US-03`) & Đổi Mật khẩu (`US-07`)

### 3.1. Đặc tả Logic Form Xác thực Học vụ (Không ép cứng email FPT)

Màn hình `S02` xác thực danh tính sinh viên thông qua **4 trường dữ liệu học vụ chuẩn của FPT University**:
1. **Mã số sinh viên (MSSV):** Định dạng chuẩn `^[A-Z]{2}\d{6}$`. Sử dụng `TextInputFormatter` để tự động in hoa mọi ký tự người dùng gõ.
2. **Campus theo học:** Dropdown với 5 cơ sở trường: `TP. Hồ Chí Minh`, `Hà Nội`, `Đà Nẵng`, `Cần Thơ`, `Quy Nhơn`.
3. **Ngành đào tạo (Major):** Dropdown gồm 4 khối ngành chính.
4. **Chuyên ngành hẹp liên động (Cascading Specialization):** Khi chọn ngành, danh sách chuyên ngành sẽ tự động thay đổi tương ứng.

### 3.2. Cấu trúc Dữ liệu Liên động (Cascading Dropdown Map)

```dart
// lib/features/auth/data/academic_constants.dart

class AcademicConstants {
  AcademicConstants._();

  static const List<String> campuses = [
    'TP. Hồ Chí Minh',
    'Hà Nội',
    'Đà Nẵng',
    'Cần Thơ',
    'Quy Nhơn',
  ];

  static const Map<String, List<String>> majorToSpecializations = {
    'Công nghệ thông tin': [
      'Kỹ thuật phần mềm (SE)',
      'Trí tuệ nhân tạo (AI)',
      'An toàn thông tin (IA)',
      'Hệ thống thông tin (IS)',
      'Thiết kế đồ họa (GD)',
    ],
    'Quản trị kinh doanh': [
      'Digital Marketing',
      'Kinh doanh quốc tế',
      'Quản trị truyền thông đa phương tiện',
      'Tài chính doanh nghiệp',
    ],
    'Ngôn ngữ': [
      'Ngôn ngữ Anh',
      'Ngôn ngữ Nhật',
      'Ngôn ngữ Hàn',
    ],
  };
}
```

### 3.3. Mã Nguồn Mẫu Form Xác thực Sinh viên `S02` (Production-Ready Dart)

```dart
// lib/features/auth/presentation/screens/student_verification_screen.dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import '../../../../core/theme/app_colors.dart';
import '../../../../core/theme/app_dimens.dart';
import '../../../../core/theme/app_typography.dart';
import '../../data/academic_constants.dart';

class StudentVerificationScreen extends StatefulWidget {
  const StudentVerificationScreen({super.key});

  @override
  State<StudentVerificationScreen> createState() => _StudentVerificationScreenState();
}

class _StudentVerificationScreenState extends State<StudentVerificationScreen> {
  final _formKey = GlobalKey<FormState>();
  final _mssvController = TextEditingController();

  String? _selectedCampus;
  String? _selectedMajor;
  String? _selectedSpecialization;
  bool _isSubmitting = false;

  @override
  void dispose() {
    _mssvController.dispose();
    super.dispose();
  }

  void _handleSubmitVerification() {
    if (!_formKey.currentState!.validate()) return;

    setState(() => _isSubmitting = true);

    // Giả lập lưu hồ sơ học vụ sinh viên
    Future.delayed(const Duration(milliseconds: 1200), () {
      if (!mounted) return;
      setState(() => _isSubmitting = false);

      // Hiển thị Dialog Thành công (Success State)
      showDialog(
        context: context,
        barrierDismissible: false,
        builder: (ctx) => AlertDialog(
          shape: const RoundedRectangleBorder(borderRadius: AppDimens.radius12),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            children: [
              const Icon(Icons.check_circle, color: AppColors.emeraldGreen, size: 60.0),
              const SizedBox(height: AppDimens.space16),
              const Text('Xác thực Thành công!', style: AppTypography.h2Bold, textAlign: TextAlign.center),
              const SizedBox(height: AppDimens.space8),
              Text(
                'MSSV ${_mssvController.text} đã được liên kết với cơ sở $_selectedCampus ngành $_selectedSpecialization.',
                style: AppTypography.bodyMedium,
                textAlign: TextAlign.center,
              ),
            ],
          ),
        ),
      );

      // Tự động chuyển tiếp sau 2 giây
      Future.delayed(const Duration(seconds: 2), () {
        if (!mounted) return;
        Navigator.pushNamedAndRemoveUntil(context, '/home', (route) => false);
      });
    });
  }

  @override
  Widget build(BuildContext context) {
    final availableSpecializations = _selectedMajor != null
        ? AcademicConstants.majorToSpecializations[_selectedMajor!] ?? []
        : <String>[];

    return Scaffold(
      backgroundColor: AppColors.backgroundLight,
      appBar: AppBar(
        backgroundColor: AppColors.surfaceWhite,
        elevation: 0.5,
        leading: IconButton(
          icon: const Icon(Icons.arrow_back, color: AppColors.textPrimary),
          onPressed: () => Navigator.pop(context),
        ),
        title: const Text('Xác thực Sinh viên FPT', style: AppTypography.h2Bold),
      ),
      body: SafeArea(
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(AppDimens.space16),
          child: Form(
            key: _formKey,
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: [
                const Text(
                  'Điền chính xác thông tin học vụ để hệ thống kết nối bạn đúng với cựu sinh viên và bạn bè cùng chuyên ngành.',
                  style: AppTypography.bodyMedium,
                ),
                const SizedBox(height: AppDimens.space24),

                // 1. Ô nhập Mã số sinh viên (MSSV)
                const Text('Mã số sinh viên (MSSV) *', style: AppTypography.bodyMediumSemiBold),
                const SizedBox(height: AppDimens.space8),
                TextFormField(
                  controller: _mssvController,
                  textCapitalization: TextCapitalization.characters,
                  inputFormatters: [
                    FilteringTextInputFormatter.allow(RegExp(r'[a-zA-Z0-9]')),
                    TextInputFormatter.withFunction((oldValue, newValue) {
                      return newValue.copyWith(text: newValue.text.toUpperCase());
                    }),
                    LengthLimitingTextInputFormatter(8),
                  ],
                  decoration: InputDecoration(
                    hintText: 'Ví dụ: SE193059, DE200114',
                    hintStyle: AppTypography.caption,
                    prefixIcon: const Icon(Icons.badge_outlined, size: 20.0, color: AppColors.textSecondary),
                    filled: true,
                    fillColor: AppColors.surfaceWhite,
                    contentPadding: const EdgeInsets.symmetric(horizontal: AppDimens.space16, vertical: 14.0),
                    border: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.borderGray)),
                    focusedBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.fptNavy, width: 2.0)),
                    errorBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.dangerRed, width: 1.5)),
                  ),
                  validator: (value) {
                    if (value == null || value.trim().isEmpty) return 'Vui lòng nhập Mã số sinh viên';
                    final mssvRegex = RegExp(r'^[A-Z]{2}\d{6}$');
                    if (!mssvRegex.hasMatch(value.trim())) {
                      return 'MSSV không đúng định dạng (Gồm 2 chữ cái và 6 số, ví dụ: SE193059)';
                    }
                    return null;
                  },
                ),
                const SizedBox(height: AppDimens.space16),

                // 2. Dropdown Campus
                const Text('Campus theo học *', style: AppTypography.bodyMediumSemiBold),
                const SizedBox(height: AppDimens.space8),
                DropdownButtonFormField<String>(
                  value: _selectedCampus,
                  isExpanded: true,
                  hint: const Text('Chọn cơ sở đào tạo', style: AppTypography.caption),
                  items: AcademicConstants.campuses.map((campus) {
                    return DropdownMenuItem(value: campus, child: Text(campus, style: AppTypography.bodyLarge));
                  }).toList(),
                  onChanged: (val) => setState(() => _selectedCampus = val),
                  decoration: InputDecoration(
                    prefixIcon: const Icon(Icons.location_on_outlined, size: 20.0, color: AppColors.textSecondary),
                    filled: true,
                    fillColor: AppColors.surfaceWhite,
                    contentPadding: const EdgeInsets.symmetric(horizontal: AppDimens.space16, vertical: 14.0),
                    border: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.borderGray)),
                    focusedBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.fptNavy, width: 2.0)),
                    errorBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.dangerRed, width: 1.5)),
                  ),
                  validator: (val) => val == null ? 'Vui lòng chọn cơ sở theo học' : null,
                ),
                const SizedBox(height: AppDimens.space16),

                // 3. Dropdown Ngành đào tạo
                const Text('Ngành đào tạo (Major) *', style: AppTypography.bodyMediumSemiBold),
                const SizedBox(height: AppDimens.space8),
                DropdownButtonFormField<String>(
                  value: _selectedMajor,
                  isExpanded: true,
                  hint: const Text('Chọn khối ngành', style: AppTypography.caption),
                  items: AcademicConstants.majorToSpecializations.keys.map((major) {
                    return DropdownMenuItem(value: major, child: Text(major, style: AppTypography.bodyLarge));
                  }).toList(),
                  onChanged: (val) {
                    setState(() {
                      _selectedMajor = val;
                      _selectedSpecialization = null; // Reset chuyên ngành khi đổi ngành
                    });
                  },
                  decoration: InputDecoration(
                    prefixIcon: const Icon(Icons.school_outlined, size: 20.0, color: AppColors.textSecondary),
                    filled: true,
                    fillColor: AppColors.surfaceWhite,
                    contentPadding: const EdgeInsets.symmetric(horizontal: AppDimens.space16, vertical: 14.0),
                    border: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.borderGray)),
                    focusedBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.fptNavy, width: 2.0)),
                    errorBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.dangerRed, width: 1.5)),
                  ),
                  validator: (val) => val == null ? 'Vui lòng chọn ngành đào tạo' : null,
                ),
                const SizedBox(height: AppDimens.space16),

                // 4. Dropdown Chuyên ngành hẹp liên động (Cascading)
                const Text('Chuyên ngành hẹp (Specialization) *', style: AppTypography.bodyMediumSemiBold),
                const SizedBox(height: AppDimens.space8),
                DropdownButtonFormField<String>(
                  value: _selectedSpecialization,
                  isExpanded: true,
                  hint: Text(
                    _selectedMajor == null ? 'Hãy chọn Ngành đào tạo trước' : 'Chọn chuyên ngành hẹp',
                    style: AppTypography.caption,
                  ),
                  items: availableSpecializations.map((spec) {
                    return DropdownMenuItem(value: spec, child: Text(spec, style: AppTypography.bodyLarge));
                  }).toList(),
                  onChanged: _selectedMajor == null ? null : (val) => setState(() => _selectedSpecialization = val),
                  decoration: InputDecoration(
                    prefixIcon: const Icon(Icons.computer_outlined, size: 20.0, color: AppColors.textSecondary),
                    filled: true,
                    fillColor: _selectedMajor == null ? AppColors.backgroundLight : AppColors.surfaceWhite,
                    contentPadding: const EdgeInsets.symmetric(horizontal: AppDimens.space16, vertical: 14.0),
                    border: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.borderGray)),
                    focusedBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.fptNavy, width: 2.0)),
                    errorBorder: OutlineInputBorder(borderRadius: AppDimens.radius8, borderSide: const BorderSide(color: AppColors.dangerRed, width: 1.5)),
                  ),
                  validator: (val) => val == null ? 'Vui lòng chọn chuyên ngành của bạn' : null,
                ),
                const SizedBox(height: AppDimens.space32),

                // Nút CTA Xác nhận & Hoàn tất
                ElevatedButton(
                  onPressed: _isSubmitting ? null : _handleSubmitVerification,
                  style: ElevatedButton.styleFrom(
                    minimumSize: const Size(double.infinity, AppDimens.buttonHeight),
                    backgroundColor: AppColors.fptOrange,
                    shape: const RoundedRectangleBorder(borderRadius: AppDimens.radius8),
                  ),
                  child: _isSubmitting
                      ? const SizedBox(width: 24.0, height: 24.0, child: CircularProgressIndicator(color: AppColors.surfaceWhite, strokeWidth: 2.5))
                      : const Text('XÁC NHẬN & HOÀN TẤT', style: AppTypography.buttonPrimary),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```

---

## 4. Khung Nhãn Trợ năng (Accessibility & Semantics) trong Flutter

Để bảo đảm ứng dụng vượt qua bài kiểm tra tiếp cận (Accessibility Testing):
1. **Ô nhập Mật khẩu:**
   ```dart
   Semantics(
     label: 'Mật khẩu',
     hint: 'Chạm đúp vào biểu tượng con mắt để ẩn hoặc hiện mật khẩu',
     textField: true,
     child: ...
   )
   ```
2. **Nút bấm Hành động:** Mọi `ElevatedButton`, `OutlinedButton`, `IconButton` đều đảm bảo `minTouchTargetSize: Size(48.0, 48.0)` theo đúng quy tắc Material 3 của Google.
3. **Dropdown Học vụ:** Cung cấp thông báo khi thay đổi lựa chọn để TalkBack đọc to tên cơ sở hoặc chuyên ngành đã chọn cho người khiếm thị.

---

## 5. Bảng 2: Ánh xạ 9 Nhóm Figma Components sang Flutter Widgets (Components ➔ Widgets Mapping)

*(Do **TV3 — Nguyễn Phương Khải** thiết lập để ánh xạ 1-1 từ **Figma Page 05 (`05_Components`)** sang bộ Widget tái sử dụng của Flutter)*

| Mã Component (`Page 05`) | Tên & Variants trên Figma | Widget Flutter Tương ứng | Cấu hình Thuộc tính & Ràng buộc Kỹ thuật trong Flutter |
| :---: | :--- | :--- | :--- |
| **`CMP-01`** | **Button**<br>• `Primary-CTA (#F27024)`<br>• `Secondary-Navy (#1B365D)`<br>• `Secondary-Outlined`<br>• `Secondary-Tonal`<br>• `Destructive-Outlined (#DC2626)`<br>• `Destructive-Solid (#DC2626)` | • `FilledButton.icon()`<br>• `ElevatedButton()`<br>• `OutlinedButton.icon()`<br>• `FilledButton.tonalIcon()` | - Bắt buộc gán `minimumSize: const Size.fromHeight(48)` (hoặc `56` cho nút Calendar 2 dòng trên `S05`).<br>- `shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(8))`.<br>- Trạng thái `Loading`: thay `icon` bằng `SizedBox(width: 18, height: 18, child: CircularProgressIndicator(strokeWidth: 2))` và truyền `onPressed: null`. |
| **`CMP-02`** | **Text Field, Filter Chip & Switch**<br>• `Input (Default/Focused/Error)`<br>• `Filter Chip (48dp + ✓)`<br>• `Privacy Switch ([ON]/[OFF])` | • `TextFormField()`<br>• `FilterChip()`<br>• `Row(children: [Badge, Switch()])` | - **`TextFormField`:** Dùng `InputDecoration(errorMaxLines: 2)` với `errorBorder: OutlineInputBorder(borderSide: BorderSide(color: AppColors.errorCrimson, width: 2))`.<br>- **`FilterChip`:** Bọc trong `SizedBox(height: 48)`, bật `showCheckmark: true`, `checkmarkColor: Colors.white`, `selectedColor: AppColors.primaryNavy` (`D04`, `D05`).<br>- **`Switch`:** Đặt `Container` nhãn `[ON]` / `[OFF]` ngay bên trái `Switch` trong cùng một `InkWell` cao `48dp` (`D05`). |
| **`CMP-03`** | **Card**<br>• `Alumni-Directory-Card`<br>• `Feed-Post-Card`<br>• `Profile-Section-Card` | `Card()` + `Padding(padding: EdgeInsets.all(16))` + `Column()` | - `elevation: 0`, `color: Colors.white`, `shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12), side: BorderSide(color: AppColors.borderDefault, width: 1))`.<br>- Dải tag kỹ năng bên trong dùng `Wrap(spacing: 8, runSpacing: 8)` để tự động xuống dòng khi đổi từ màn `360dp` sang `412dp`. |
| **`CMP-04`** | **Navigation**<br>• `Bottom Navigation Bar (4 Tabs)`<br>• `Segmented Space Switcher` | • `NavigationBar()` (Material 3)<br>• `SegmentedButton<CampusSpace>()` | - `NavigationBar(height: 64, backgroundColor: Colors.white, indicatorColor: AppColors.orangeTint)` với 4 `NavigationDestination` (`Home`, `Directory`, `Chat`, `Profile`).<br>- Khi màn hình `width >= 600dp`, tự động chuyển sang `NavigationRail` bên trái. |
| **`CMP-05`** | **App Bar**<br>• `Top-Level-Brand`<br>• `Detail-Back`<br>• `Chat-Room-Header` | `AppBar()` | - `toolbarHeight: 56`, `backgroundColor: Colors.white`, `surfaceTintColor: Colors.transparent`, `bottom: PreferredSize(preferredSize: Size.fromHeight(1), child: Divider(height: 1))`.<br>- Mỗi `Scaffold` chỉ khai báo **duy nhất 1 `appBar`** (tuân thủ quyết định `D01`). |
| **`CMP-06`** | **Dialog / Confirmation Modal**<br>• `Block-User-Dialog (US-10)`<br>• `Unblock-Dialog` | `showDialog()` + `AlertDialog()` | - `shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(16))`, `backgroundColor: Colors.white`.<br>- Chứa icon cảnh báo tròn đỏ `48×48dp`, tiêu đề `18sp Bold`, nội dung giải thích `14sp #595959`, và 2 nút `Cancel` + `Confirm Block` đều có `minimumSize: Size(120, 48)`. |
| **`CMP-07`** | **Loading State**<br>• `Skeleton-Card`<br>• `Inline-Spinner` | `Shimmer` / Custom `SkeletonCard` widget + `CircularProgressIndicator` | - Tạo `AlumniSkeletonCard` có kích thước y hệt thẻ thật (Avatar tròn `56dp`/`72dp` + các hộp bo góc `#E5E7EB`) hiển thị khi `AsyncValue.isLoading`. |
| **`CMP-08`** | **Empty State**<br>• `No-Search-Results (S04)`<br>• `Empty-Chat-Icebreakers (S06)` | Custom `EmptyStateView` (`Center` + `Padding(24)` + `Column`) | - Hiển thị icon minh họa `64×64dp` trong khung tròn `#EEF2FF`, tiêu đề `18sp Bold`, mô tả `14sp #595959` và nút hành động phục hồi cao `48dp` (`Reset All Filters` hoặc dải `Icebreaker Chips`). |
| **`CMP-09`** | **Error State**<br>• `Network-Error-Banner (S07)`<br>• `Inline-Field-Error`<br>• `Chat-Message-Failed (S06)` | Custom `NetworkErrorBanner` (`Container` `#FDF3F2` + viền `#DC2626`) | - Giữ nguyên toàn bộ `TextEditingController` trong State để không mất dữ liệu người dùng đã gõ (`D06`), cung cấp nút **`↻ Retry Save`** cao `48dp` gọi lại hàm lưu bất đồng bộ. |

---

## 6. Bộ Code Khởi tạo Theme Chuẩn (`lib/core/theme/app_theme.dart`)

Đoạn code dưới đây hiện thực hóa toàn bộ **Bảng 1** để đội phát triển đưa trực tiếp vào dự án Flutter:

```dart
import 'package:flutter/material.dart';
import 'package:google_fonts/google_fonts.dart';

/// Hệ thống mã màu chuẩn của FPT Alumni Connect (Đồng bộ 100% với Figma Page 04)
abstract final class AppColors {
  static const Color primaryNavy = Color(0xFF1B365D);
  static const Color navyTint = Color(0xFFEEF2FF);
  static const Color ctaOrange = Color(0xFFF27024);
  static const Color orangeTint = Color(0xFFFFF7ED);
  static const Color verifiedEmerald = Color(0xFF10B981);
  static const Color emeraldTint = Color(0xFFD1FAE5);
  static const Color emeraldTextDark = Color(0xFF065F46);
  static const Color errorCrimson = Color(0xFFDC2626);
  static const Color errorTint = Color(0xFFFDF3F2);
  static const Color background = Color(0xFFF8F9FA);
  static const Color cardSurface = Color(0xFFFFFFFF);
  static const Color inputFill = Color(0xFFF3F4F6);
  static const Color textPrimary = Color(0xFF111827); // Contrast 17.7:1 on White
  static const Color textSecondary = Color(0xFF595959); // Contrast 7.0:1 on White (WCAG AA/AAA)
  static const Color borderDefault = Color(0xFFE5E7EB);
}

/// Hệ thống lưới khoảng cách 8dp & chuẩn vùng chạm 48dp (WCAG 2.1 AA)
abstract final class AppDimens {
  static const double space4 = 4.0;
  static const double space8 = 8.0;
  static const double space12 = 12.0;
  static const double space16 = 16.0;
  static const double space24 = 24.0;
  static const double space32 = 32.0;
  static const double minTouchTarget = 48.0;
  static const double primaryCtaHeight = 56.0;
  static const double radiusSm = 8.0;
  static const double radiusMd = 12.0;
  static const double radiusLg = 16.0;
}

class AlumniConnectTheme {
  static ThemeData get lightTheme {
    final baseTextTheme = GoogleFonts.interTextTheme();
    return ThemeData(
      useMaterial3: true,
      scaffoldBackgroundColor: AppColors.background,
      colorScheme: const ColorScheme.light(
        primary: AppColors.primaryNavy,
        onPrimary: Colors.white,
        primaryContainer: AppColors.navyTint,
        onPrimaryContainer: AppColors.primaryNavy,
        secondary: AppColors.ctaOrange,
        onSecondary: Colors.white,
        secondaryContainer: AppColors.orangeTint,
        error: AppColors.errorCrimson,
        onError: Colors.white,
        errorContainer: AppColors.errorTint,
        surface: AppColors.cardSurface,
        onSurface: AppColors.textPrimary,
        onSurfaceVariant: AppColors.textSecondary,
        outline: AppColors.borderDefault,
        outlineVariant: AppColors.borderDefault,
      ),
      textTheme: baseTextTheme.copyWith(
        headlineMedium: GoogleFonts.hankenGrotesk(
          fontSize: 24,
          fontWeight: FontWeight.w700,
          height: 32 / 24,
          color: AppColors.textPrimary,
        ),
        titleLarge: GoogleFonts.hankenGrotesk(
          fontSize: 20,
          fontWeight: FontWeight.w700,
          height: 28 / 20,
          color: AppColors.textPrimary,
        ),
        titleMedium: GoogleFonts.hankenGrotesk(
          fontSize: 18,
          fontWeight: FontWeight.w700,
          height: 24 / 18,
          color: AppColors.textPrimary,
        ),
        titleSmall: GoogleFonts.inter(
          fontSize: 16,
          fontWeight: FontWeight.w600,
          height: 24 / 16,
          color: AppColors.textPrimary,
        ),
        bodyLarge: GoogleFonts.inter(
          fontSize: 16,
          fontWeight: FontWeight.w400,
          height: 24 / 16,
          color: AppColors.textPrimary,
        ),
        bodyMedium: GoogleFonts.inter(
          fontSize: 14, // Bắt buộc >= 14sp theo quy tắc Lab 2
          fontWeight: FontWeight.w400,
          height: 20 / 14,
          color: AppColors.textPrimary,
        ),
        labelLarge: GoogleFonts.inter(
          fontSize: 14,
          fontWeight: FontWeight.w600,
          height: 20 / 14,
        ),
      ),
    );
  }
}
```

---

## 4. Handoff Chi tiết Màn hình `S05`: Member Public Profile & Mentorship Connect (Do TV3 Nguyễn Phương Khải Phụ trách)

### 7.1. Sơ đồ Cây Widget (`S05` Widget Tree Hierarchy)

```text
S05AlumniDetailScreen (Scaffold - backgroundColor: #F8F9FA)
 ├── appBar: AppBar (toolbarHeight: 56, single Back arrow >= 48x48dp, title: "Alumni Detail") [D01]
 └── body: SafeArea
      └── LayoutBuilder (Responsive: 1-column on 360dp/412dp, centered max-width 640dp on >=600dp)
           └── SingleChildScrollView (padding: EdgeInsets.all(16))
                └── Column (crossAxisAlignment: CrossAxisAlignment.stretch, spacing: 16)
                     ├── 1. ProfileIdentityCard (CMP-03 Card)
                     │    ├── Row (Avatar 72dp with Verified Badge + StatusPill "● OPEN FOR MENTORING")
                     │    ├── Text("Dang Minh Tuan", style: headlineMedium 24sp Bold)
                     │    ├── Text("Tech Lead at VNG Corporation", style: titleSmall 16sp #1B365D)
                     │    ├── MetadataRows (Campus HCMC K12, SE Cloud Computing, Location - 14sp #595959)
                     │    └── VerifiedAlumniShieldBox (ID: SE04821, bg: #EEF2FF)
                     ├── 2. PrimarySecondaryCtaGroup (US-11 & S06 Navigation)
                     │    ├── PrimaryCalendarCtaButton (height: 56dp, bg: #F27024, launches Google Calendar)
                     │    └── SecondaryDirectMessageButton (height: 48dp, bg: #EEF2FF, navigates to S06)
                     ├── 3. AboutMentorshipCard (CMP-03 Card - 14sp body text)
                     ├── 4. MentorshipTopicsCard (CMP-03 Card + Wrap of 5 Focus Area Chips >= 14sp)
                     ├── 5. CareerHistoryTimelineCard (CMP-03 Card - VNG, Shopee, FPT Software)
                     └── 6. SafetyAndBlockUserCard (US-10, D01)
                          └── OutlinedButton.icon("Block This User", height: 48dp, color: #DC2626)
                               └── onTap -> showBlockConfirmationDialog(context) [CMP-06 Dialog]
```

### 7.2. Code Mẫu Triển khai Flutter cho `S05` (`lib/features/directory/presentation/s05_alumni_detail_screen.dart`)

```dart
import 'package:flutter/material.dart';
import '../../../core/theme/app_theme.dart';

class S05AlumniDetailScreen extends StatefulWidget {
  const S05AlumniDetailScreen({super.key});

  @override
  State<S05AlumniDetailScreen> createState() => _S05AlumniDetailScreenState();
}

class _S05AlumniDetailScreenState extends State<S05AlumniDetailScreen> {
  bool _hasCalendarError = false;

  /// US-11: Mở đường dẫn đặt lịch Google Calendar của Cựu sinh viên
  Future<void> _handleBookCalendar() async {
    // Trong thực tế gọi url_launcher: launchUrl(Uri.parse(alumni.calendarUrl))
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text('Opening Google Calendar booking slot (Tomorrow, 2:00 PM)...'),
      ),
    );
  }

  /// US-10 & Quyết định D01: Hiển thị Dialog xác nhận trước khi chặn tài khoản
  Future<void> _showBlockConfirmationDialog() async {
    final confirmed = await showDialog<bool>(
      context: context,
      builder: (ctx) => AlertDialog(
        backgroundColor: AppColors.cardSurface,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(AppDimens.radiusLg),
        ),
        icon: Container(
          width: AppDimens.minTouchTarget,
          height: AppDimens.minTouchTarget,
          decoration: const BoxDecoration(
            color: AppColors.errorTint,
            shape: BoxShape.circle,
          ),
          child: const Icon(Icons.block, color: AppColors.errorCrimson, size: 24),
        ),
        title: Text(
          'Block Dang Minh Tuan?',
          style: Theme.of(context).textTheme.titleMedium,
          textAlign: TextAlign.center,
        ),
        content: Text(
          'They will no longer be able to send you direct messages or view your shared Google Calendar schedule. You can unblock them anytime in Profile > Security & Account.',
          style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                color: AppColors.textSecondary,
              ),
          textAlign: TextAlign.center,
        ),
        actionsAlignment: MainAxisAlignment.spaceEvenly,
        actions: [
          OutlinedButton(
            style: OutlinedButton.styleFrom(
              minimumSize: const Size(120, AppDimens.minTouchTarget),
              side: const BorderSide(color: AppColors.primaryNavy, width: 1.5),
              shape: RoundedRectangleBorder(
                borderRadius: BorderRadius.circular(AppDimens.radiusSm),
              ),
            ),
            onPressed: () => Navigator.of(ctx).pop(false),
            child: const Text('Cancel'),
          ),
          FilledButton(
            style: FilledButton.styleFrom(
              backgroundColor: AppColors.errorCrimson,
              minimumSize: const Size(140, AppDimens.minTouchTarget),
              shape: RoundedRectangleBorder(
                borderRadius: BorderRadius.circular(AppDimens.radiusSm),
              ),
            ),
            onPressed: () => Navigator.of(ctx).pop(true),
            child: const Text('Confirm Block'),
          ),
        ],
      ),
    );

    if (confirmed == true && mounted) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Dang Minh Tuan has been added to your Blocked Users list.'),
        ),
      );
      Navigator.of(context).maybePop();
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // Quyết định D01: Duy nhất 1 thanh AppBar, không lặp thanh "< Directory"
      appBar: AppBar(
        toolbarHeight: 56,
        backgroundColor: AppColors.cardSurface,
        leading: Semantics(
          label: 'Back to Directory',
          button: true,
          child: IconButton(
            constraints: const BoxConstraints(
              minWidth: AppDimens.minTouchTarget,
              minHeight: AppDimens.minTouchTarget,
            ),
            icon: const Icon(Icons.arrow_back, color: AppColors.textPrimary),
            onPressed: () => Navigator.of(context).maybePop(),
          ),
        ),
        title: Text(
          'Alumni Detail',
          style: Theme.of(context).textTheme.titleMedium,
        ),
      ),
      body: SafeArea(
        child: Center(
          child: ConstrainedBox(
            constraints: const BoxConstraints(maxWidth: 640),
            child: SingleChildScrollView(
              padding: const EdgeInsets.all(AppDimens.space16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.stretch,
                children: [
                  _buildIdentityCard(context),
                  const SizedBox(height: AppDimens.space16),
                  _buildCtaButtons(context),
                  if (_hasCalendarError) ...[
                    const SizedBox(height: AppDimens.space12),
                    _buildCalendarErrorBanner(context),
                  ],
                  const SizedBox(height: AppDimens.space16),
                  _buildSafetyBlockCard(context),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }

  Widget _buildIdentityCard(BuildContext context) {
    return Card(
      elevation: 0,
      color: AppColors.cardSurface,
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(AppDimens.radiusMd),
        side: const BorderSide(color: AppColors.borderDefault),
      ),
      child: Padding(
        padding: const EdgeInsets.all(AppDimens.space16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                const CircleAvatar(
                  radius: 36,
                  backgroundColor: AppColors.navyTint,
                  child: Icon(Icons.person, size: 36, color: AppColors.primaryNavy),
                ),
                // Quyết định D05: Chấm màu đi kèm nhãn chữ tường minh "OPEN FOR MENTORING"
                Container(
                  padding: const EdgeInsets.symmetric(horizontal: 12, vertical: 6),
                  decoration: BoxDecoration(
                    color: AppColors.emeraldTint,
                    borderRadius: BorderRadius.circular(999),
                  ),
                  child: const Row(
                    mainAxisSize: MainAxisSize.min,
                    children: [
                      Icon(Icons.circle, size: 8, color: AppColors.verifiedEmerald),
                      SizedBox(width: 6),
                      Text(
                        'OPEN FOR MENTORING',
                        style: TextStyle(
                          fontSize: 12,
                          fontWeight: FontWeight.w700,
                          color: AppColors.emeraldTextDark,
                        ),
                      ),
                    ],
                  ),
                ),
              ],
            ),
            const SizedBox(height: AppDimens.space12),
            Row(
              children: [
                Text('Dang Minh Tuan', style: Theme.of(context).textTheme.headlineMedium),
                const SizedBox(width: 6),
                const Icon(Icons.verified, color: AppColors.verifiedEmerald, size: 22),
              ],
            ),
            const SizedBox(height: 4),
            Text(
              'Tech Lead at VNG Corporation',
              style: Theme.of(context).textTheme.titleSmall?.copyWith(
                    color: AppColors.primaryNavy,
                  ),
            ),
            const SizedBox(height: AppDimens.space8),
            Text(
              '🎓 FPT University HCMC • Cohort K12 (2020)\n'
              '<> Software Engineering (Cloud Computing)\n'
              '📍 District 7, Ho Chi Minh City, Vietnam',
              style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                    color: AppColors.textSecondary,
                    height: 1.6,
                  ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildCtaButtons(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: [
        // Primary CTA (US-11): Chiều cao 56dp (>= 48dp touch target)
        FilledButton(
          style: FilledButton.styleFrom(
            backgroundColor: AppColors.ctaOrange,
            minimumSize: const Size.fromHeight(AppDimens.primaryCtaHeight),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(AppDimens.radiusSm),
            ),
          ),
          onPressed: _handleBookCalendar,
          child: const Row(
            children: [
              Icon(Icons.calendar_month, color: Colors.white),
              SizedBox(width: AppDimens.space12),
              Expanded(
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    Text(
                      'Xem Lịch rảnh tham khảo (Google Calendar)',
                      style: TextStyle(fontSize: 15, fontWeight: FontWeight.w700, color: Colors.white),
                    ),
                    Text(
                      'Xem khung giờ rảnh/bận để chủ động nhắn tin (US-11) (30 mins)',
                      style: TextStyle(fontSize: 13, color: Colors.white),
                    ),
                  ],
                ),
              ),
              Icon(Icons.arrow_forward, color: Colors.white),
            ],
          ),
        ),
        const SizedBox(height: AppDimens.space12),
        // Secondary Action: Mở phòng chat S06
        FilledButton.tonalIcon(
          style: FilledButton.styleFrom(
            backgroundColor: AppColors.navyTint,
            foregroundColor: AppColors.primaryNavy,
            minimumSize: const Size.fromHeight(AppDimens.minTouchTarget),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(AppDimens.radiusSm),
            ),
          ),
          onPressed: () {
            // Điều hướng sang màn hình S06 Chat Room
          },
          icon: const Icon(Icons.chat_bubble_outline, size: 20),
          label: const Text('Direct Message'),
        ),
      ],
    );
  }

  Widget _buildCalendarErrorBanner(BuildContext context) {
    return Container(
      padding: const EdgeInsets.all(AppDimens.space12),
      decoration: BoxDecoration(
        color: AppColors.errorTint,
        borderRadius: BorderRadius.circular(AppDimens.radiusSm),
        border: Border.all(color: AppColors.errorCrimson),
      ),
      child: Row(
        children: [
          const Icon(Icons.error_outline, color: AppColors.errorCrimson),
          const SizedBox(width: AppDimens.space8),
          const Expanded(
            child: Text(
              'Unable to load Google Calendar slot. Check connection.',
              style: TextStyle(fontSize: 14, color: AppColors.errorCrimson),
            ),
          ),
          TextButton(
            style: TextButton.styleFrom(
              minimumSize: const Size(88, AppDimens.minTouchTarget),
            ),
            onPressed: () => setState(() => _hasCalendarError = false),
            child: const Text('Retry'),
          ),
        ],
      ),
    );
  }

  Widget _buildSafetyBlockCard(BuildContext context) {
    return Card(
      elevation: 0,
      color: AppColors.cardSurface,
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(AppDimens.radiusMd),
        side: const BorderSide(color: AppColors.borderDefault),
      ),
      child: Padding(
        padding: const EdgeInsets.all(AppDimens.space16),
        child: Column(
          children: [
            Text(
              'Safety & Community Standards',
              style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                    color: AppColors.textSecondary,
                    fontWeight: FontWeight.w500,
                  ),
            ),
            const SizedBox(height: AppDimens.space12),
            OutlinedButton.icon(
              style: OutlinedButton.styleFrom(
                foregroundColor: AppColors.errorCrimson,
                minimumSize: const Size.fromHeight(AppDimens.minTouchTarget),
                side: const BorderSide(color: AppColors.errorCrimson, width: 1.5),
                shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(AppDimens.radiusSm),
                ),
              ),
              onPressed: _showBlockConfirmationDialog,
              icon: const Icon(Icons.block, size: 20),
              label: const Text('Block This User'),
            ),
          ],
        ),
      ),
    );
  }
}
```

---

## 5. Handoff Chi tiết Màn hình `S06`: Direct & Group Mentorship Chat Room (Do TV3 Nguyễn Phương Khải Phụ trách)

### 8.1. Sơ đồ Cây Widget (`S06` Widget Tree Hierarchy)

```text
S06ChatRoomScreen (Scaffold - backgroundColor: #F8F9FA)
 ├── appBar: AppBar (toolbarHeight: 56)
 │    ├── leading: Back IconButton (48x48dp)
 │    ├── title: Row (Avatar 40dp + Column["Dang Minh Tuan", "● Online • K12 Alumni"]) [D05]
 │    └── actions: [CalendarQuickAction (48x48dp), SafetyPopupMenuButton (48x48dp -> Block US-10)]
 └── body: SafeArea
      └── Column
           ├── 1. MentorshipContextBanner (bg: #EEF2FF, Topic + "View Calendar ↗" button) [CD-04]
           ├── 2. Expanded (MessageListView or EmptyIcebreakerView [CMP-08])
           │    ├── DateHeaderPill ("Today • Oct 22")
           │    ├── OutgoingMessageBubble (bg: #1B365D, text: #FFFFFF 14sp, status: "✓✓ Read")
           │    ├── IncomingMessageBubble (bg: #FFFFFF, border: #E5E7EB, text: #111827 14sp)
           │    ├── EmbeddedCalendarSlotCard (US-11: "Tomorrow, 2:00 PM" + 48dp CTA "Open Calendar")
           │    └── FailedMessageRetryRow (if network error -> "⚠️ Failed to send • Tap to retry" 48dp) [D06]
           ├── 3. QuickIcebreakerStrip (Horizontal ScrollView of 48dp Suggestion Chips)
           └── 4. BottomComposerOrBlockedBar
                ├── [If Active]: Row (Attach 48x48dp + Rounded TextField 48dp + Orange Send 48x48dp)
                └── [If Blocked US-10]: BlockedSafetyBanner ("You have blocked this user" + 48dp Unblock CTA)
```

### 8.2. Code Mẫu Triển khai Flutter cho `S06` (`lib/features/chat/presentation/s06_chat_room_screen.dart`)

```dart
import 'package:flutter/material.dart';
import '../../../core/theme/app_theme.dart';

class S06ChatRoomScreen extends StatefulWidget {
  const S06ChatRoomScreen({super.key});

  @override
  State<S06ChatRoomScreen> createState() => _S06ChatRoomScreenState();
}

class _S06ChatRoomScreenState extends State<S06ChatRoomScreen> {
  final TextEditingController _messageController = TextEditingController();
  bool _isBlocked = false;
  bool _lastMessageFailed = false;

  static const List<String> _icebreakers = [
    '👋 Ask about VNG OJT process',
    '📄 Request CV review',
    '📅 Confirm 2:00 PM slot',
  ];

  @override
  void dispose() {
    _messageController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        toolbarHeight: 56,
        backgroundColor: AppColors.cardSurface,
        leading: IconButton(
          constraints: const BoxConstraints(
            minWidth: AppDimens.minTouchTarget,
            minHeight: AppDimens.minTouchTarget,
          ),
          icon: const Icon(Icons.arrow_back),
          tooltip: 'Back',
          onPressed: () => Navigator.of(context).maybePop(),
        ),
        titleSpacing: 0,
        title: Row(
          children: [
            const CircleAvatar(
              radius: 20,
              backgroundColor: AppColors.navyTint,
              child: Icon(Icons.person, color: AppColors.primaryNavy),
            ),
            const SizedBox(width: AppDimens.space8),
            Expanded(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    'Dang Minh Tuan',
                    style: Theme.of(context).textTheme.titleSmall,
                    overflow: TextOverflow.ellipsis,
                  ),
                  // Quyết định D05: Kèm nhãn chữ "Online" rõ ràng, không chỉ dùng chấm màu
                  Text(
                    '● Online • K12 Alumni (VNG)',
                    style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                          fontSize: 13,
                          color: AppColors.textSecondary,
                        ),
                    overflow: TextOverflow.ellipsis,
                  ),
                ],
              ),
            ),
          ],
        ),
        actions: [
          IconButton(
            constraints: const BoxConstraints(
              minWidth: AppDimens.minTouchTarget,
              minHeight: AppDimens.minTouchTarget,
            ),
            icon: const Icon(Icons.calendar_month, color: AppColors.primaryNavy),
            tooltip: 'Open Google Calendar Booking (US-11)',
            onPressed: () {},
          ),
          PopupMenuButton<String>(
            constraints: const BoxConstraints(
              minWidth: AppDimens.minTouchTarget,
              minHeight: AppDimens.minTouchTarget,
            ),
            onSelected: (value) {
              if (value == 'block') {
                setState(() => _isBlocked = !_isBlocked);
              }
            },
            itemBuilder: (ctx) => [
              PopupMenuItem(
                value: 'block',
                height: AppDimens.minTouchTarget,
                child: Text(
                  _isBlocked ? 'Unblock User (US-10)' : '🚫 Block User (US-10)',
                  style: const TextStyle(color: AppColors.errorCrimson, fontSize: 14),
                ),
              ),
            ],
          ),
        ],
      ),
      body: SafeArea(
        child: Column(
          children: [
            _buildMentorshipContextBanner(context),
            Expanded(
              child: ListView(
                padding: const EdgeInsets.all(AppDimens.space16),
                children: [
                  _buildOutgoingBubble(
                    'Em chào anh Tuấn ạ! Em là Trung K19 SE. Em vừa xem hồ sơ anh và muốn xin lời khuyên chuẩn bị phỏng vấn OJT Backend tại VNG ạ.',
                    '10:15 AM • ✓✓ Read',
                  ),
                  const SizedBox(height: AppDimens.space12),
                  _buildIncomingBubble(
                    'Chào Trung! Anh rất sẵn lòng hỗ trợ. Em có thể đặt lịch 30 phút vào chiều mai qua Google Calendar bên dưới để anh review CV luôn nhé!',
                    '10:18 AM',
                  ),
                  const SizedBox(height: AppDimens.space12),
                  _buildEmbeddedCalendarCard(context),
                  if (_lastMessageFailed) ...[
                    const SizedBox(height: AppDimens.space12),
                    _buildFailedMessageRetry(),
                  ],
                ],
              ),
            ),
            if (!_isBlocked) _buildIcebreakerStrip(),
            _isBlocked ? _buildBlockedFooter() : _buildMessageComposer(),
          ],
        ),
      ),
    );
  }

  Widget _buildMentorshipContextBanner(BuildContext context) {
    return Container(
      padding: const EdgeInsets.symmetric(horizontal: AppDimens.space16, vertical: 8),
      decoration: const BoxDecoration(
        color: AppColors.navyTint,
        border: Border(bottom: BorderSide(color: AppColors.borderDefault)),
      ),
      child: Row(
        children: [
          const Icon(Icons.track_changes, size: 18, color: AppColors.primaryNavy),
          const SizedBox(width: AppDimens.space8),
          const Expanded(
            child: Text(
              'Topic: OJT Prep & Microservices',
              style: TextStyle(
                fontSize: 14,
                fontWeight: FontWeight.w500,
                color: AppColors.primaryNavy,
              ),
            ),
          ),
          TextButton(
            style: TextButton.styleFrom(
              minimumSize: const Size(88, AppDimens.minTouchTarget),
              foregroundColor: AppColors.primaryNavy,
            ),
            onPressed: () {},
            child: const Text('View Calendar ↗'),
          ),
        ],
      ),
    );
  }

  Widget _buildOutgoingBubble(String message, String meta) {
    return Align(
      alignment: Alignment.centerRight,
      child: Container(
        constraints: const BoxConstraints(maxWidth: 280),
        padding: const EdgeInsets.all(AppDimens.space12),
        decoration: const BoxDecoration(
          color: AppColors.primaryNavy,
          borderRadius: BorderRadius.only(
            topLeft: Radius.circular(AppDimens.radiusMd),
            topRight: Radius.circular(AppDimens.radiusMd),
            bottomLeft: Radius.circular(AppDimens.radiusMd),
            bottomRight: Radius.circular(4),
          ),
        ),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.end,
          children: [
            Text(message, style: const TextStyle(fontSize: 14, color: Colors.white, height: 1.45)),
            const SizedBox(height: 4),
            Text(meta, style: const TextStyle(fontSize: 12, color: AppColors.borderDefault)),
          ],
        ),
      ),
    );
  }

  Widget _buildIncomingBubble(String message, String time) {
    return Align(
      alignment: Alignment.centerLeft,
      child: Container(
        constraints: const BoxConstraints(maxWidth: 280),
        padding: const EdgeInsets.all(AppDimens.space12),
        decoration: BoxDecoration(
          color: AppColors.cardSurface,
          border: Border.all(color: AppColors.borderDefault),
          borderRadius: const BorderRadius.only(
            topLeft: Radius.circular(AppDimens.radiusMd),
            topRight: Radius.circular(AppDimens.radiusMd),
            bottomLeft: Radius.circular(4),
            bottomRight: Radius.circular(AppDimens.radiusMd),
          ),
        ),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              message,
              style: const TextStyle(fontSize: 14, color: AppColors.textPrimary, height: 1.45),
            ),
            const SizedBox(height: 4),
            Text(time, style: const TextStyle(fontSize: 12, color: AppColors.textSecondary)),
          ],
        ),
      ),
    );
  }

  Widget _buildEmbeddedCalendarCard(BuildContext context) {
    return Card(
      elevation: 0,
      color: AppColors.cardSurface,
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(AppDimens.radiusMd),
        side: const BorderSide(color: AppColors.ctaOrange, width: 1.5),
      ),
      child: Padding(
        padding: const EdgeInsets.all(AppDimens.space16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            const Row(
              children: [
                Icon(Icons.event_available, color: AppColors.ctaOrange),
                SizedBox(width: AppDimens.space8),
                Expanded(
                  child: Text(
                    'Google Calendar Mentorship Slot',
                    style: TextStyle(fontSize: 15, fontWeight: FontWeight.w700),
                  ),
                ),
              ],
            ),
            const SizedBox(height: AppDimens.space8),
            const Text(
              'Tomorrow, 2:00 PM – 2:30 PM (30m Google Meet)',
              style: TextStyle(fontSize: 14, color: AppColors.textSecondary),
            ),
            const SizedBox(height: AppDimens.space12),
            FilledButton(
              style: FilledButton.styleFrom(
                backgroundColor: AppColors.ctaOrange,
                minimumSize: const Size.fromHeight(AppDimens.minTouchTarget),
                shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(AppDimens.radiusSm),
                ),
              ),
              onPressed: () {},
              child: const Text('Open in Google Calendar'),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildFailedMessageRetry() {
    return InkWell(
      onTap: () => setState(() => _lastMessageFailed = false),
      child: Container(
        constraints: const BoxConstraints(minHeight: AppDimens.minTouchTarget),
        alignment: Alignment.centerRight,
        child: const Text(
          '⚠️ Failed to send due to network error • Tap to retry',
          style: TextStyle(
            fontSize: 14,
            fontWeight: FontWeight.w600,
            color: AppColors.errorCrimson,
          ),
        ),
      ),
    );
  }

  Widget _buildIcebreakerStrip() {
    return SizedBox(
      height: 56,
      child: ListView.separated(
        padding: const EdgeInsets.symmetric(horizontal: AppDimens.space16, vertical: 4),
        scrollDirection: Axis.horizontal,
        itemCount: _icebreakers.length,
        separatorBuilder: (_, __) => const SizedBox(width: AppDimens.space8),
        itemBuilder: (context, index) {
          final text = _icebreakers[index];
          return ActionChip(
            label: Text(text, style: const TextStyle(fontSize: 14, color: AppColors.primaryNavy)),
            backgroundColor: AppColors.cardSurface,
            side: const BorderSide(color: AppColors.primaryNavy),
            onPressed: () => _messageController.text = text,
          );
        },
      ),
    );
  }

  Widget _buildMessageComposer() {
    return Container(
      padding: const EdgeInsets.symmetric(horizontal: AppDimens.space12, vertical: 8),
      decoration: const BoxDecoration(
        color: AppColors.cardSurface,
        border: Border(top: BorderSide(color: AppColors.borderDefault)),
      ),
      child: Row(
        children: [
          IconButton(
            constraints: const BoxConstraints(
              minWidth: AppDimens.minTouchTarget,
              minHeight: AppDimens.minTouchTarget,
            ),
            icon: const Icon(Icons.attach_file, color: AppColors.primaryNavy),
            tooltip: 'Attach CV or Document',
            onPressed: () {},
          ),
          Expanded(
            child: ConstrainedBox(
              constraints: const BoxConstraints(minHeight: AppDimens.minTouchTarget),
              child: TextField(
                controller: _messageController,
                style: const TextStyle(fontSize: 14, color: AppColors.textPrimary),
                decoration: InputDecoration(
                  hintText: 'Write a message to Dang Minh Tuan...',
                  hintStyle: const TextStyle(fontSize: 14, color: AppColors.textSecondary),
                  filled: true,
                  fillColor: AppColors.inputFill,
                  contentPadding: const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
                  border: OutlineInputBorder(
                    borderRadius: BorderRadius.circular(24),
                    borderSide: BorderSide.none,
                  ),
                ),
              ),
            ),
          ),
          const SizedBox(width: AppDimens.space8),
          Semantics(
            label: 'Send message',
            button: true,
            child: IconButton.filled(
              style: IconButton.styleFrom(
                backgroundColor: AppColors.ctaOrange,
                minimumSize: const Size(AppDimens.minTouchTarget, AppDimens.minTouchTarget),
              ),
              icon: const Icon(Icons.send, color: Colors.white, size: 20),
              onPressed: () => _messageController.clear(),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildBlockedFooter() {
    return Container(
      padding: const EdgeInsets.all(AppDimens.space12),
      color: AppColors.errorTint,
      child: Row(
        children: [
          const Icon(Icons.block, color: AppColors.errorCrimson),
          const SizedBox(width: AppDimens.space8),
          const Expanded(
            child: Text(
              'You have blocked this user. Messaging is disabled.',
              style: TextStyle(fontSize: 14, color: AppColors.errorCrimson),
            ),
          ),
          OutlinedButton(
            style: OutlinedButton.styleFrom(
              minimumSize: const Size(96, AppDimens.minTouchTarget),
              foregroundColor: AppColors.errorCrimson,
              side: const BorderSide(color: AppColors.errorCrimson),
            ),
            onPressed: () => setState(() => _isBlocked = false),
            child: const Text('Unblock'),
          ),
        ],
      ),
    );
  }
}
```

---

## 9. Bàn giao Kỹ thuật S03, S04, S07, S08 & Quy tắc Đa Màn hình (360dp, 412dp, ≥600dp) & Khả năng Tiếp cận khi Lập trình Flutter (Responsive & A11y Rules)

1. **Cơ chế Responsive Master–Detail cho `S04/S05` và `S06` ở Breakpoint `≥ 600 dp`:**
   - Sử dụng `LayoutBuilder(builder: (context, constraints) { ... })`:
     - Nếu `constraints.maxWidth < 600`: Điều hướng bằng `Navigator.push()` từ `S04 Directory` sang `S05 Alumni Detail` và sang `S06 Chat Room`; sử dụng `NavigationBar` ở đáy.
     - Nếu `constraints.maxWidth >= 600`: Sử dụng `Row` chia 2 cột (`SizedBox(width: 340, child: S04DirectoryList())` + `const VerticalDivider(width: 1)` + `Expanded(child: S05AlumniDetailScreen())`) và đổi thanh điều hướng thành `NavigationRail` bên trái.
2. **Bảo toàn Khả năng Tiếp cận (WCAG 2.1 AA Compliance trong Flutter):**
   - Mọi `IconButton` (Back, Attach, Send, Calendar, Menu) đều phải khai báo `tooltip` hoặc bọc `Semantics(label: ..., button: true)` và giữ `constraints: BoxConstraints(minWidth: 48, minHeight: 48)`.
   - Không khóa `MediaQuery.textScalerOf(context)`: toàn bộ `Card` trên `S05` và `ChatBubble` trên `S06` không gán cứng `height`, chỉ dùng `minHeight: 48` để khi người dùng phóng to phông chữ hệ thống lên `130%` vẫn không xảy ra lỗi `RenderFlex overflowed`.


### 9.1. Bảng Đặc tả 6 Phần Bàn giao Kỹ thuật cho S03, S04, S07, S08 (Chuẩn Gate G5)

| Mã Màn | 1. Layout (360×800dp) | 2. Components (Page 05) | 3. States (Default/Load/Empty/Err) | 4. Interactions | 5. Navigation | 6. UI Constraints |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **S03**<br>*(TV2)* | Scaffold + TopAppBar 64dp + QuickComposerCard + FilterChips 48dp + SeminarCard + PostCard + BottomNav 64dp | CMP-05, CMP-02, CMP-03, CMP-01 (Outlined Nhắn tin trao đổi), CMP-04 | Default Feed, CMP-07 Skeleton, CMP-08 Empty Space, CMP-09 Network Banner | Chạm Filter Chip (✓), chạm 💬 Nhắn tin trao đổi (48dp), chạm Đăng ký tham gia Seminar | Tab 1 Root; sang S05 khi chạm tác giả; sang S06 khi chạm Nhắn tin trao đổi | Không dùng FAB nổi (D02); mọi nút/chip cao ≥ 48dp; chữ ≥ 14sp |
| **S04**<br>*(TV2)* | Scaffold + SearchHeader (52dp) + FilterChips 48dp + SwitchRow [ON] 48dp + ListView AlumniCard + BottomNav 64dp | CMP-02 (Search, Chip ✓, Switch [ON]), CMP-03, CMP-01, CMP-04 | Default Results, CMP-07 Skeleton, CMP-08 No Match + Reset Filters, CMP-09 Error | Gõ tìm kiếm (debounce 300ms), bật/tắt Chip (✓), gạt Switch [ON]/[OFF] | Tab 2 Root; chạm thẻ hoặc View Profile → S05; chạm Message → S06 | FilterChip cao 48dp có showCheckmark: true (D04, D05); chuyển GridView 2 cột khi ≥ 600dp |
| **S07**<br>*(TV4)* | Scaffold + AppBar + NetworkErrorBanner + AcademicCard (Locked) + EditBioForm + CalendarLinkCard + BottomNav | CMP-09 (Banner + Retry 48dp & Inline Error), CMP-03, CMP-02, CMP-01 | Clean Form, Inline URL Error (#DC2626), Network Error (Preserved State), Saved SnackBar | Sửa Bio/Skills, kiểm tra Regex URL calendar.google.com, bấm ↻ Retry Save (48dp) | Tab 4 Root; bấm Privacy & Security → (48dp) → S08 | Không lộ mã US-xx (D03); giữ nguyên TextEditingController khi mất mạng (D06) |
| **S08**<br>*(TV4)* | Scaffold + DetailAppBar 56dp + PrivacyCard (5 Switches) + SecurityCard (Pass & Blocked) + LogoutButton 52dp | CMP-05, CMP-02 (Switch + [ON]/[OFF] Badge), CMP-03, CMP-01 (Destructive) | Default (3 [ON], 2 [OFF]), Unblock Modal, Sync Error Rollback, Logout Dialog | Gạt 5 công tắc Privacy, mở Change Password, mở Manage & Unblock (2 accounts), bấm Log Out | Từ S07 sang; Back về S07; Log Out xóa session về S01 | 100% Switch có nhãn chữ **[ON] / [OFF]** (D05 — Critical B4); mỗi hàng cao 56dp |
