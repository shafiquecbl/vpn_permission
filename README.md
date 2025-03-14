# vpn_permission

A Flutter plugin to manage VPN permissions on Android and iOS.

**Android**: Automatically requests VPN permission via `VpnService`.  
**iOS**: Configures VPN profiles using `NETunnelProviderManager`.

## Features

- **Android**: Auto-adds `BIND_VPN_SERVICE` permission (no setup required).
- **iOS**: Triggers system dialogs for VPN configuration.
- Simple API for permission checks/requests.

## Installation

Add to `pubspec.yaml`:

```yaml
dependencies:
  vpn_permission: ^0.0.1
```

## Setup

### Android

Nothing needed! The plugin automatically adds:

```xml
<uses-permission android:name="android.permission.BIND_VPN_SERVICE" />
```

### iOS

#### 1. Add Capabilities

1. Open your project in **Xcode**.
2. For your main app target:
   - **Signing & Capabilities** → **+** → **App Groups** (e.g., `group.com.example.vpn`)
   - **+** → **Network Extensions** → Enable **Packet Tunnel**

#### 2. Create VPN Extension

1. **File** → **New** → **Target** → **Network Extension** → **Packet Tunnel Provider**
2. Name: `VPNExtension`
3. Bundle ID: `com.example.vpn.VPNExtension` (match your app ID + `.VPNExtension`)

#### 3. Configure Extension

Repeat for `VPNExtension` target:

- Same **App Group** as main app
- **Network Extensions** capability

## Usage

### 1. Import

```dart
import 'package:vpn_permission/vpn_permission.dart';
```

### 2. Define Constants

```dart
class AppConstants {
  static const String iOSPackageName = "com.example.vpn";
  static const String providerBundleIdentifier = "$iOSPackageName.VPNExtension";
  static const String groupIdentifier = "group.$iOSPackageName";
  static const String localizedDescription = "Free VPN - Fast & Secure";
}
```

### 3. Check/Request Permissions

```dart
// Check permission
bool hasPermission = await VpnPermission.checkPermission();

// Request permission
if (!hasPermission) {
  bool granted = await VpnPermission.requestPermission(
    providerBundleIdentifier: AppConstants.providerBundleIdentifier,
    groupIdentifier: AppConstants.groupIdentifier,
    localizedDescription: AppConstants.localizedDescription,
  );
  print("Granted: $granted");
}
```

## Contribution

Issues/PRs welcome! Keep it simple.
