---
name: ios-mobile-designer
description: Design iOS apps following Apple Human Interface Guidelines. Typography, Color, and Spacing are primary tools. Generate native, accessible interfaces for iPhone & iPad.
---

# iOS Mobile App Designer

Design professional iOS applications following Apple Human Interface Guidelines (HIG). **Typography, color, and spacing are your primary design tools** — translucent materials are supplementary. This skill enables creation of native, accessible interfaces for iPhone, iPad, and Vision Pro using SwiftUI, React Native, or Flutter.

> **Apple HIG**: "The interface should be legible and easy to understand. Clear text, sharp icons, strong visual hierarchy."

---

## Core Principles

| Principle | Description |
|-----------|-------------|
| **Clarity** | Content is paramount. Text legible, icons precise, adornments subtle |
| **Deference** | UI supports content, never competes with it |
| **Depth** | Visual layers convey hierarchy and indicate what's possible |
| **Consistency** | Familiar controls, standard gestures, predictable behaviors |

---

## Typography System

Apple uses San Francisco (SF Pro). Always use Dynamic Type for accessibility.

### Type Scale

| Style | Size | Weight | Use Case |
|-------|------|--------|----------|
| Large Title | 34pt | Bold | Screen titles |
| Title 1 | 28pt | Bold | Primary headers |
| Title 2 | 22pt | Bold | Section headers |
| Title 3 | 20pt | Semibold | Subsections |
| Headline | 17pt | Semibold | Emphasized body |
| Body | 17pt | Regular | Primary content |
| Callout | 16pt | Regular | Secondary content |
| Subheadline | 15pt | Regular | Tertiary content |
| Footnote | 13pt | Regular | Fine print |
| Caption 1 | 12pt | Regular | Labels |
| Caption 2 | 11pt | Regular | Small labels |

### SwiftUI

```swift
Text("Title").font(.largeTitle)
Text("Body").font(.body)

// Custom with Dynamic Type
Text("Custom")
    .font(.system(size: 17, weight: .semibold, design: .rounded))
    .dynamicTypeSize(...DynamicTypeSize.accessibility3)
```

### React Native

```tsx
const styles = StyleSheet.create({
    largeTitle: { fontSize: 34, fontWeight: 'bold', letterSpacing: 0.37 },
    body: { fontSize: 17, fontWeight: '400', letterSpacing: -0.41 },
});

<Text style={styles.body} allowFontScaling={true} maxFontSizeMultiplier={1.5}>
    Accessible text
</Text>
```

### Flutter

```dart
Text('Title', style: CupertinoTheme.of(context).textTheme.navLargeTitleTextStyle);
Text('Body', style: CupertinoTheme.of(context).textTheme.textStyle);
```

---

## Color System

Use semantic colors that automatically adapt to Light and Dark modes.

### Semantic Colors

| Color | Use |
|-------|-----|
| Label / Secondary / Tertiary | Text hierarchy |
| System Background / Secondary / Tertiary | Backgrounds |
| Separator | Dividers |
| System Blue / Green / Red / Orange | Actions, status |

### SwiftUI

```swift
Text("Primary").foregroundStyle(.primary)
Text("Secondary").foregroundStyle(.secondary)
Rectangle().fill(Color(.systemBackground))
Rectangle().fill(Color(.secondarySystemBackground))
Button("Action") { }.tint(.blue)

// Custom adaptive color
extension Color {
    static let customAccent = Color("AccentColor") // from Assets
}
```

### React Native

```tsx
import { PlatformColor } from 'react-native';

const styles = StyleSheet.create({
    container: { backgroundColor: PlatformColor('systemBackground') },
    text: { color: PlatformColor('label') },
    accent: { color: PlatformColor('systemBlue') },
});
```

### Flutter

```dart
Container(
    color: CupertinoColors.systemBackground.resolveFrom(context),
    child: Text('Label', style: TextStyle(color: CupertinoColors.label.resolveFrom(context))),
);
```

---

## Spacing & Layout

### 8-Point Grid

All spacing should be multiples of 8pt:
- **4pt** - Fine adjustments
- **8pt** - Base unit
- **16pt** - Standard padding
- **24pt** - Section spacing
- **32pt** - Large spacing

### Safe Areas

Always respect safe areas for notch, Dynamic Island, and home indicator.

```swift
// SwiftUI
VStack { content }
    .safeAreaInset(edge: .bottom) { toolbar }
    .ignoresSafeArea(.container, edges: .horizontal)
```

```tsx
// React Native
import { SafeAreaView, useSafeAreaInsets } from 'react-native-safe-area-context';

const Screen = () => {
    const insets = useSafeAreaInsets();
    return <SafeAreaView style={{ paddingBottom: insets.bottom }}>{content}</SafeAreaView>;
};
```

### Adaptive Layouts

```swift
// SwiftUI
@Environment(\.horizontalSizeClass) var sizeClass

if sizeClass == .compact {
    VStack { content }  // iPhone
} else {
    HStack { sidebar; content }  // iPad
}

// Or use NavigationSplitView
NavigationSplitView {
    Sidebar()
} detail: {
    DetailView()
}
```

---

## Backgrounds & Materials

**Solid backgrounds are the default** for maximum legibility. Translucent materials are supplementary.

### When to Use Materials

| Scenario | Recommendation |
|----------|----------------|
| Navigation bar | ✅ Use |
| Tab bar | ✅ Use |
| Modal sheet | ⚠️ Sparingly |
| Cards over busy backgrounds | ❌ Avoid |
| Forms / text-heavy content | ❌ Avoid |
| More than 3 surfaces | ❌ Avoid |

### Performance Limits

| Constraint | iPhone | iPad |
|------------|--------|------|
| Max compositing layers | ≤4 | ≤6 |
| Blur radius | ≤40px | ≤60px |

### Implementation

```swift
// SwiftUI materials
.background(.ultraThinMaterial)
.background(.thinMaterial)
.background(.regularMaterial)

// REQUIRED: Accessibility fallback
struct AccessibleCard: View {
    @Environment(\.accessibilityReduceTransparency) var reduceTransparency

    var body: some View {
        content
            .background(
                reduceTransparency
                    ? AnyShapeStyle(Color(.secondarySystemBackground))
                    : AnyShapeStyle(.ultraThinMaterial)
            )
    }
}
```

```tsx
// React Native
import { BlurView } from '@react-native-community/blur';

<BlurView
    blurType="ultraThinMaterial"
    blurAmount={20}
    reducedTransparencyFallbackColor="#F2F2F7"
>
    {children}
</BlurView>
```

---

## UI Components

### Navigation

```swift
// Standard navigation
NavigationStack {
    List { items }
        .navigationTitle("Title")
        .navigationBarTitleDisplayMode(.large)
        .toolbar {
            ToolbarItem(placement: .primaryAction) {
                Button("Add", systemImage: "plus") { }
            }
        }
}

// With search
.searchable(text: $searchText, placement: .navigationBarDrawer)
```

### Tab Bar

```swift
TabView {
    HomeView()
        .tabItem { Label("Home", systemImage: "house") }
    SettingsView()
        .tabItem { Label("Settings", systemImage: "gear") }
}
```

### Sheets & Modals

```swift
// Resizable sheet (iOS 16+)
.sheet(isPresented: $showSheet) {
    SheetContent()
        .presentationDetents([.medium, .large])
        .presentationDragIndicator(.visible)
}

// Confirmation dialog
.confirmationDialog("Title", isPresented: $showDialog) {
    Button("Delete", role: .destructive) { delete() }
    Button("Cancel", role: .cancel) { }
}
```

### Buttons

```swift
// Standard styles
Button("Primary") { }.buttonStyle(.borderedProminent)
Button("Secondary") { }.buttonStyle(.bordered)
Button("Tertiary") { }.buttonStyle(.borderless)
Button("Delete", role: .destructive) { }

// Custom with haptic
Button(action: {
    UIImpactFeedbackGenerator(style: .medium).impactOccurred()
    action()
}) {
    Label("Action", systemImage: "star")
}
```

### Forms & Lists

```swift
Form {
    Section("Account") {
        TextField("Email", text: $email)
            .textContentType(.emailAddress)
            .keyboardType(.emailAddress)
        SecureField("Password", text: $password)
            .textContentType(.password)
    }

    Section {
        Toggle("Notifications", isOn: $notifications)
        Picker("Theme", selection: $theme) {
            Text("System").tag(Theme.system)
            Text("Light").tag(Theme.light)
            Text("Dark").tag(Theme.dark)
        }
    }
}

List {
    ForEach(items) { item in
        Text(item.title)
    }
    .onDelete(perform: delete)
    .onMove(perform: move)
}
.listStyle(.insetGrouped)
```

---

## Touch Targets & Gestures

### Minimum Sizes

- **Minimum touch target**: 44x44pt
- **Recommended**: 48x48pt for primary actions

### Standard Gestures

| Gesture | Use |
|---------|-----|
| Tap | Primary action |
| Long press | Context menu |
| Swipe | Delete, actions |
| Pull down | Refresh |
| Pinch | Zoom |
| Edge swipe | Back navigation |

### Haptic Feedback

```swift
// Haptic hierarchy
UIImpactFeedbackGenerator(style: .light).impactOccurred()   // Subtle
UIImpactFeedbackGenerator(style: .medium).impactOccurred()  // Standard
UIImpactFeedbackGenerator(style: .heavy).impactOccurred()   // Emphasis

UISelectionFeedbackGenerator().selectionChanged()           // Selection
UINotificationFeedbackGenerator().notificationOccurred(.success)  // Result

// Haptic on press, not release (anticipatory)
Button(action: action) { label }
    .simultaneousGesture(
        DragGesture(minimumDistance: 0)
            .onChanged { _ in
                UIImpactFeedbackGenerator(style: .medium).impactOccurred()
            }
    )
```

---

## Accessibility

### VoiceOver

```swift
Image(systemName: "star.fill")
    .accessibilityLabel("Favorite")
    .accessibilityHint("Double tap to remove from favorites")

Button("Delete") { }
    .accessibilityAddTraits(.isButton)
```

### Dynamic Type

```swift
// Allow text scaling
Text("Scales with system settings")
    .font(.body)
    .dynamicTypeSize(...DynamicTypeSize.accessibility3)

// Limit scaling for specific elements
Text("Limited scaling")
    .dynamicTypeSize(.large ... .accessibility1)
```

### Color Contrast

| Type | Minimum Ratio |
|------|---------------|
| Normal text | 4.5:1 |
| Large text (18pt+) | 3:1 |
| UI components | 3:1 |

### Motion & Transparency

```swift
@Environment(\.accessibilityReduceMotion) var reduceMotion
@Environment(\.accessibilityReduceTransparency) var reduceTransparency

// Respect preferences
withAnimation(reduceMotion ? nil : .spring()) {
    isExpanded.toggle()
}

// Provide solid fallbacks
.background(reduceTransparency ? Color(.systemBackground) : .ultraThinMaterial)
```

---

## Animation & Motion

### Spring Animations

```swift
// Presets
extension Animation {
    static let snappy = Animation.spring(response: 0.25, dampingFraction: 0.8)
    static let standard = Animation.spring(response: 0.4, dampingFraction: 0.75)
    static let bouncy = Animation.spring(response: 0.5, dampingFraction: 0.5)
}

withAnimation(.standard) { isExpanded.toggle() }
```

### Symbol Effects (iOS 17+)

```swift
Image(systemName: "wifi")
    .symbolEffect(.variableColor.iterative)

Image(systemName: "checkmark.circle")
    .symbolEffect(.bounce.up.byLayer, value: trigger)
    .contentTransition(.symbolEffect(.replace))
```

### Phased & Keyframe Animations (iOS 17+)

```swift
// Phased
PhaseAnimator([Phase.start, .middle, .end], trigger: trigger) { phase in
    Circle().scaleEffect(phase.scale)
} animation: { phase in
    .spring(duration: 0.3)
}

// Keyframes
content.keyframeAnimator(initialValue: Values(), trigger: trigger) { content, value in
    content.scaleEffect(value.scale).offset(y: value.offset)
} keyframes: { _ in
    KeyframeTrack(\.scale) {
        SpringKeyframe(1.2, duration: 0.2)
        SpringKeyframe(1.0, duration: 0.3)
    }
}
```

### Performance Tips

1. **Use transform properties** (scale, rotation, translation) - GPU-accelerated
2. **Avoid animating layout** (width/height trigger recalculation)
3. **Use `.drawingGroup()`** for complex hierarchies
4. **Profile on device** - simulator doesn't reflect real performance

---

## SF Symbols

6,000+ symbols. Use them instead of custom icons when possible.

```swift
// Basic
Image(systemName: "star.fill")

// Variants
Image(systemName: "star")
    .symbolVariant(.fill)
    .symbolRenderingMode(.hierarchical)

// Custom configuration
Image(systemName: "star.fill")
    .font(.system(size: 24, weight: .medium))
    .foregroundStyle(.yellow)
```

---

## Live Activities & Widgets

### Live Activities

For time-sensitive, real-time data (flights, deliveries, sports).

```swift
struct FlightActivity: ActivityAttributes {
    let flightNumber: String

    struct ContentState: Codable, Hashable {
        let status: String
        let progress: Double
    }
}

// Widget configuration
ActivityConfiguration(for: FlightActivity.self) { context in
    // Lock Screen view
} dynamicIsland: { context in
    DynamicIsland {
        DynamicIslandExpandedRegion(.leading) { Text(context.attributes.flightNumber) }
        DynamicIslandExpandedRegion(.trailing) { Text(context.state.status) }
    } compactLeading: {
        Image(systemName: "airplane")
    } compactTrailing: {
        Text(context.state.status)
    } minimal: {
        Image(systemName: "airplane.circle.fill")
    }
}
```

### Widgets

```swift
struct MyWidget: Widget {
    var body: some WidgetConfiguration {
        StaticConfiguration(kind: "MyWidget", provider: Provider()) { entry in
            WidgetView(entry: entry)
                .containerBackground(.fill.tertiary, for: .widget)
        }
        .supportedFamilies([.systemSmall, .systemMedium, .systemLarge])
    }
}

// Interactive button (iOS 17+)
Button(intent: ToggleIntent()) {
    Image(systemName: isOn ? "checkmark.circle.fill" : "circle")
}
.invalidatableContent()
```

---

## iOS 18+ Features

### Control Center Widgets

```swift
struct QuickToggle: ControlWidget {
    var body: some ControlWidgetConfiguration {
        StaticControlConfiguration(kind: "toggle") {
            ControlWidgetToggle("Feature", isOn: binding, action: ToggleIntent()) { isOn in
                Label(isOn ? "On" : "Off", systemImage: isOn ? "checkmark" : "xmark")
            }
        }
        .displayName("Quick Toggle")
    }
}
```

### App Intents

```swift
struct CreateItemIntent: AppIntent {
    static var title: LocalizedStringResource = "Create Item"

    @Parameter(title: "Name")
    var name: String

    func perform() async throws -> some IntentResult {
        await Manager.shared.create(name: name)
        return .result()
    }
}

// Siri/Spotlight phrases
struct AppShortcuts: AppShortcutsProvider {
    static var appShortcuts: [AppShortcut] {
        AppShortcut(
            intent: CreateItemIntent(),
            phrases: ["Create item in \(.applicationName)"],
            shortTitle: "Create",
            systemImageName: "plus"
        )
    }
}
```

### Tinted Icons

Provide `AppIcon - Dark` and `AppIcon - Tinted` variants in Asset Catalog for automatic tinting.

---

## visionOS

### Key Differences

| Aspect | iOS | visionOS |
|--------|-----|----------|
| Input | Touch | Eye + hands |
| Layout | 2D | 3D volumes |
| Typography | Standard | 20% larger |
| Materials | Optional | Glass default |

### Implementation

```swift
// Glass background (visionOS only)
#if os(visionOS)
content.glassBackgroundEffect()
#else
content.background(.ultraThinMaterial)
#endif

// Hover effects
Button("Action") { }
    .hoverEffect(.highlight)

// Ornaments
RealityView { content in }
    .ornament(attachmentAnchor: .scene(.bottom)) {
        HStack { controls }
            .glassBackgroundEffect()
    }
```

---

## Empty States & Edge Cases

### Empty State Pattern

```swift
ContentUnavailableView {
    Label("No Items", systemImage: "tray")
} description: {
    Text("Items you add will appear here")
} actions: {
    Button("Add Item") { }
}
```

### Loading States

```swift
// Skeleton loading (preferred over spinners)
ForEach(0..<5) { _ in
    RoundedRectangle(cornerRadius: 8)
        .fill(.quaternary)
        .frame(height: 60)
        .shimmer()  // Custom modifier
}
```

### Error States

```swift
ContentUnavailableView {
    Label("Connection Error", systemImage: "wifi.slash")
} description: {
    Text("Check your internet connection")
} actions: {
    Button("Try Again", action: retry)
}
```

---

## Screen Sizes

### iPhone (Points)

| Device | Width | Height |
|--------|-------|--------|
| iPhone 15 Pro Max | 430 | 932 |
| iPhone 15 Pro | 393 | 852 |
| iPhone 15 | 393 | 852 |
| iPhone SE | 375 | 667 |

### iPad (Points)

| Device | Width | Height |
|--------|-------|--------|
| iPad Pro 12.9" | 1024 | 1366 |
| iPad Pro 11" | 834 | 1194 |
| iPad Air | 820 | 1180 |

---

## Best Practices Checklist

### Core Design
- [ ] Typography: SF Pro with Dynamic Type
- [ ] Colors: Semantic colors (auto dark/light)
- [ ] Spacing: 8-point grid
- [ ] Touch targets: ≥44x44pt

### Materials (If Used)
- [ ] ≤3 surfaces with materials
- [ ] ≤4 compositing layers
- [ ] Solid fallback for Reduce Transparency

### Accessibility
- [ ] VoiceOver labels on all interactive elements
- [ ] Dynamic Type support
- [ ] Color contrast ≥4.5:1 (text), ≥3:1 (UI)
- [ ] Reduce Motion alternatives
- [ ] Reduce Transparency fallbacks

### Animation
- [ ] Spring-based (not linear)
- [ ] 200-500ms duration
- [ ] Interruptible
- [ ] Respects Reduce Motion

### iOS 18+
- [ ] Control Center widgets (if applicable)
- [ ] App Intents for Siri/Shortcuts
- [ ] Tinted icon variants

### Testing
- [ ] Multiple device sizes
- [ ] Light and Dark modes
- [ ] VoiceOver enabled
- [ ] Dynamic Type sizes
- [ ] Reduce Motion/Transparency enabled
