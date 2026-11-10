# Changelog

All notable changes to the **Fish Story (เล่าเรื่องปลา)** mobile application will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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

