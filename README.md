# Focusly: Screen Time & Digital Wellbeing

> Repository chính thức chứa toàn bộ mã nguồn của ứng dụng **Focusly: Screen Time** cho cả 2 nền tảng **iOS** (Swift / Xcode) và **Android** (Kotlin / Android Studio).

---

## 📂 Cấu trúc Repository (Monorepo)

```
focusly-screen-time/
├── README.md                            # Tài liệu tổng quan & kiến trúc
├── .gitignore                           # Git ignore dùng chung
│
├── focusly_ios/                         # DỰ ÁN IOS (SWIFT / XCODE)
│   ├── Focusly.xcodeproj/               # Project Xcode
│   ├── Focusly/
│   │   ├── App/FocuslyApp.swift         # SwiftUI App Entry
│   │   ├── Core/
│   │   │   ├── Constants/AppConstants.swift
│   │   │   ├── ScreenTime/              # ScreenTimeManager, ShieldManager, DeviceActivityManager
│   │   │   └── Storage/UserPreferences.swift (App Group Storage)
│   │   ├── Models/                      # AppLimit, BlockSchedule, FocusSession, DailyStats
│   │   ├── ViewModels/                  # Dashboard, AppLimits, FocusTimer, Statistics
│   │   ├── Views/                       # Dashboard, Blocking, Focus Timer, Insights, Settings
│   │   ├── Focusly.entitlements         # FamilyControls & App Group
│   │   └── Info.plist
│   ├── FocuslyDeviceActivityMonitor/    # Extension chạy ngầm khi vượt ngưỡng giới hạn
│   ├── FocuslyShieldConfiguration/      # Extension tuỳ biến màn hình khoá Shield
│   ├── FocuslyShieldAction/             # Extension bắt sự kiện nút bấm trên Shield
│   └── FocuslyDeviceActivityReport/     # Extension hiển thị báo cáo Screen Time
│
└── focusly_android/                     # DỰ ÁN ANDROID (KOTLIN / ANDROID STUDIO)
    ├── build.gradle.kts / settings.gradle.kts / gradle.properties
    ├── gradlew / gradlew.bat
    └── app/
        ├── build.gradle.kts             # Jetpack Compose, Room KSP, WorkManager, Coroutines
        └── src/main/
            ├── AndroidManifest.xml      # Khai báo Permissions, Service, Activity, Receiver
            ├── res/xml/accessibility_service_config.xml
            └── java/com/focusly/app/
                ├── FocuslyApplication.kt
                ├── data/local/ (Room Database, DAOs, Entities)
                ├── data/repository/ (UsageStats, AppLimits, FocusSession)
                ├── domain/manager/ (UsageStatsHelper, PermissionHelper)
                ├── service/ (FocuslyAccessibilityService, FocusTimerService)
                ├── worker/ (UsageLimitWorker)
                └── ui/
                    ├── MainActivity.kt (BottomNavigation & NavHost)
                    ├── overlay/BlockOverlayActivity.kt (Màn hình khoá)
                    ├── permissions/PermissionGuideScreen.kt (Cấp quyền 3 bước)
                    ├── dashboard/, blocking/, focus/, statistics/
                    └── theme/ (Colors, Type, Theme Material 3)
```

---

## 🎯 6 Nhóm tính năng cốt lõi

| Tính năng | iOS (Swift / Xcode) | Android (Kotlin / Android Studio) |
|---|---|---|
| **1. Screen Time Tracking** | **Apple Screen Time API** (`FamilyControls` & `DeviceActivityReportExtension`). Dữ liệu được bảo mật sandbox theo chuẩn Apple. | **`UsageStatsManager`** (`PACKAGE_USAGE_STATS`). Thống kê chính xác thời gian dùng app, số lần mở máy trong ngày. |
| **2. App Blocking** | **`ManagedSettingsStore`**: Tự động gán `shield.applications = applicationTokens`. Giao diện màn hình khoá tuỳ biến qua `ShieldConfigurationExtension`. | **`AccessibilityService`** bắt sự kiện `TYPE_WINDOW_STATE_CHANGED` tức thì. Hiển thị `BlockOverlayActivity` đè lên ứng dụng bị chặn. |
| **3. App Usage Limits** | `DeviceActivityEvent` với ngưỡng threshold phút theo ngày. Tự động trigger qua `DeviceActivityMonitorExtension`. | Lưu trong Room Database, `UsageLimitWorker` (WorkManager) định kỳ kiểm tra và cảnh báo. |
| **4. Focus Mode / Pomodoro** | Timer SwiftUI kết hợp kích hoạt chế độ Strict Shield khoá app gây xao nhãng. | **`ForegroundService`** (`FocusTimerService`) với sticky notification điều khiển trực tiếp trên thanh thông báo. |
| **5. Daily Insights & Reports** | Báo cáo trực quan qua SwiftUI, phân tích nhóm ứng dụng (Social, Productivity, Entertainment). | Biểu đồ thanh Jetpack Compose (`UsageBarChart`), thống kê trung bình ngày/tuần. |
| **6. Smart Reminders** | `eventWillReachThresholdWarning` gửi cảnh báo trước 5 phút khi chạm ngưỡng. | Gửi `NotificationCompat` cảnh báo tại mốc **80%** và **100%** hạn mức dùng. |

---

## 🚀 Hướng dẫn mở & Chạy Dự Án

### 🍎 iOS (Xcode)
1. Mở thư mục `focusly_ios/Focusly.xcodeproj` trên macOS với **Xcode 15+**.
2. Vào **Signing & Capabilities**:
   - Chọn Developer Team của bạn.
   - Bật Capability **Family Controls** và **App Groups** (`group.com.focusly.app`).
3. Chạy trên thiết bị thật iPhone (iOS 16.0+) để Apple Screen Time API hoạt động.

### 🤖 Android (Android Studio)
1. Mở **Android Studio** (Koala / Jellyfish / Iguana) -> Chọn **Open** -> Chọn thư mục `focusly_android`.
2. Android Studio sẽ tự động đồng bộ Gradle (`Sync Project with Gradle Files`).
3. Chạy trên máy ảo (Emulator) hoặc điện thoại thật (Android 8.0 trở lên).
4. Cấp 3 quyền hệ thống: **Usage Access**, **Display over other apps**, và **Accessibility Service**.

---

## 🔗 GitHub Repository
👉 **[https://github.com/locsummer/focusly-screen-time](https://github.com/locsummer/focusly-screen-time)**
