# iOS Mobile Designer

[![Version](https://img.shields.io/badge/Version-2.0.0-green.svg)](CHANGELOG.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai)
[![iOS](https://img.shields.io/badge/iOS-26+-000000?logo=apple)](https://developer.apple.com/ios/)
[![SwiftUI](https://img.shields.io/badge/SwiftUI-Supported-orange?logo=swift)](https://developer.apple.com/xcode/swiftui/)
[![React Native](https://img.shields.io/badge/React%20Native-Supported-61DAFB?logo=react)](https://reactnative.dev/)
[![Flutter](https://img.shields.io/badge/Flutter-Supported-02569B?logo=flutter)](https://flutter.dev/)

A Claude Code skill for designing **premium iOS applications** following Apple's Human Interface Guidelines (HIG) with support for **Liquid Glass** (iOS 26). Generates **Flighty-level** quality designs.

<p align="center">
  <img src="https://developer.apple.com/design/human-interface-guidelines/images/intro/platforms/platform-iOS-intro_2x.png" alt="iOS Design" width="400">
</p>

## Features

### Core Design
- **Multi-Framework Support**: SwiftUI, React Native, and Flutter code examples
- **Liquid Glass (iOS 26)**: Modern translucent, glass-like UI elements with refraction effects
- **Adaptive Design**: iPhone and iPad layouts with size class awareness
- **Accessibility First**: VoiceOver, Dynamic Type, color contrast, Reduce Motion

### Premium Features (v2.0)
- **Live Activities & Dynamic Island**: Flighty-style real-time updates
- **Widgets**: All sizes, Lock Screen, StandBy, interactive (iOS 17+)
- **Physics-Based Animations**: Spring presets, gesture-responsive, UIKit Dynamics
- **Haptic Hierarchy System**: Strategic feedback (light → heavy)
- **Data Visualization**: SwiftUI Charts, progress rings, real-time updates
- **Empty States & Edge Cases**: Skeleton loading, error states, offline mode
- **Premium Polish**: Shadow system, corner radius hierarchy, gradients
- **Sound Design**: When and how to use audio feedback
- **Context-Specific Design**: Travel, Finance, Health, Productivity patterns

## Installation

### From GitHub (Recommended)

**Step 1: Add the marketplace**
```bash
/plugin marketplace add sh4d0w/ios-mobile-designer
```

**Step 2: Install the plugin**
```bash
/plugin install ios-mobile-designer@sh4d0w-plugins
```

Or use the full Git URL:
```bash
/plugin marketplace add https://github.com/sh4d0w/ios-mobile-designer.git
/plugin install ios-mobile-designer@sh4d0w-plugins
```

### Manual Installation (Local)

```bash
git clone https://github.com/sh4d0w/ios-mobile-designer.git
cd ios-mobile-designer
/plugin install .
```

### Interactive UI

You can also use the plugin manager UI:
```bash
/plugin
```
Then navigate to **Marketplaces** tab and add `sh4d0w/ios-mobile-designer`.

## Updating

To update to the latest version:

```bash
# Uninstall current version
/plugin uninstall ios-mobile-designer@sh4d0w-plugins

# Refresh marketplace
/plugin marketplace add sh4d0w/ios-mobile-designer

# Reinstall
/plugin install ios-mobile-designer@sh4d0w-plugins
```

Check [CHANGELOG.md](CHANGELOG.md) for version history.

## Usage

The skill automatically activates when you work on iOS mobile app design tasks. Simply describe what you want to build:

```
"Create a settings screen with profile section and preferences"

"Design a music player with album art, controls, and playlist"

"Build a login form with email/password fields and social sign-in"

"Create an adaptive dashboard that works on iPhone and iPad"

"Design a Liquid Glass navigation bar with blur effects"
```

Claude will generate production-ready code following Apple HIG with:
- Proper semantic colors for dark/light mode
- Accessibility labels and hints
- Touch targets of 44x44pt minimum
- Native animations and haptic feedback
- Liquid Glass effects where appropriate

## Supported Frameworks

| Framework | Version | Notes |
|-----------|---------|-------|
| SwiftUI | iOS 15+ | Primary framework, latest APIs for iOS 26 Liquid Glass |
| React Native | 0.72+ | With native iOS components and blur effects |
| Flutter | 3.x | Cupertino widgets and BackdropFilter |

## What's Included

### Design Principles
- **Clarity**: Clean, legible content that's easy to understand
- **Deference**: UI supports content without competing for attention
- **Depth**: Visual layers convey hierarchy and relationships
- **Consistency**: Familiar patterns and predictable behaviors

### Key Specifications
| Aspect | Specification |
|--------|---------------|
| Typography | SF Pro with Dynamic Type |
| Colors | Semantic colors (auto dark/light) |
| Spacing | 8-point grid system |
| Touch Targets | Minimum 44×44pt |
| Animations | Spring-based with Reduce Motion support |

### Components Covered
- Navigation (NavigationStack, Tab Bar, Sheets)
- Forms (TextField, SecureField, Toggle, Picker)
- Lists (insetGrouped, sections, swipe actions)
- Buttons (filled, bordered, plain, destructive)
- Cards & Containers with Liquid Glass effects
- Accessibility (VoiceOver, Dynamic Type, Reduce Motion)

## Project Structure

```
ios-mobile-designer/
├── .claude-plugin/
│   ├── plugin.json          # Plugin manifest
│   └── marketplace.json     # Marketplace configuration
├── skills/
│   └── ios-mobile-designer/
│       └── SKILL.md         # Main skill instructions (~34KB)
├── .gitignore
├── LICENSE
└── README.md
```

## Resources

- [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines)
- [SF Symbols](https://developer.apple.com/sf-symbols/)
- [iOS Design Resources](https://developer.apple.com/design/resources/)
- [WWDC25: Get to know the new design system](https://developer.apple.com/videos/play/wwdc2025/356/)

## Contributing

Contributions are welcome! Please read our [Contributing Guidelines](CONTRIBUTING.md) before submitting PRs.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Credits

Inspired by and based on research from:
- [Anthropic frontend-design plugin](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design)
- [jamesrochabrun/skills apple-hig-designer](https://github.com/jamesrochabrun/skills)
- [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<p align="center">
  Made with ❤️ for the iOS development community
</p>
