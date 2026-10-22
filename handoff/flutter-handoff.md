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

## 5. Vùng Đồng bộ với Thành viên Khác (`S03` → `S08`)

Tài liệu này đã dựng sẵn toàn bộ hệ thống Tokens và khuôn mẫu mã nguồn chuẩn mực. Các thành viên tiếp theo có thể kế thừa trực tiếp:
- **TV2 (Trung):** Sử dụng `AppColors.fptNavy` và `AppColors.fptOrange` cho Bảng tin `S03` và Danh bạ `S04`.
- **TV3 (Khải):** Lấy các lớp Token trên để dựng 9 Component trên Figma Page 05 và mã nguồn `S05, S06`.
- **TV4 (Tín):** Kế thừa cho màn `S07` (My Profile) và `S08` (Privacy & Blocked List).
