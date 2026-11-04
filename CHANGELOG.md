# Changelog

All notable changes to the **Fish Story (เล่าเรื่องปลา)** mobile application will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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

