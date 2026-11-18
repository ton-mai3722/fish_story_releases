# Changelog

All notable changes to the **Fish Story (เล่าเรื่องปลา)** mobile application will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.5.18+52] - 2026-11-18

### 🐛 Fix OTP Screen Horizontal Overflow (`otp_verification_page.dart`)
- **Responsive 6-Digit OTP Boxes & Resend Text**:
  - เปลี่ยนช่องกรอก OTP 6 หลักเป็น `Expanded` ภายใน `Row` เพื่อปรับขนาดอัตโนมัติตามความกว้างหน้าจอทุกขนาด
  - เปลี่ยน `Row` ของข้อความนับถอยหลัง/ปุ่มส่งรหัสใหม่เป็น `Wrap` เพื่อป้องกัน `RenderFlex overflowed` บนหน้าจอขนาดเล็ก

---

## [1.5.17+51] - 2026-11-18

### ✉️ Email OTP Verification & Mandatory Re-auth Account Deletion
- **ระบบยืนยันอีเมลด้วย OTP 6 หลักตอนสมัครสมาชิก**:
  - เพิ่มหน้ากรอก OTP สไตล์ Apple Liquid Glass (`OtpVerificationPage`) พร้อมช่องกรอก 6 หลัก, นับถอยหลังส่งรหัสใหม่ 60 วินาที, และรองรับ 2 ภาษา (ไทย/อังกฤษ)
  - เชื่อมต่อ `EmailOtpService` กับ Backend (`/api/auth/send-otp`, `/api/auth/verify-otp`) ก่อนสร้างบัญชี Firebase Auth
- **ปรับปรุงระบบยืนยันตัวตนก่อนลบบัญชีถาวร (`settings_page.dart`)**:
  - บังคับยืนยันรหัสผ่าน (สำหรับบัญชี Email) หรือยืนยันด้วย Google Sign-In **ทุกครั้ง** ก่อนลบข้อมูลใน Firestore, Google Drive และ Firebase Auth
  - แก้ไขบั๊ก `TextEditingController` ถูก dispose ระหว่างปิด Dialog และเพิ่ม `SingleChildScrollView` ป้องกัน Overflow

---

## [1.5.16+50] - 2026-11-18

### 🗑️ Permanent Account & Data Deletion (ลบบัญชีถาวรเฉพาะข้อมูลของผู้ใช้คนนั้น ๆ)
- **เพิ่มฟังก์ชัน `ลบบัญชีถาวร` ในหน้าตั้งค่า (`settings_page.dart`)**:
  - **Re-authentication อัตโนมัติ**: หากเซสชันเกิน 4 นาที ระบบจะให้ยืนยันตัวตน (รหัสผ่าน หรือ Google Sign-In) ก่อนลบเพื่อความปลอดภัยสูงสุด
  - **ลบข้อมูล Firestore เฉพาะผู้ใช้คนนั้น (`users/{uid}`)**: ลบข้อมูลปลา + ประวัติ `logs`, ตู้เลี้ยง + `logs`/`expenses`/`hardwares`/`maintenance_tasks` และเอกสารโปรไฟล์ผู้ใช้ทั้งหมด
  - **ลบรูปภาพและโฟลเดอร์ Google Drive เฉพาะผู้ใช้คนนั้น (`user_{uid}`)**: ลบไฟล์และโฟลเดอร์ย่อยทั้งหมดภายใน `user_{uid}` แบบ Recursive โดยไม่กระทบข้อมูลของผู้ใช้คนอื่น
  - **ลบบัญชี Firebase Auth & Session**: ลบบัญชีผู้ใช้ออกจาก Firebase Authentication และล้างแคช/เซสชันในเครื่องอย่างสมบูรณ์

---

## [1.5.15+49] - 2026-11-18

### 📐 Top-Aligned Empty State Cards (จัดตำแหน่งการ์ดสถานะว่างให้อยู่ด้านบน)
- **ย้ายการ์ดแสดงสถานะว่างขึ้นไปชิดส่วนหัวด้านบน**:
  - เปลี่ยนจาก `SliverFillRemaining` + `Center` (ที่ดันการ์ดลงมากลางหน้าจอจนเกิดพื้นที่ว่างด้านบนมากเกินไป) เป็น `SliverToBoxAdapter` ในแท็บ **ปลา (Fishes)**, **ตู้เลี้ยง (Tanks)** และ **บันทึก (Logs)**
  - การ์ดแสดงผลอยู่ชิดใต้ Header อย่างเป็นระเบียบและสมดุลเช่นเดียวกับหน้า Home

---

## [1.5.14+48] - 2026-11-18

### 🛡️ Centralized User-Friendly Error Handling (ระบบจัดการ Error กลางที่เป็นมิตรต่อผู้ใช้)
- **สร้าง Core กลาง `AppErrorHandler` และ `enum AppErrorType` (`lib/core/error/app_error_handler.dart`)**:
  - จัดกลุ่ม Error มาตรฐาน (เช่น `network`, `timeout`, `server`, `authInvalidCredentials`, `authGoogleSignIn`, `authEmailInUse`, `authWeakPassword`, `downloadFailed`, `uploadFailed`, `unknown`)
  - แปลง Exception ทางเทคนิคทั้งหมด (เช่น `PlatformException(google_sign_in, GIDClientID...)`, `FirebaseAuthException`, `SocketException`, `TimeoutException`) เป็นข้อความภาษาไทยและภาษาอังกฤษที่ผู้ใช้ทั่วไปเข้าใจง่าย
  - เก็บ Raw Error ไว้เฉพาะใน `debugPrint` สำหรับนักพัฒนา ไม่แสดงข้อความเทคนิคตรง ๆ บนหน้าจออีกต่อไป
- **ครอบคลุมทุกหน้าและ BLoC/Cubit**:
  - `login_page.dart`, `home_page.dart`, `settings_page.dart`, `fish_timeline_page.dart`, `smart_form_page.dart`, `tank_tracker_page.dart`
  - `HomeBloc`, `TimelineBloc`, `SmartFormBloc`, `TankTrackerCubit`

---

## [1.5.13+47] - 2026-11-18

### 🌐 Bilingual Splash Screen (รองรับ 2 ภาษาในหน้า Splash Screen)
- **รองรับภาษาไทย (TH 🇹🇭) และภาษาอังกฤษ (EN 🇺🇸) อัตโนมัติตั้งแต่หน้าแรก**:
  - เพิ่ม Key คำแปล `splash` ใน `th.json` และ `en.json` (`pill`, `slogan`, `subtitle`, `loading`)
  - เชื่อมต่อ `context.tr(...)` ใน `splash_page.dart` แทนข้อความภาษาไทยที่เคยฝังตายตัว

---

## [1.5.12+46] - 2026-11-17

### 🐛 Fix ListTile Material Ancestor Assertion (แก้ไข Assertion ของ ListTile ภายใน Container)
- **Wrapped `ListTile` & `SwitchListTile` in `Material` Widgets**:
  - แก้ไขข้อผิดพลาด `ListTile background color or ink splashes may be invisible` ในหน้า Quick Add Sheet (`home_page.dart`), ฟอร์มเพิ่มตู้ปลา (`tank_form_dialog.dart`), และหน้าจัดการอุปกรณ์ (`hardware_page.dart`)
  - หุ้ม `ListTile` / `SwitchListTile` ด้วย `Material(clipBehavior: Clip.antiAlias, borderRadius: ...)` เพื่อให้สีพื้นหลังและเอฟเฟกต์ Ink Splash แสดงผลอย่างถูกต้องตามมาตรฐาน Flutter

---

## [1.5.11+45] - 2026-11-17

### 🧹 Clean Settings Page (ตัดเมนูที่ยังไม่รองรับออกจากหน้าตั้งค่า)
- **เหลือเฉพาะฟีเจอร์ที่ใช้งานได้จริง 100%**:
  - **ล้างแคชข้อมูลชั่วคราว (Clear Cache)** — ล้างหน่วยความจำแคชรูปภาพ
  - **อัปเดตแอปพลิเคชัน (App Update)** — ตรวจสอบและดาวน์โหลดอัปเดตในแอป
  - **ภาษาของแอปพลิเคชัน (Language)** — สลับภาษา ไทย / อังกฤษ ทันที
  - **ข้อมูลทางกฎหมายและนโยบาย (Terms & Privacy)** — แสดงข้อกำหนดการใช้งานและนโยบายความเป็นส่วนตัว
  - **เกี่ยวกับ Fish Story (About)** — แสดงข้อมูลและเวอร์ชันจริงจาก `PackageInfo`
  - **ออกจากระบบ (Logout)** — ออกจากระบบอย่างปลอดภัย
- **นำเมนูที่ยังไม่ทำงานจริงออก**:
  - โหมดการแสดงผล (Theme Light/Dark/Auto), การแจ้งเตือน, สถานะออนไลน์ล่าสุด, Scan to Login, อุปกรณ์และการเข้าสู่ระบบ และลบบัญชีถาวร (เดิมเป็นเพียงการ Mock/จำลอง)

---

## [1.5.10+44] - 2026-11-17

### ✨ Minimalist Sliding-Pill Navigation Bar (ปรับโฉม Navbar ให้เรียบหรู สบายตา)
- **Clean Sliding Pill Indicator**:
  - เปลี่ยนจากกรอบขอบและแถบขีดด้านล่างที่ดูเยอะเกินไป เป็น **แถบแคปซูลโปร่งแสงชิ้นเดียว (`AnimatedPositioned`)** ที่เลื่อนไหลข้ามแท็บอย่างนุ่มนวลตามตำแหน่งที่กด
  - ไม่มีขอบซ้อนและไม่มีขีดกวนสายตา คงความสะอาดตาแบบ Apple Liquid Glass
- **Subtle Micro-Interactions**:
  - ยุบตัวเบา ๆ (`scale: 0.90`) เมื่อแตะ พร้อมเปลี่ยนสีและขยายไอคอนอย่างนุ่มนวล (`1.08x`)

---

## [1.5.9+43] - 2026-11-17

### 🐛 Fix Navigation Bar Text Shadow Assertion (แก้ไขข้อผิดพลาด Blur Radius ใน NavBar)
- **Resolved Shadow Interpolation Exception**:
  - แก้ไขข้อผิดพลาด `Text shadow blur radius should be non-negative` ใน `dart:ui/painting.dart`
  - ปรับการแอนิเมชันของแคปซูลและ Indicator ใน `_AnimatedBottomNavItem` โดยใช้ `Curves.easeOut` ที่เรียบเนียน และตัด `BoxShadow` ออกจากการสลับสถานะ `BoxDecoration` เพื่อป้องกันค่ารัศมีเงาติดลบระหว่างการแทรกค่าเฟรม

---

## [1.5.8+42] - 2026-11-17

### 📐 Home Empty State Cards Full Width Alignment (ปรับขนาดการ์ดสถานะว่างให้กว้างเต็มเท่าการ์ดอื่น)
- **Full Width Responsive Layout**:
  - ปรับความกว้างการ์ดแจ้งเตือนสถานะเมื่อยังไม่มีข้อมูลปลาหรือตู้เลี้ยง (`_buildEmptyState` และ `_buildEmptyTanksState`) ให้กางกว้างเต็มความกว้างหน้าจอ (`width: double.infinity`)
  - จัดระยะขอบซ้าย-ขวาให้ตรงกับ Hero Summary Card และการ์ดอื่น ๆ ในหน้า Home อย่างสมบูรณ์แบบตาม `ResponsiveLayout.horizontalPadding(context)`
  - ปรับปรุงให้รองรับทั้งในหน้า **ภาพรวม (Home)**, **ปลา (Fishes)**, **ตู้เลี้ยง (Tanks)**, และ **บันทึก (Logs)** อย่างเป็นระเบียบสวยงาม

---

## [1.5.7+41] - 2026-11-17

### ✨ Interactive Animated Bottom Navigation Bar (แอนิเมชันเนวิเกชันบาร์ตอนกด)
- **Haptic & Spring Press Animation**:
  - เพิ่มแอนิเมชันตอบสนองการกดแบบสปริง (`AnimatedScale` ยุบลง 0.86 เมื่อแตะ และดีดตัวกลับด้วย `Curves.elasticOut`)
  - รองรับการสั่นตอบสนองแบบเบา (`HapticFeedback.selectionClick()`) ทันทีที่แตะ
- **Active Tab Selection & Pop Bounce**:
  - เมื่อเปลี่ยนแท็บ ไอคอนจะเล่นแอนิเมชันเด้ง Pop (`Scale: 1.30` -> `1.0` และลอยตัว `Offset: -3.5px` -> `0.0`) ด้วย `Curves.elasticOut` & `Curves.bounceOut`
  - กรอบพื้นหลังแคปซูลเรืองแสงนุ่มนวลตามสีประจำแท็บ (`BoxShadow` + สีไฮไลท์โปร่งแสง)
  - แถบ Indicator ด้านล่างตัวหนังสือขยายตัวและเปล่งประกายสีประจำหมวดหมู่อย่างนุ่มนวล
- **Apple Frosted Acrylic Glass Container**:
  - หุ้มขอบแคปซูลด้วย `ClipRRect(32)` และเบลอพื้นหลังด้วย `BackdropFilter(sigma: 16)` สไตล์ Liquid Glass อย่างสมบูรณ์แบบ

---

## [1.5.6+40] - 2026-11-17

### ⚖️ Legal Privacy & Terms Neutralization (ปรับปรุงข้อกำหนดและนโยบายความเป็นส่วนตัวตามมาตรฐานสากล)
- **ปรับปรุงข้อความข้อกำหนดและเงื่อนไข (Terms & Conditions) และนโยบายความเป็นส่วนตัว (Privacy Policy)**:
  - สอดคล้องตามมาตรฐานกฎหมายคุ้มครองข้อมูลส่วนบุคคล (PDPA / GDPR / App Store & Google Play Policy)
  - ปรับภาษาให้เป็นกลางและเป็นมืออาชีพ ไม่ระบุชื่อเทคโนโลยีหรือโครงสร้างพื้นฐานจัดเก็บข้อมูลเฉพาะเจาะจง (ตัดคำว่า Cloud Firestore และ Google Drive ออกจากข้อความสำหรับผู้ใช้งาน)
  - แบ่งหมวดหมู่ข้อตกลงชัดเจน 4 หัวข้อ:
    1. **การให้บริการและวัตถุประสงค์ (Service & Purpose)**: เครื่องมือบันทึกและจัดการข้อมูลการเลี้ยงสัตว์น้ำสำหรับบริหารจัดการส่วนบุคคล
    2. **การคุ้มครองข้อมูลและความเป็นส่วนตัว (Data Privacy & Protection)**: ข้อมูลได้รับการปกป้องตามมาตรฐานความปลอดภัยที่เหมาะสม ไม่เผยแพร่ ส่งต่อ หรือจำหน่ายแก่บุคคลภายนอกโดยไม่ได้รับความยินยอม
    3. **สิทธิ์และการเป็นเจ้าของข้อมูล (Data Ownership & Rights)**: ผู้ใช้เป็นเจ้าของข้อมูล 100% มีสิทธิ์เข้าถึง แก้ไข ปรับปรุง หรือขอลบข้อมูลออกจากระบบได้ตลอดเวลา
    4. **ข้อกำหนดการใช้งานและความปลอดภัย (Terms of Use & Safety)**: ใช้งานอย่างถูกต้องตามกฎหมาย รักษาความปลอดภัยของบัญชีผู้ใช้
- **UI Consistency**: ปรับเปลี่ยนไอคอนในข้อ 4 เป็นไอคอนตราความยุติธรรมและกฎหมาย (`Icons.gavel_rounded` ใน Mobile App และ `Scale` ใน Web CMS)

---

## [1.5.5+39] - 2026-11-13

### 🎨 Modern Settings Redesign (ปรับโฉมหน้าการตั้งค่าใหม่ตามแบบอ้างอิง)
- **โหมดการแสดงผล (Theme Selection)**:
  - การ์ดเลือกธีม 3 รูปแบบ: **สว่าง (Light)**, **มืด (Dark)**, และ **ตามระบบ (Auto)**
  - ดีไซน์การ์ดโค้งมนพร้อมไอคอนและตราสัญลักษณ์เช็คถูกเมื่อเลือกใช้งาน
- **การแจ้งเตือนและการใช้งาน (Notifications & Usage)**:
  - เมนูตั้งค่าการแจ้งเตือนเสียงและข้อความ
  - ปุ่มล้างแคชข้อมูลชั่วคราว (Clear Cache) เพื่อเพิ่มพื้นที่จัดเก็บในเครื่อง
  - เมนูอัปเดตแอปพลิเคชันพร้อมแสดงสถานะ `✓ ล่าสุดแล้ว` หรือปุ่มอัปเดตทันที
- **ความเป็นส่วนตัวและความปลอดภัย (Privacy & Security)**:
  - สวิตช์เปิด/ปิดแสดงสถานะการใช้งานล่าสุด
  - ลิงก์เปิดดูข้อมูลทางกฎหมายและนโยบายความเป็นส่วนตัว (Terms & Privacy Sheet)
  - ตัวสลับภาษาแอปพลิเคชัน (🇹🇭 ภาษาไทย / 🇺🇸 English)
- **จัดการบัญชี (Account Management)**:
  - ข้อมูลบัญชีผู้ใช้ (User Profile & Display Name)
  - อุปกรณ์และการเข้าสู่ระบบ (Devices & Sessions)
  - ออกจากระบบ (Logout) และเมนูลบบัญชีถาวร (Danger Zone)
- **Brand Footer**:
  - แสดงไอคอนแอปและเครดิต `Version 1.5.5 • Crafted with ❤️ for Aquarists`

---

## [1.5.4+38] - 2026-11-13

### 📜 Terms & Conditions & Privacy Policy Gate (ข้อกำหนดและเงื่อนไขการใช้งาน)
- **First-Time Login & Post-Registration Consent**:
  - เพิ่มหน้าต่างยืนยันข้อกำหนดและเงื่อนไขการใช้งาน (Terms & Conditions) และนโยบายความเป็นส่วนตัว (Privacy Policy) สไตล์ Apple Liquid Glass
  - ตรวจสอบสถานะการยอมรับข้อกำหนดอัตโนมัติเมื่อ:
    - ผู้ใช้สมัครสมาชิกใหม่เสร็จสิ้น (Register)
    - ผู้ใช้เข้าสู่ระบบครั้งแรก (First-Time Login) ด้วย Email หรือ Google
  - บันทึกการยอมรับข้อกำหนด `acceptedTerms: true`, `termsAcceptedAt`, และ `termsVersion` ลง Firestore
  - ป้องกันการเข้าถึงข้อมูลโดยไม่ผ่านการยอมรับข้อกำหนด (หากยกเลิกระบบจะ Sign-out กลับสู่หน้า Login อย่างปลอดภัย)

---

## [1.5.3+37] - 2026-11-13

### 🔐 Register & Google Sign-In (ระบบสมัครสมาชิกและเข้าสู่ระบบด้วย Google)
- **Register Mode & User Creation**:
  - เพิ่มแถบ Segmented Tab สลับระหว่าง "เข้าสู่ระบบ" และ "สมัครสมาชิก"
  - รองรับการสร้างบัญชีผู้ใช้ด้วยอีเมลและรหัสผ่าน พร้อมระบุ **ชื่อผู้ใช้ (Display Name)**
  - บันทึกโปรไฟล์ลง Firestore `users/{uid}` อัตโนมัติ พร้อมตั้งค่า Role และ Timestamp
- **Google Sign-in**:
  - รองรับการเข้าสู่ระบบผ่าน Google Account ด้วย Firebase Authentication
  - ออกแบบปุ่ม Liquid Frosted White Glass พร้อมโลโก้ Google 4 สี
  - ซิงค์ Display Name และโปรไฟล์อัตโนมัติ

### 👤 Display Name Everywhere
- แสดงชื่อจริงของผู้ใช้ (Display Name) แทนรหัส UID ทั้งในหน้าหลัก (Home Page), การตั้งค่า (Settings Page) และ Web CMS Header

### 📁 Google Drive Clean Hierarchy Architecture
- ปรับโครงสร้างจัดเก็บรูปภาพใน Google Drive แยกตาม User ID และหมวดหมู่อย่างเป็นระเบียบ:
  `Root (1Qk6tdv8ws5yHBqxdVXPB2KyebmjC5cAX) -> user_{uid} -> fishes / tanks / others`
- รูปโปรไฟล์ปลาและไทม์ไลน์บันทึกปลาเก็บใน `fishes/fish_{id}`
- รูปตู้ปลาและบันทึกการดูแลเก็บใน `tanks/tank_{id}`
- รูปสลิปค่าใช้จ่ายและอุปกรณ์เก็บใน `others/`

---

## [1.5.2+36] - 2026-11-12

### 🎨 UI & Clean-up
- **Generic Login Input Placeholders**:
  - ลบตัวอย่างอีเมลส่วนตัวออกจากช่องกรอกอีเมลทั้งภาษาไทยและอังกฤษ
  - ปรับเป็นข้อความแนะนำทั่วไป "กรอกอีเมลของคุณ" / "Enter your email"

---

## [1.5.1+35] - 2026-11-12

### 🔐 Firebase Authentication & Multi-Tenant Isolation
- **Firebase Authentication Integration**:
  - เปลี่ยนระบบ Login จาก Username เป็น Firebase Authentication (Email & Password)
  - รองรับการ Sign-in และจัดการ Session อย่างปลอดภัย
- **User-Isolated Database Architecture**:
  - ปรับโครงสร้าง Firestore ให้แยกข้อมูลผู้ใช้แต่ละรายอย่างเป็นสัดส่วนภายใต้ Collection `users/{userId}/...`
  - ย้ายข้อมูลตู้ปลา, ปลา, ค่าใช้จ่าย และประวัติการบันทึกเข้าสู่ User Collection ใหม่อย่างสมบูรณ์
- **Codebase Clean-Up**:
  - ลบ Hardcoded bypass เก่าทั้งหมดออก 100%

---

## [1.5.0+34] - 2026-11-12

### 🚀 Major Milestone Release (Apple Liquid Glass & Modern Aquarium Ecosystem)
- **Apple Liquid Glass Design Language & Bento Cards (ยกเครื่องดีไซน์ใหม่หมด)**:
  - ดีไซน์การ์ดและหน้าจอใหม่ในสไตล์ Apple Liquid Glass ผสาน Bento Grid บนทุกหน้าหลัก (หน้า Home, หน้าตู้เลี้ยง, หน้าอุปกรณ์ & ค่าไฟ, และหน้าตารางดูแลตู้)
  - เพิ่ม Photo Carousel บนการ์ดตู้ปลา เลื่อนดูรูปพัฒนาการของตู้จากไทม์ไลน์ได้ทันที
- **Direct Camera & Photo Upload (ระบบเลือกและถ่ายรูปจากอุปกรณ์โดยตรง)**:
  - ถอดการใช้งาน Google Drive File ID / ลิงก์ภายนอกออกทั้งหมด รองรับการถ่ายภาพจากกล้องหรือเลือกจาก Photo Gallery ภายในเครื่องได้โดยตรง ทั้งบน Mobile App และ Web CMS
- **Modern Pull-To-Refresh**:
  - เปลี่ยนระบบรีเฟรชข้อมูลเป็น Gesture Pull-To-Refresh ลากดึงลงเพื่อโหลดข้อมูลใหม่ในทุกหน้า
- **Smart Hardware & Utility Dashboard**:
  - คำนวณค่าไฟฟ้า ค่าน้ำประปา และติดตามอายุการใช้งานอุปกรณ์ในตู้ปลาแบบเรียลไทม์
- **Safe Area & Modal Polish**:
  - ล็อคขอบเขต Modal Bottom Sheet ให้อยู่ใต้ Status Bar และ Dynamic Island พร้อม Grip Handle สไตล์ iOS Native

---

## [1.4.27+33] - 2026-11-12

### Added & Enhanced
- **UI & Bug Fixes: Safe Area & Status Bar Overlap (แก้ปัญหา Modal เกยขอบ Status Bar / Dynamic Island)**:
  - เพิ่ม `useSafeArea: true` และ iOS Grab Handle บน Modal Bottom Sheet ของหน้า "อุปกรณ์ & ค่าไฟ" (Hardware Page) และหน้า "ตารางดูแลตู้" (Maintenance Page) ป้องกัน Modal ดันขึ้นไปทับแถบสถานะด้านบน
- **Feature: Pull-To-Refresh ทุกหน้า**:
  - เปลี่ยนจากการกดปุ่มรีเฟรชบนหัว AppBar เป็นการเลื่อนดึงเพื่อรีเฟรช (Pull-To-Refresh) ในทุกหน้า (ภาพรวม, ตู้เลี้ยง, จัดการตู้, ไทม์ไลน์ตู้ปลา, รายการปลา, ไทม์ไลน์ปลา) พร้อมนำปุ่มรีเฟรชและอวตารเดิมออกเพื่อความมินิมอล
- **UX: ถ่ายภาพและเลือกรูปจากเครื่องโดยตรง (Direct Camera & Gallery Photo Picker)**:
  - ตัดช่องกรอก Google Drive File ID หรือ External URL ออกทั้งหมด ทั้งในแอปและ CMS ให้เลือกถ่ายภาพหรือเลือกไฟล์รูปจากเครื่องได้โดยตรง
- **Design: Modern Liquid Glass & Compact Layout**:
  - ปรับดีไซน์หน้าตารางดูแลตู้ และหน้าอุปกรณ์ & ค่าไฟ เป็นสไตล์ Apple Liquid Glass และ Bento UI
  - ปรับลดช่องว่างระหว่างการ์ดตู้ปลาบนหน้า Home ให้กระชับ สวยงามพอดีกับหน้าจอ

---

## [1.4.26+32] - 2026-11-10

### Added & Enhanced
- **Feature: Apple Liquid Glass Tank Card & Timeline Photo Slideshow (การ์ดตู้ปลา Liquid Glass & สไลด์โชว์ภาพจากไทม์ไลน์)**:
  - ดีไซน์การ์ดตู้ปลาใหม่บนหน้า Home และ Tab Tanks ให้ทันสมัย สวยงาม นุ่มนวล สไตล์ Apple Liquid Glass พร้อม Bento Metrics แสดงปริมาตรน้ำ, ขนาดตู้ และจำนวนวันเปิดตู้
  - ดึงภาพล่าสุดและรูปทั้งหมดจากบันทึกไทม์ไลน์มาทำเป็น Carousel Slideshow ปาดเลื่อนดูภาพพัฒนาการของตู้ปลาได้โดยตรงจากการ์ด
  - แก้ปัญหาภาพกระพริบตอนเลื่อนหน้าจอและเลื่อนรูป โดยปรับระบบ Image Pre-caching ดึงภาพเข้า GPU/RAM ล่วงหน้า, แสดงภาพทันทีโดยไม่เฟดขาว (`fadeInDuration: Duration.zero`), ตัด Spinner ใน Placeholder ออก, และเปิด Keep-Alive สำหรับสไลด์
- **Feature: Direct Add New Tank Action (เพิ่มตู้ปลาใหม่ได้ทันที)**:
  - ปุ่ม "+ Add New Tank" บน Tab Tanks กดแล้วเปิดฟอร์มเพิ่มตู้ปลาใหม่ได้ทันที ไม่ต้อง Push ข้ามหน้า
- **UI & Bug Fixes: Safe Area & Bilingual Tank Modal (ปรับปรุงหน้าต่างเพิ่มตู้ปลา)**:
  - ปรับระยะ Safe Area ไม่ให้ Bottom Sheet เกยแถบ Status Bar ด้านบน
  - รองรับระบบ 2 ภาษา (ไทย / อังกฤษ) ครบถ้วนทุกช่องและข้อความ
  - ซ่อนส่วนระบุการยุบตู้ (Decommission) ออกอัตโนมัติเมื่อเป็นการสร้างตู้ใหม่
- **UI Revamp: Hardware, Electricity & Maintenance Dashboard (ยกเครื่องหน้าอุปกรณ์ & ตารางดูแลตู้)**:
  - ปรับโฉมหน้าอุปกรณ์และค่าไฟ แก้ไขปัญหา RenderFlex overflow บนหน้าจอขนาดต่างๆ
  - ปรับปรุงการแสดงผลรายการกิจกรรมและตารางการดูแลตู้ปลา

---

## [1.4.25+31] - 2026-11-09

### Added & Enhanced
- **Feature: Strict Lock for Decommissioned Tanks (ล็อคการแก้ไขตู้ที่ยุบแล้วอย่างสมบูรณ์)**:
  - If a tank is marked as decommissioned ("ยุบตู้แล้ว ⚠️"), all editing options are strictly locked and hidden across the app:
    - Hides the "แก้ไขตู้" (Edit Tank) action from the Tank Timeline AppBar.
    - Hides the edit pencil icon from the Home screen tank card and Tanks management list.
    - Hides the "บันทึกการเปลี่ยนแปลง" (Floating Action Button) from the timeline, preventing new change logs from being submitted.
    - Hides edit and delete actions on timeline logs for decommissioned tanks.
    - Added security guard inside `TankFormDialog` preventing decommissioned tanks from being opened or edited.

---

## [1.4.24+30] - 2026-11-09

### Added & Enhanced
- **Feature: Direct Tank Edit & Decommissioning Access (ปุ่มแก้ไขตู้ & เมนูยุบตู้โดยตรง)**:
  - Added direct **"แก้ไขตู้"** (Edit Tank) action button inside the AppBar of **Tank Timeline (`TankTimelinePage`)**.
  - Added edit icon button on each **Tank Card (`_GlassTankCard`)** on the Home screen and Tanks tab.
  - Made the full Tank Form Bottom Sheet reusable via `TankFormDialog`, allowing users to toggle **"ยุบตู้เลี้ยงแล้ว (ไม่ได้มีตู้นี้แล้ว) ⚠️"**, select the decommission date, and enter the reason from anywhere in the app.
  - Displays high-visibility decommission badges (`⚠️ ยุบตู้แล้ว`) and detailed decommission reason banners across Home cards, Tanks lists, and Timeline hero headers.

---

## [1.4.23+29] - 2026-11-09

### Added & Enhanced
- **Feature: Pet Rehome Tracking & History Locking (ระบบย้ายบ้านสัตว์เลี้ยง & ล็อคการแก้ไขประวัติ)**:
  - Added "ย้ายบ้าน 🏡" (Rehomed) status for pets and livestock alongside existing alive and deceased options.
  - Automatically locks the profile and timeline from further edits and hides the "บันทึกเพิ่มเติม" button once a pet is rehomed or deceased.
  - Auto-heals and synchronizes existing timeline entries with the fish profile across Mobile App, Web CMS, and Backend API.
  - Excluded rehomed pets from the active alive count on the Dashboard.

---

## [1.4.17+23] - 2026-11-09

### Added & Enhanced
- **Feature: Tank Decommission & Reason Tracking (ระบบยุบตู้เลี้ยง & ระบุเหตุผล)**:
  - Added ability to mark tanks as decommissioned ("ยุบตู้แล้ว / ไม่ได้มีตู้นี้แล้ว") along with decommission date and detailed reason (e.g. ขายตู้, รั่วซึม, ย้ายปลาไปตู้ใหม่, เลิกเลี้ยงชั่วคราว).
  - Updated days running calculation to freeze at the decommission date instead of continuing to count.
  - Enhanced Tank Cards and Tank Timeline with high-visibility decommission warning badges, reason details box, and updated status indicators across App, CMS, and Backend API.

---

## [1.4.16+22] - 2026-11-04

### Added & Redesigned
- **Feature: In-App Downloader & Package Installer (ดาวน์โหลดและติดตั้งอัปเดตในแอป)**:
  - Added native In-App APK downloader with real-time download progress bar, file size indicators, and automatic prompt to launch the native Android Package Installer (`open_filex` & `REQUEST_INSTALL_PACKAGES`).
- **UI: True Floating Capsule Bottom Navigation Bar**:
  - Removed opaque white background block; content now smoothly glides underneath the floating capsule menu across all tabs.
- **UI: Complete Redesign with Shimmer Skeleton (Apple Liquid Glass UI & Shimmer Wave)**:
  - **Tank Timeline Page (`TankTimelinePage`)**: Liquid Mesh ambient background, frosted showcase hero card, emerald Day 0 setup milestone node, category gradient badges (`💧 -30%`), and animated `_ShimmerBox` loading skeleton.
  - **Project Fishes Page (`ProjectFishesPage`)**: Full visual overhaul replacing old heavy banner with floating frosted glass app bar, real-time live search, Survival Rate progress bar, filter pills (`ทั้งหมด`, `มีชีวิต`, `สูญเสีย`), and upgraded fish identity cards.
  - **Tanks Management Page (`TanksManagementPage`)**: Replaced `CircularProgressIndicator` with full multi-card Shimmer Wave skeleton matching the layout, plus frosted header and gradient FAB.
- **UI: Streamlined Header**:
  - Removed red Logout button from Home header for a cleaner, minimalist overview.
  - Fixed localization key `btn_add_fish` in Thai and English translations.

---

## [1.4.15+21] - 2026-11-04

### Fixed & Cleaned
- **Bug Fix: In-App Version Checker Platform Registration**:
  - Registered `GeneratedPluginRegistrant.registerWith(flutterEngine)` explicitly in `MainActivity.kt` ensuring native Android plugin channels (including `package_info_plus`) are always bound.
  - Added defensive `try-catch` with automatic fallback versioning (`v1.4.15+21`) in `AppUpdateService` to prevent `MissingPluginException` or raw error cards from displaying.
- **UI: Streamlined Home Header**:
  - Removed redundant language toggle from the Home header; language selection is now conveniently managed exclusively in the **Settings** page.

---

## [1.4.14+20] - 2026-11-04

### Added & Redesigned
- **UI: Floating Apple Liquid Glass Bottom Navigation Bar (ระบบ Navbar 5 แท็บหลักสไตล์ Liquid Glass)**:
  - Added modern floating acrylic Bottom Navigation Bar matching the Web CMS design:
    - 🧭 **ภาพรวม (Overview)**: Summary metrics card, Quick Action Grid (Add Fish, Add Tank, Logs, Equipment), and recent Fish Projects & Tanks previews.
    - 🐟 **ปลา (Fishes)**: Full grid of fish project categories with count metrics and FAB for new fish entries.
    - 🫧 **ตู้เลี้ยง (Tanks)**: Full list of aquariums & ponds with real-time stats and FAB for adding/managing tanks.
    - 📝 **บันทึก (Logs & Timelines)**: Segmented view for Tanks & Fish diaries with 1-tap direct links to comprehensive visual timelines.
    - ⚙️ **อุปกรณ์ (Equipment)**: Full hardware tracker, parameter logs, maintenance tasks, and budget analytics seamlessly integrated.
  - **Haptic Feedback & Micro-interactions**: Smooth selection feedback on tab switching with responsive active indicator bubbles.
  - **Contextual FAB**: Dynamic Floating Action Buttons tailored to active views (Quick Add Sheet on Overview, Add Fish on Fishes tab, Add Tank on Tanks tab).

---

## [1.4.13+19] - 2026-11-04

### Redesigned & Polished
- **UI: Complete Tank Card Redesign (`_GlassTankCard`)**:
  - **Integrated Hero Photo Header**: Expanded photo height to 185px with cinematic dual gradient vignettes.
  - **Frosted Acrylic Badges**: Replaced dull dark boxes with luxury acrylic glass pills (`🌊 Marine` category badge, `🟢 ACTIVE` live status indicator, and `⏱ Running Days` counter).
  - **Cohesive 3-Column Specs Grid**: Replaced cluttered, mismatched colored chips with a clean automotive/smart-home styled specs bar featuring **Water Volume**, **Formatted Dimensions (`24×18×16`)**, and **Cycling Days**.
  - **Clean Purpose Subtitle**: Formatted tank purpose note as an elegant subtitle line with subtle flag icon instead of disconnected colored pills.

---

## [1.4.12+18] - 2026-11-04

### Redesigned & Polished
- **UI: Premium Project Card Redesign (ปรับดีไซน์การ์ดหมวดหมู่ปลาใหม่ทั้งหมด)**:
  - **Ambient Radial Glow**: Added soft ambient radial light tinted with each category's distinct color in the top-right corner.
  - **Decorative Watermark**: Incorporated large, translucent silhouette watermark icon in the bottom-right for elegant depth.
  - **Refined Typography Hierarchy**: Separated Thai primary titles (e.g. **ปลากินเนื้อ**) and crisp uppercase English category badges (`CARNIVORE`) to eliminate awkward parentheses wrapping.
  - **Hero Icon & Specular Borders**: Enhanced 44x44 squircle icons with dual specular borders and glowing drop shadows.
  - **Action Chevron Chip**: Added circular glass forward arrow chip with category border accents.
  - **Proportional Grid Aspect Ratio**: Optimized grid child aspect ratio to `0.95` for tight, balanced proportions on all mobile screen sizes.

---

## [1.4.11+17] - 2026-11-04

### Added
- **Feature: In-App Version Update & Settings Page (หน้าตั้งค่าและระบบอัปเดตเวอร์ชันในแอป)**:
  - Added new **Settings Page** accessed via the gear icon on the Home page top header.
  - **In-App Version Checker (`AppUpdateService`)**: Checks real-time releases against GitHub Releases API (`ton-mai3722/fish_story_releases/releases/latest`).
  - **1-Click Direct Download & Update**: Users can tap "ดาวน์โหลดและอัปเดตทันที" to download the latest `.apk` and trigger installation directly from their device.
  - Displays dynamic Release Notes, file size, and status badges.
  - Users on the latest version can re-download APK or check for updates anytime.
  - **Language & Preferences**: Switch between Thai 🇹🇭 and English 🇺🇸 with instant persistence.
  - **Account & Community Links**: Quick links to official releases repository and user session logout.

---

## [1.4.10+16] - 2026-11-04

### Fixed & Performance Overhaul
- **Performance: 120 FPS Buttery-Smooth Scroll**:
  - Removed 60-120Hz continuous ambient repaint loop (`AnimationController` + full-screen `BackdropFilter` sigma 50).
  - Replaced CPU/GPU-heavy backdrop filters with high-performance static radial mesh gradient and hardware-accelerated translucent layers.
  - Eliminated nested `BackdropFilter` readbacks from cards (`_ProjectCard`, `_GlassTankCard`, header icons, segmented bar) to completely solve scrolling lag and stutter.
- **Layout & Typography: Font Scaling Clamped**:
  - Clamped global Android system `textScaler` (`minScaleFactor: 0.85, maxScaleFactor: 1.0`) to prevent oversized system font settings from blowing up UI components.
  - Reduced oversized font sizes in Summary Card, Header, and Floating Action Button.
  - Shortened labels to fit compact mobile screens without text clipping.
- **UI: Mobile Modal Overlay Offset**:
  - Fixed Add Change Log modal bottom sheet positioning so action buttons are never obscured by bottom system navigation bar.

### Added
- **UI: Apple Liquid Glass Redesign**:
  - Implemented iOS frosted acrylic glassmorphism across **Splash Screen**, **Login Page**, and **Home Page**.
  - Dynamic ambient mesh background with Gaussian blur and subtle floating light particles.
  - Specular borders with high-refraction glassmorphic cards.
- **Brand: 3D Transparent App Icon**:
  - High-resolution 3D translucent fish skeleton icon with alpha channel transparency for Android adaptive icons and iOS app icons.
- **i18n: Runtime Bilingual Localization (TH / EN)**:
  - JSON-based translation dictionaries (`assets/lang/th.json` & `assets/lang/en.json`).
  - Minimalist glassmorphic `TH / EN` language toggle button on Login and Home screens.
  - Persistent user language preference powered by `SharedPreferences`.
- **Feature: Tanks & Ponds Integration on Home Page**:
  - Glass Segmented Bar switching seamlessly between **Fish Projects** (โปรเจกต์ปลา) and **Tanks & Habitats** (ตู้และบ่อเลี้ยง).
  - Quick-action buttons to jump directly to tank management and log creation.

### Fixed
- **Media: Tank Photo Rendering Overhaul**:
  - Added support for Base64 Data URI decoding (`data:image/jpeg;base64,...`) alongside Google Drive CDN images to fix broken thumbnails.
- **Stability: ProviderNotFoundException**:
  - Resolved missing `TankTrackerCubit` dependency error when launching `TanksManagementPage` directly from Home screen.
- **Android: Gradle 8 & Java 17 Compatibility**:
  - Configured optimized build properties for release APK generation.

