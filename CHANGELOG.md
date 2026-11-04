# Changelog

All notable changes to the **Fish Story (เล่าเรื่องปลา)** mobile application will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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

