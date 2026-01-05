# iOS Mobile App Designer

Design professional iOS applications following Apple Human Interface Guidelines (HIG) with support for the new Liquid Glass design language (iOS 26). This skill enables creation of native, accessible, and visually stunning interfaces for iPhone and iPad using SwiftUI, React Native, or Flutter.

---

## Core Design Principles

### Clarity
Content is paramount. Every element serves the user's goals. Text must be legible at all sizes, icons precise and clear, adornments subtle and appropriate.

### Deference
The UI helps people understand and interact with content but never competes with it. Fluid motion and crisp interfaces support interaction without distracting from the experience.

### Depth
Visual layers and realistic motion convey hierarchy, establish relationships, and indicate what's possible. Transitions provide a sense of depth as you navigate through content.

### Consistency
Use familiar controls, standard gestures, and predictable behaviors. People should feel confident they understand how to interact with your app.

---

## Liquid Glass (iOS 26)

The most significant visual update since iOS 7. Liquid Glass introduces translucent, glass-like elements that respond to light, motion, and content.

### Key Characteristics
- **Translucency**: UI elements feature rounded, translucent surfaces with optical properties of real glass
- **Refraction**: Content beneath glass elements is subtly distorted, creating depth
- **Dynamic response**: Elements adapt to lighting conditions and underlying content
- **Fluidity**: Smooth transitions and morphing between states

### SwiftUI Implementation

```swift
// Basic glass effect
struct GlassCard: View {
    var body: some View {
        VStack {
            Text("Glass Card")
                .font(.headline)
            Text("With Liquid Glass effect")
                .font(.subheadline)
        }
        .padding()
        .background(.ultraThinMaterial)
        .clipShape(RoundedRectangle(cornerRadius: 20, style: .continuous))
        // iOS 26+ specific
        .glassEffect()
    }
}

// Navigation bar with glass
NavigationStack {
    ContentView()
        .toolbarBackground(.ultraThinMaterial, for: .navigationBar)
}

// Tab bar with glass
TabView {
    // tabs
}
.tabViewStyle(.automatic)
.background(.ultraThinMaterial)
```

### React Native Implementation

```tsx
import { BlurView } from '@react-native-community/blur';
import { StyleSheet, View, Text } from 'react-native';

const GlassCard = () => (
  <BlurView
    style={styles.glassCard}
    blurType="ultraThinMaterial"
    blurAmount={20}
    reducedTransparencyFallbackColor="white"
  >
    <Text style={styles.title}>Glass Card</Text>
  </BlurView>
);

const styles = StyleSheet.create({
  glassCard: {
    borderRadius: 20,
    padding: 16,
    overflow: 'hidden',
  },
  title: {
    fontSize: 17,
    fontWeight: '600',
  },
});
```

### Flutter Implementation

```dart
import 'dart:ui';

class GlassCard extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ClipRRect(
      borderRadius: BorderRadius.circular(20),
      child: BackdropFilter(
        filter: ImageFilter.blur(sigmaX: 20, sigmaY: 20),
        child: Container(
          padding: EdgeInsets.all(16),
          decoration: BoxDecoration(
            color: CupertinoColors.systemBackground.withOpacity(0.3),
            borderRadius: BorderRadius.circular(20),
            border: Border.all(
              color: CupertinoColors.white.withOpacity(0.2),
            ),
          ),
          child: Text('Glass Card'),
        ),
      ),
    );
  }
}
```

---

## Typography System

Apple uses San Francisco (SF Pro) as the system font. Use Dynamic Type to support accessibility.

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

### SwiftUI Typography

```swift
// Use semantic text styles
Text("Large Title")
    .font(.largeTitle)

Text("Body text with system font")
    .font(.body)

// Custom with Dynamic Type support
Text("Custom")
    .font(.system(size: 17, weight: .semibold, design: .rounded))

// Ensure accessibility scaling
Text("Accessible")
    .font(.body)
    .dynamicTypeSize(...DynamicTypeSize.accessibility3)
```

### React Native Typography

```tsx
import { Text, StyleSheet } from 'react-native';

// Use system font (San Francisco on iOS)
const styles = StyleSheet.create({
  largeTitle: {
    fontSize: 34,
    fontWeight: 'bold',
    letterSpacing: 0.37,
  },
  title1: {
    fontSize: 28,
    fontWeight: 'bold',
    letterSpacing: 0.36,
  },
  body: {
    fontSize: 17,
    fontWeight: '400',
    letterSpacing: -0.41,
  },
  caption: {
    fontSize: 12,
    fontWeight: '400',
    letterSpacing: 0,
  },
});

// Support Dynamic Type
<Text
  style={styles.body}
  allowFontScaling={true}
  maxFontSizeMultiplier={1.5}
>
  Accessible text
</Text>
```

### Flutter Typography (Cupertino)

```dart
import 'package:flutter/cupertino.dart';

// Use CupertinoTextTheme
Text(
  'Large Title',
  style: CupertinoTheme.of(context).textTheme.navLargeTitleTextStyle,
);

Text(
  'Body',
  style: CupertinoTheme.of(context).textTheme.textStyle,
);

// Custom with SF Pro
Text(
  'Custom',
  style: TextStyle(
    fontFamily: '.SF Pro Text',
    fontSize: 17,
    fontWeight: FontWeight.w600,
  ),
);
```

---

## Color System

Use semantic colors that automatically adapt to Light and Dark modes.

### Semantic Colors

| Color | Light Mode | Dark Mode | Use |
|-------|------------|-----------|-----|
| Label | Black | White | Primary text |
| Secondary Label | 60% gray | 60% gray | Secondary text |
| Tertiary Label | 30% gray | 30% gray | Disabled text |
| System Background | White | Black | Primary background |
| Secondary Background | F2F2F7 | 1C1C1E | Grouped content |
| Tertiary Background | White | 2C2C2E | Elevated surfaces |
| Separator | C6C6C8 | 38383A | Dividers |
| System Blue | 007AFF | 0A84FF | Links, actions |
| System Green | 34C759 | 30D158 | Success |
| System Red | FF3B30 | FF453A | Errors, destructive |
| System Orange | FF9500 | FF9F0A | Warnings |

### SwiftUI Colors

```swift
// Semantic colors (auto dark/light)
Text("Primary")
    .foregroundStyle(.primary)

Text("Secondary")
    .foregroundStyle(.secondary)

Rectangle()
    .fill(Color(.systemBackground))

Rectangle()
    .fill(Color(.secondarySystemBackground))

// System colors
Button("Action") { }
    .tint(.blue)

// Custom with dark mode support
extension Color {
    static let customAccent = Color("AccentColor") // from Assets
}

// Or programmatic
extension Color {
    static let adaptiveBackground = Color(
        light: .white,
        dark: Color(hex: "1C1C1E")
    )
}
```

### React Native Colors

```tsx
import { PlatformColor, DynamicColorIOS } from 'react-native';

const styles = StyleSheet.create({
  container: {
    // System semantic color
    backgroundColor: PlatformColor('systemBackground'),
  },
  text: {
    // Adaptive label color
    color: PlatformColor('label'),
  },
  card: {
    // Secondary background
    backgroundColor: PlatformColor('secondarySystemBackground'),
  },
  accent: {
    // System blue
    color: PlatformColor('systemBlue'),
  },
});

// Dynamic color with explicit values
const dynamicColor = DynamicColorIOS({
  light: '#FFFFFF',
  dark: '#1C1C1E',
});
```

### Flutter Colors (Cupertino)

```dart
import 'package:flutter/cupertino.dart';

// Semantic colors
Container(
  color: CupertinoColors.systemBackground,
  child: Text(
    'Label',
    style: TextStyle(color: CupertinoColors.label),
  ),
);

// Resolve dynamic colors
final resolvedColor = CupertinoDynamicColor.resolve(
  CupertinoColors.secondarySystemBackground,
  context,
);

// System colors
CupertinoButton(
  color: CupertinoColors.systemBlue,
  child: Text('Action'),
  onPressed: () {},
);

// Custom dynamic color
const customColor = CupertinoDynamicColor.withBrightness(
  color: Color(0xFFFFFFFF),
  darkColor: Color(0xFF1C1C1E),
);
```

---

## Spacing & Layout

### 8-Point Grid System

All spacing values should be multiples of 8pt for visual harmony:
- 4pt (half-unit for fine adjustments)
- 8pt (base unit)
- 16pt (standard padding)
- 24pt (section spacing)
- 32pt (large spacing)
- 48pt (major sections)

### Safe Areas

Always respect safe areas for notch, Dynamic Island, and home indicator.

#### SwiftUI

```swift
struct ContentView: View {
    var body: some View {
        VStack {
            // Content
        }
        .safeAreaInset(edge: .bottom) {
            // Bottom toolbar
        }
        // Ignore specific edges when needed
        .ignoresSafeArea(.container, edges: .horizontal)
    }
}

// Keyboard avoidance (automatic in iOS 14+)
ScrollView {
    // Content automatically adjusts for keyboard
}
```

#### React Native

```tsx
import { SafeAreaView, useSafeAreaInsets } from 'react-native-safe-area-context';

const Screen = () => {
  const insets = useSafeAreaInsets();

  return (
    <SafeAreaView style={styles.container}>
      <View style={{ paddingBottom: insets.bottom }}>
        {/* Content */}
      </View>
    </SafeAreaView>
  );
};

// With KeyboardAvoidingView
import { KeyboardAvoidingView, Platform } from 'react-native';

<KeyboardAvoidingView
  behavior={Platform.OS === 'ios' ? 'padding' : 'height'}
  keyboardVerticalOffset={100}
>
  {/* Form content */}
</KeyboardAvoidingView>
```

#### Flutter

```dart
import 'package:flutter/cupertino.dart';

class Screen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final mediaQuery = MediaQuery.of(context);

    return CupertinoPageScaffold(
      // Safe area handled automatically
      child: SafeArea(
        child: Padding(
          padding: EdgeInsets.only(
            bottom: mediaQuery.viewInsets.bottom, // Keyboard
          ),
          child: // Content
        ),
      ),
    );
  }
}
```

### Adaptive Layouts (iPhone & iPad)

#### SwiftUI

```swift
struct AdaptiveView: View {
    @Environment(\.horizontalSizeClass) var horizontalSizeClass

    var body: some View {
        if horizontalSizeClass == .compact {
            // iPhone layout (stack vertically)
            VStack {
                SidebarContent()
                MainContent()
            }
        } else {
            // iPad layout (side by side)
            HStack {
                SidebarContent()
                    .frame(width: 320)
                MainContent()
            }
        }
    }
}

// NavigationSplitView for automatic adaptation
NavigationSplitView {
    Sidebar()
} content: {
    ContentList()
} detail: {
    DetailView()
}
```

#### React Native

```tsx
import { useWindowDimensions } from 'react-native';

const AdaptiveLayout = () => {
  const { width } = useWindowDimensions();
  const isTablet = width >= 768;

  return (
    <View style={[styles.container, isTablet && styles.tabletContainer]}>
      {isTablet && <Sidebar style={styles.sidebar} />}
      <MainContent style={isTablet ? styles.tabletMain : styles.phoneMain} />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  tabletContainer: {
    flexDirection: 'row',
  },
  sidebar: {
    width: 320,
  },
  phoneMain: {
    flex: 1,
  },
  tabletMain: {
    flex: 1,
  },
});
```

#### Flutter

```dart
class AdaptiveLayout extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return LayoutBuilder(
      builder: (context, constraints) {
        if (constraints.maxWidth >= 768) {
          // iPad layout
          return Row(
            children: [
              SizedBox(width: 320, child: Sidebar()),
              Expanded(child: MainContent()),
            ],
          );
        }
        // iPhone layout
        return MainContent();
      },
    );
  }
}
```

---

## UI Components

### Navigation

#### Navigation Bar (SwiftUI)

```swift
NavigationStack {
    List {
        // Content
    }
    .navigationTitle("Settings")
    .navigationBarTitleDisplayMode(.large) // .inline for small
    .toolbar {
        ToolbarItem(placement: .topBarTrailing) {
            Button("Edit") { }
        }
    }
    .toolbarBackground(.ultraThinMaterial, for: .navigationBar)
}
```

#### Navigation Bar (React Native)

```tsx
import { createNativeStackNavigator } from '@react-navigation/native-stack';

const Stack = createNativeStackNavigator();

<Stack.Navigator
  screenOptions={{
    headerLargeTitle: true,
    headerTranslucent: true,
    headerBlurEffect: 'regular',
  }}
>
  <Stack.Screen
    name="Settings"
    component={SettingsScreen}
    options={{
      headerRight: () => <Button title="Edit" />,
    }}
  />
</Stack.Navigator>
```

#### Navigation Bar (Flutter)

```dart
CupertinoPageScaffold(
  navigationBar: CupertinoNavigationBar(
    largeTitle: Text('Settings'),
    trailing: CupertinoButton(
      padding: EdgeInsets.zero,
      child: Text('Edit'),
      onPressed: () {},
    ),
    // Glass effect
    backgroundColor: CupertinoColors.systemBackground.withOpacity(0.7),
  ),
  child: // Content
);
```

### Tab Bar

#### SwiftUI

```swift
TabView {
    HomeView()
        .tabItem {
            Label("Home", systemImage: "house.fill")
        }

    SearchView()
        .tabItem {
            Label("Search", systemImage: "magnifyingglass")
        }

    ProfileView()
        .tabItem {
            Label("Profile", systemImage: "person.fill")
        }
}
.tint(.blue)
```

#### React Native

```tsx
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { BlurView } from '@react-native-community/blur';

const Tab = createBottomTabNavigator();

<Tab.Navigator
  screenOptions={{
    tabBarStyle: { position: 'absolute' },
    tabBarBackground: () => (
      <BlurView
        style={StyleSheet.absoluteFill}
        blurType="regular"
      />
    ),
  }}
>
  <Tab.Screen
    name="Home"
    component={HomeScreen}
    options={{
      tabBarIcon: ({ color }) => <HomeIcon color={color} />,
    }}
  />
</Tab.Navigator>
```

#### Flutter

```dart
CupertinoTabScaffold(
  tabBar: CupertinoTabBar(
    items: const [
      BottomNavigationBarItem(
        icon: Icon(CupertinoIcons.home),
        label: 'Home',
      ),
      BottomNavigationBarItem(
        icon: Icon(CupertinoIcons.search),
        label: 'Search',
      ),
      BottomNavigationBarItem(
        icon: Icon(CupertinoIcons.person),
        label: 'Profile',
      ),
    ],
  ),
  tabBuilder: (context, index) {
    switch (index) {
      case 0: return HomeScreen();
      case 1: return SearchScreen();
      case 2: return ProfileScreen();
      default: return HomeScreen();
    }
  },
);
```

### Sheets & Modals

#### SwiftUI

```swift
struct ContentView: View {
    @State private var showSheet = false

    var body: some View {
        Button("Show Sheet") {
            showSheet = true
        }
        .sheet(isPresented: $showSheet) {
            SheetContent()
                .presentationDetents([.medium, .large])
                .presentationDragIndicator(.visible)
                .presentationBackground(.ultraThinMaterial)
        }
    }
}

// Full screen cover
.fullScreenCover(isPresented: $showFullScreen) {
    FullScreenContent()
}

// Confirmation dialog
.confirmationDialog("Options", isPresented: $showOptions) {
    Button("Option 1") { }
    Button("Delete", role: .destructive) { }
    Button("Cancel", role: .cancel) { }
}
```

#### React Native

```tsx
import { Modal, ActionSheetIOS } from 'react-native';
import BottomSheet from '@gorhom/bottom-sheet';

// Bottom sheet
const Sheet = () => {
  const snapPoints = ['50%', '90%'];

  return (
    <BottomSheet
      snapPoints={snapPoints}
      backgroundStyle={{ backgroundColor: 'rgba(255,255,255,0.8)' }}
      handleIndicatorStyle={{ backgroundColor: '#C6C6C8' }}
    >
      {/* Content */}
    </BottomSheet>
  );
};

// Action sheet (native)
ActionSheetIOS.showActionSheetWithOptions(
  {
    options: ['Cancel', 'Option 1', 'Delete'],
    cancelButtonIndex: 0,
    destructiveButtonIndex: 2,
  },
  (buttonIndex) => { /* handle */ }
);
```

#### Flutter

```dart
// Bottom sheet
showCupertinoModalPopup(
  context: context,
  builder: (context) => CupertinoActionSheet(
    title: Text('Options'),
    actions: [
      CupertinoActionSheetAction(
        onPressed: () {},
        child: Text('Option 1'),
      ),
      CupertinoActionSheetAction(
        onPressed: () {},
        isDestructiveAction: true,
        child: Text('Delete'),
      ),
    ],
    cancelButton: CupertinoActionSheetAction(
      onPressed: () => Navigator.pop(context),
      child: Text('Cancel'),
    ),
  ),
);

// Modal sheet
showCupertinoModalPopup(
  context: context,
  builder: (context) => Container(
    height: MediaQuery.of(context).size.height * 0.5,
    decoration: BoxDecoration(
      color: CupertinoColors.systemBackground,
      borderRadius: BorderRadius.vertical(top: Radius.circular(20)),
    ),
    child: // Content
  ),
);
```

### Buttons

#### SwiftUI

```swift
// Filled button (primary action)
Button("Continue") { }
    .buttonStyle(.borderedProminent)

// Bordered button (secondary)
Button("Cancel") { }
    .buttonStyle(.bordered)

// Plain button (tertiary)
Button("Skip") { }
    .buttonStyle(.plain)

// Destructive
Button("Delete", role: .destructive) { }
    .buttonStyle(.borderedProminent)

// Custom button
Button(action: { }) {
    HStack {
        Image(systemName: "plus")
        Text("Add Item")
    }
    .frame(maxWidth: .infinity)
    .padding()
    .background(.blue)
    .foregroundStyle(.white)
    .clipShape(RoundedRectangle(cornerRadius: 12))
}

// Minimum touch target (44x44)
Button("Small") { }
    .frame(minWidth: 44, minHeight: 44)
```

#### React Native

```tsx
import { Pressable, Text, StyleSheet } from 'react-native';

// Filled button
const FilledButton = ({ title, onPress, destructive }) => (
  <Pressable
    style={({ pressed }) => [
      styles.filledButton,
      destructive && styles.destructive,
      pressed && styles.pressed,
    ]}
    onPress={onPress}
  >
    <Text style={styles.filledButtonText}>{title}</Text>
  </Pressable>
);

const styles = StyleSheet.create({
  filledButton: {
    backgroundColor: '#007AFF',
    paddingVertical: 14,
    paddingHorizontal: 20,
    borderRadius: 12,
    minHeight: 44, // Touch target
    alignItems: 'center',
  },
  destructive: {
    backgroundColor: '#FF3B30',
  },
  pressed: {
    opacity: 0.8,
  },
  filledButtonText: {
    color: '#FFFFFF',
    fontSize: 17,
    fontWeight: '600',
  },
});
```

#### Flutter

```dart
// Filled button
CupertinoButton.filled(
  child: Text('Continue'),
  onPressed: () {},
);

// Tinted button
CupertinoButton(
  color: CupertinoColors.systemBlue.withOpacity(0.1),
  child: Text('Secondary', style: TextStyle(color: CupertinoColors.systemBlue)),
  onPressed: () {},
);

// Plain button
CupertinoButton(
  child: Text('Skip'),
  onPressed: () {},
);

// Destructive
CupertinoButton(
  color: CupertinoColors.systemRed,
  child: Text('Delete'),
  onPressed: () {},
);

// Custom with minimum touch target
GestureDetector(
  onTap: () {},
  child: Container(
    constraints: BoxConstraints(minWidth: 44, minHeight: 44),
    child: // Button content
  ),
);
```

### Form Controls

#### TextField (SwiftUI)

```swift
struct LoginForm: View {
    @State private var email = ""
    @State private var password = ""

    var body: some View {
        Form {
            Section {
                TextField("Email", text: $email)
                    .textContentType(.emailAddress)
                    .keyboardType(.emailAddress)
                    .autocapitalization(.none)

                SecureField("Password", text: $password)
                    .textContentType(.password)
            }
        }
    }
}

// With validation
TextField("Email", text: $email)
    .textFieldStyle(.roundedBorder)
    .overlay(
        RoundedRectangle(cornerRadius: 8)
            .stroke(isValid ? .clear : .red, lineWidth: 1)
    )
```

#### TextField (React Native)

```tsx
import { TextInput, StyleSheet } from 'react-native';

const FormInput = ({ label, error, ...props }) => (
  <View>
    <Text style={styles.label}>{label}</Text>
    <TextInput
      style={[styles.input, error && styles.inputError]}
      placeholderTextColor="#8E8E93"
      {...props}
    />
    {error && <Text style={styles.errorText}>{error}</Text>}
  </View>
);

const styles = StyleSheet.create({
  label: {
    fontSize: 13,
    color: '#8E8E93',
    marginBottom: 4,
  },
  input: {
    height: 44,
    borderRadius: 10,
    backgroundColor: '#F2F2F7',
    paddingHorizontal: 16,
    fontSize: 17,
  },
  inputError: {
    borderWidth: 1,
    borderColor: '#FF3B30',
  },
  errorText: {
    fontSize: 13,
    color: '#FF3B30',
    marginTop: 4,
  },
});
```

#### TextField (Flutter)

```dart
CupertinoTextField(
  placeholder: 'Email',
  keyboardType: TextInputType.emailAddress,
  autocorrect: false,
  prefix: Padding(
    padding: EdgeInsets.only(left: 16),
    child: Icon(CupertinoIcons.mail),
  ),
  decoration: BoxDecoration(
    color: CupertinoColors.systemGrey6,
    borderRadius: BorderRadius.circular(10),
  ),
);

// Password field
CupertinoTextField(
  placeholder: 'Password',
  obscureText: true,
  suffix: CupertinoButton(
    padding: EdgeInsets.only(right: 16),
    child: Icon(CupertinoIcons.eye),
    onPressed: () {},
  ),
);
```

### Lists

#### SwiftUI

```swift
List {
    Section("Account") {
        NavigationLink("Profile") {
            ProfileView()
        }

        NavigationLink("Notifications") {
            NotificationsView()
        }
    }

    Section("Preferences") {
        Toggle("Dark Mode", isOn: $isDarkMode)

        Picker("Language", selection: $language) {
            Text("English").tag("en")
            Text("Spanish").tag("es")
        }
    }
}
.listStyle(.insetGrouped)
```

#### React Native

```tsx
import { SectionList, Switch } from 'react-native';

const SettingsList = () => (
  <SectionList
    sections={[
      {
        title: 'Account',
        data: [
          { title: 'Profile', type: 'navigation' },
          { title: 'Notifications', type: 'navigation' },
        ],
      },
      {
        title: 'Preferences',
        data: [
          { title: 'Dark Mode', type: 'toggle', value: isDarkMode },
        ],
      },
    ]}
    renderItem={({ item }) => (
      <Pressable style={styles.row}>
        <Text style={styles.rowTitle}>{item.title}</Text>
        {item.type === 'navigation' && <ChevronRight />}
        {item.type === 'toggle' && <Switch value={item.value} />}
      </Pressable>
    )}
    renderSectionHeader={({ section }) => (
      <Text style={styles.sectionHeader}>{section.title}</Text>
    )}
    stickySectionHeadersEnabled={false}
  />
);
```

#### Flutter

```dart
CupertinoListSection.insetGrouped(
  header: Text('Account'),
  children: [
    CupertinoListTile(
      title: Text('Profile'),
      trailing: CupertinoListTileChevron(),
      onTap: () {},
    ),
    CupertinoListTile(
      title: Text('Notifications'),
      trailing: CupertinoListTileChevron(),
      onTap: () {},
    ),
  ],
);

CupertinoListSection.insetGrouped(
  header: Text('Preferences'),
  children: [
    CupertinoListTile(
      title: Text('Dark Mode'),
      trailing: CupertinoSwitch(
        value: isDarkMode,
        onChanged: (value) {},
      ),
    ),
  ],
);
```

---

## Touch Targets & Gestures

### Minimum Touch Target: 44x44pt
All interactive elements must have a minimum tappable area of 44x44 points, even if the visual element is smaller.

### Standard Gestures
- **Tap**: Primary action
- **Long press**: Secondary actions, context menus
- **Swipe**: Navigation, delete, actions
- **Pinch**: Zoom
- **Rotation**: Rotate content
- **Pan/Drag**: Move, scroll

### Haptic Feedback

#### SwiftUI

```swift
// Impact feedback
let impact = UIImpactFeedbackGenerator(style: .medium)
impact.impactOccurred()

// Selection feedback
let selection = UISelectionFeedbackGenerator()
selection.selectionChanged()

// Notification feedback
let notification = UINotificationFeedbackGenerator()
notification.notificationOccurred(.success) // .warning, .error

// Sensory feedback (iOS 17+)
Button("Tap") { }
    .sensoryFeedback(.impact(weight: .medium), trigger: tapped)
```

#### React Native

```tsx
import * as Haptics from 'expo-haptics';

// Impact
Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium);

// Selection
Haptics.selectionAsync();

// Notification
Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success);
```

#### Flutter

```dart
import 'package:flutter/services.dart';

// Light impact
HapticFeedback.lightImpact();

// Medium impact
HapticFeedback.mediumImpact();

// Heavy impact
HapticFeedback.heavyImpact();

// Selection click
HapticFeedback.selectionClick();
```

---

## Accessibility

### VoiceOver Support

#### SwiftUI

```swift
Image(systemName: "heart.fill")
    .accessibilityLabel("Favorite")
    .accessibilityHint("Double tap to add to favorites")
    .accessibilityAddTraits(.isButton)

// Combine elements
HStack {
    Image(systemName: "star.fill")
    Text("4.5")
}
.accessibilityElement(children: .combine)
.accessibilityLabel("Rating: 4.5 stars")

// Hide decorative elements
Image("decorative-pattern")
    .accessibilityHidden(true)
```

#### React Native

```tsx
<Pressable
  accessible={true}
  accessibilityLabel="Favorite"
  accessibilityHint="Double tap to add to favorites"
  accessibilityRole="button"
>
  <HeartIcon />
</Pressable>

// Hide decorative
<Image
  source={decorativeImage}
  accessibilityElementsHidden={true}
/>
```

#### Flutter

```dart
Semantics(
  label: 'Favorite',
  hint: 'Double tap to add to favorites',
  button: true,
  child: Icon(CupertinoIcons.heart_fill),
);

// Hide decorative
ExcludeSemantics(
  child: Image.asset('decorative.png'),
);
```

### Dynamic Type

Always support Dynamic Type to allow users to scale text.

#### SwiftUI

```swift
// Automatic with semantic fonts
Text("Body")
    .font(.body)

// Limit maximum scaling if needed
Text("Limited")
    .font(.body)
    .dynamicTypeSize(...DynamicTypeSize.xxxLarge)

// Check current size class
@Environment(\.dynamicTypeSize) var dynamicTypeSize

if dynamicTypeSize >= .accessibility1 {
    // Use larger touch targets, simplified layout
}
```

#### React Native

```tsx
<Text
  style={styles.body}
  allowFontScaling={true}
  maxFontSizeMultiplier={2.0}
>
  Accessible text
</Text>
```

#### Flutter

```dart
Text(
  'Body',
  style: TextStyle(fontSize: 17),
  textScaleFactor: MediaQuery.textScaleFactorOf(context),
);
```

### Color Contrast

Minimum contrast ratios:
- **Normal text**: 4.5:1
- **Large text (18pt+ or 14pt bold)**: 3:1
- **UI components**: 3:1

### Reduce Motion

Respect user's motion preferences.

#### SwiftUI

```swift
@Environment(\.accessibilityReduceMotion) var reduceMotion

withAnimation(reduceMotion ? nil : .spring()) {
    // Animate
}
```

#### React Native

```tsx
import { AccessibilityInfo } from 'react-native';

const [reduceMotion, setReduceMotion] = useState(false);

useEffect(() => {
  AccessibilityInfo.isReduceMotionEnabled().then(setReduceMotion);
}, []);

// Use in animations
Animated.timing(value, {
  duration: reduceMotion ? 0 : 300,
  useNativeDriver: true,
});
```

#### Flutter

```dart
final reduceMotion = MediaQuery.of(context).disableAnimations;

AnimatedContainer(
  duration: reduceMotion ? Duration.zero : Duration(milliseconds: 300),
  // ...
);
```

---

## Animation & Motion

### Principles
- **Purpose**: Animation should have meaning and guide attention
- **Responsiveness**: Immediate response to user input
- **Continuity**: Maintain context during transitions
- **Subtlety**: Avoid excessive or distracting motion

### Spring Animations

#### SwiftUI

```swift
// Default spring
withAnimation(.spring()) {
    isExpanded.toggle()
}

// Custom spring
withAnimation(.spring(response: 0.4, dampingFraction: 0.7)) {
    // Animate
}

// Bouncy
withAnimation(.spring(response: 0.5, dampingFraction: 0.5, blendDuration: 0.5)) {
    // Bouncy animation
}

// Smooth (no bounce)
withAnimation(.smooth) {
    // Smooth transition
}
```

#### React Native

```tsx
import Animated, { withSpring, withTiming } from 'react-native-reanimated';

// Spring animation
const animatedStyle = useAnimatedStyle(() => ({
  transform: [
    {
      scale: withSpring(isPressed ? 0.95 : 1, {
        damping: 15,
        stiffness: 150,
      }),
    },
  ],
}));

// Timing (for non-spring)
withTiming(targetValue, {
  duration: 300,
  easing: Easing.bezier(0.25, 0.1, 0.25, 1),
});
```

#### Flutter

```dart
// Spring simulation
final spring = SpringSimulation(
  SpringDescription(
    mass: 1,
    stiffness: 150,
    damping: 15,
  ),
  0, // start
  1, // end
  0, // velocity
);

// Curve-based
AnimatedContainer(
  duration: Duration(milliseconds: 300),
  curve: Curves.easeInOut,
  // ...
);

// iOS-style curves
curve: Curves.easeInOutCubicEmphasized // Similar to iOS spring
```

### Page Transitions

#### SwiftUI

```swift
NavigationStack {
    // Automatic iOS transitions
}

// Custom transition
.transition(.asymmetric(
    insertion: .move(edge: .trailing),
    removal: .move(edge: .leading)
))

// Matched geometry for shared element
@Namespace var animation

Image("photo")
    .matchedGeometryEffect(id: "photo", in: animation)
```

#### React Native

```tsx
// Native stack navigator uses native transitions
import { createNativeStackNavigator } from '@react-navigation/native-stack';

// Shared element transitions
import { SharedElement } from 'react-navigation-shared-element';

<SharedElement id="photo">
  <Image source={photo} />
</SharedElement>
```

#### Flutter

```dart
// Hero animation (shared element)
Hero(
  tag: 'photo',
  child: Image.asset('photo.png'),
);

// Page route with Cupertino transition
CupertinoPageRoute(
  builder: (context) => DetailScreen(),
);

// Custom page transition
PageRouteBuilder(
  pageBuilder: (context, animation, secondaryAnimation) => Screen(),
  transitionsBuilder: (context, animation, secondaryAnimation, child) {
    return SlideTransition(
      position: Tween<Offset>(
        begin: Offset(1, 0),
        end: Offset.zero,
      ).animate(CurvedAnimation(
        parent: animation,
        curve: Curves.easeInOutCubicEmphasized,
      )),
      child: child,
    );
  },
);
```

---

## SF Symbols

Apple's icon library with 6,000+ symbols. Use them instead of custom icons when possible.

### Symbol Variants
- **Outline**: Default, for toolbars and navigation
- **Fill**: Selected states, emphasis
- **Slash**: Disabled or unavailable
- **Badge**: Notifications, counts

### Rendering Modes
- **Monochrome**: Single color
- **Hierarchical**: Primary/secondary emphasis
- **Palette**: Multiple custom colors
- **Multicolor**: System-defined colors

### SwiftUI

```swift
// Basic
Image(systemName: "heart")

// Filled variant
Image(systemName: "heart.fill")

// With rendering mode
Image(systemName: "heart.fill")
    .symbolRenderingMode(.hierarchical)
    .foregroundStyle(.red)

// Variable value (0-1)
Image(systemName: "speaker.wave.3.fill", variableValue: 0.7)

// Symbol effect (iOS 17+)
Image(systemName: "heart.fill")
    .symbolEffect(.bounce, value: isFavorite)
```

### React Native

```tsx
// Use SF Symbols via native module or react-native-sfsymbols
import SFSymbol from 'react-native-sfsymbols';

<SFSymbol
  name="heart.fill"
  size={24}
  color="#FF3B30"
  renderingMode="hierarchical"
/>
```

### Flutter

```dart
// Use cupertino_icons or sf_symbols_flutter
import 'package:flutter/cupertino.dart';

Icon(
  CupertinoIcons.heart_fill,
  size: 24,
  color: CupertinoColors.systemRed,
);
```

---

## Screen Sizes Reference

### iPhone (points)

| Device | Width | Height | Safe Area Top | Safe Area Bottom |
|--------|-------|--------|---------------|------------------|
| iPhone 16 Pro Max | 440 | 956 | 59 | 34 |
| iPhone 16 Pro | 402 | 874 | 59 | 34 |
| iPhone 16/15/14 | 393 | 852 | 59 | 34 |
| iPhone SE (3rd) | 375 | 667 | 20 | 0 |

### iPad (points)

| Device | Width | Height |
|--------|-------|--------|
| iPad Pro 12.9" | 1024 | 1366 |
| iPad Pro 11" | 834 | 1194 |
| iPad Air | 820 | 1180 |
| iPad Mini | 744 | 1133 |

---

## Best Practices Checklist

### Design
- [ ] Follow Clarity, Deference, Depth principles
- [ ] Use Liquid Glass for modern iOS 26+ feel
- [ ] Implement semantic colors for dark/light mode
- [ ] Use SF Pro and Dynamic Type scales
- [ ] Follow 8-point grid for spacing
- [ ] Design for both iPhone and iPad

### Components
- [ ] Use native components when possible
- [ ] Minimum 44x44pt touch targets
- [ ] Consistent navigation patterns
- [ ] Proper keyboard handling
- [ ] Safe area respect

### Accessibility
- [ ] VoiceOver labels for all interactive elements
- [ ] Dynamic Type support
- [ ] Sufficient color contrast (4.5:1 for text)
- [ ] Reduce Motion support
- [ ] Increase Contrast support

### Performance
- [ ] Smooth 60fps animations
- [ ] Lazy loading for lists
- [ ] Efficient image handling
- [ ] Minimal layout recalculations

### Testing
- [ ] Test on multiple device sizes
- [ ] Test in Light and Dark modes
- [ ] Test with VoiceOver enabled
- [ ] Test with Dynamic Type at various sizes
- [ ] Test with Reduce Motion enabled
