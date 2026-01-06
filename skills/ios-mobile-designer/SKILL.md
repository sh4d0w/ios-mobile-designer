---
name: ios-mobile-designer
description: Design iOS apps following Apple's Human Interface Guidelines with Liquid Glass (iOS 26). Generate native components, ensure accessibility, and create stunning interfaces for iPhone & iPad.
---

# iOS Mobile App Designer

Design professional iOS applications following Apple Human Interface Guidelines (HIG) with support for the new Liquid Glass design language (iOS 26). This skill enables creation of native, accessible, and visually stunning interfaces for iPhone and iPad using SwiftUI, React Native, or Flutter.

---

## Design Philosophy: Beyond Specifications

> "Following HIG makes your app feel native. Breaking rules intentionally makes it memorable."
> — The difference between good apps and Flighty-level apps

### The Intentionality Principle
Every design decision must be **intentional**, not default. Before using any component or pattern, ask:
- Why this element and not another?
- What emotion should this screen evoke?
- How does this reinforce the app's personality?

### The Flighty Principle: Data as Art

> "Pack it, wrap it, put some color on it." — Flighty Design Team

Premium apps don't just display data—they **transform** it into visual stories:

**Progressive Disclosure**
- Show **20%** of data by default (the essential)
- Reveal **80%** on interaction (the detailed)
- Surface should feel "calm and simple"
- Depth available on demand

**Information Hierarchy**
```
┌─────────────────────────────────┐
│      PRIMARY METRIC             │  ← Largest, centered
│         (42°F)                  │
├─────────────────────────────────┤
│  Supporting    │    Context     │  ← Smaller, peripheral
│   (Feels 38°)  │   (Humidity)   │
├─────────────────────────────────┤
│     Tap for detailed forecast   │  ← On-demand depth
└─────────────────────────────────┘
```

**What Flighty Does Differently:**
- Prioritizes data above the fold ruthlessly
- Uses airport signage conventions (familiar visual language)
- Color encodes meaning (green = on time, red = delayed)
- Animation reveals data progressively, not all at once

### Physical Metaphor Grounding

Every gesture should echo a real-world interaction. This creates intuitive, memorable UX.

| App | Physical Metaphor | Implementation |
|-----|-------------------|----------------|
| Halide | Film camera | Swipe up/down = exposure dial |
| Things 3 | Paper checklist | Dragging creates spatial meaning |
| Apple Books | Physical book | Page curl, weight, texture |
| Wallet | Leather wallet | Card stack, peek, slide |

**How to Apply:**
1. Identify your app's real-world equivalent
2. Study how people interact with that object
3. Map gestures to those physical actions
4. Add appropriate haptic feedback (tactile confirmation)

```swift
// Camera-inspired exposure control (Halide-style)
DragGesture(minimumDistance: 0)
    .onChanged { value in
        // Vertical drag = exposure adjustment
        let delta = value.translation.height / 200
        exposure = max(-2, min(2, exposure - delta))

        // Haptic detent at neutral (0)
        if abs(exposure) < 0.1 && previousExposure >= 0.1 {
            UIImpactFeedbackGenerator(style: .light).impactOccurred()
        }
    }
```

### Anticipatory Feedback

Premium apps prepare users for what's coming **before** it happens.

**Haptics on Press, Not Release:**
```swift
Button(action: performAction) {
    Label("Delete", systemImage: "trash")
}
.simultaneousGesture(
    DragGesture(minimumDistance: 0)
        .onChanged { _ in
            // Haptic BEFORE action completes
            UIImpactFeedbackGenerator(style: .medium).impactOccurred()
        }
)
```

**Visual Preparation:**
- Button scales down slightly on press (confirms touch registered)
- Destructive actions show preview of consequence
- Loading states appear instantly (not after delay)

**The Apple Pay Pattern:**
1. Face ID begins → Subtle haptic
2. Authentication succeeds → Success haptic
3. Payment processes → Reassuring vibration pattern
4. Complete → Visual + haptic confirmation

User feels confident at every step because feedback **anticipates** the next state.

### Delight Budget

Allocate **5-10%** of design effort to unexpected moments of joy.

**Types of Delight:**
| Type | Example | When to Use |
|------|---------|-------------|
| Seasonal | Carrot Weather's garden changes monthly | Repeat users notice |
| Achievement | Confetti on goal completion | Milestone moments |
| Easter Egg | Hidden animations on specific gestures | Power users discover |
| Personality | Unique empty states with character | First impressions |
| Sound | Satisfying "ding" on success | Task completion |

**Rules for Delight:**
- Never interrupt workflow
- Respect Reduce Motion settings
- Make it discoverable, not mandatory
- Consistent with app personality

```swift
// Celebration on achievement (respects Reduce Motion)
func celebrate() {
    let generator = UINotificationFeedbackGenerator()
    generator.notificationOccurred(.success)

    if !UIAccessibility.isReduceMotionEnabled {
        withAnimation(.spring(response: 0.6, dampingFraction: 0.7)) {
            showConfetti = true
        }
    }
}
```

### Constraint-Based Design

> "Limitations breed innovation." — Things 3 Design Philosophy

The best designs emerge from **intentional constraints**:

**One-Handed Design:**
- All primary actions reachable with thumb
- Critical buttons in bottom 1/3 of screen
- Swipe gestures for common actions

**The Magic Button Pattern (Things 3):**
Instead of modal dialogs for "where to create?":
- Tap = create in current context
- Drag = position determines context
- One interaction, infinite possibilities

```swift
// Magic button: tap or drag to create
struct MagicAddButton: View {
    @State private var dragLocation: CGPoint?

    var body: some View {
        Image(systemName: "plus.circle.fill")
            .gesture(
                DragGesture(minimumDistance: 0)
                    .onChanged { value in
                        dragLocation = value.location
                        // Show drop zones as user drags
                    }
                    .onEnded { value in
                        if let zone = dropZone(at: value.location) {
                            createItem(in: zone)
                        } else {
                            createItem(in: .current)
                        }
                    }
            )
            .onTapGesture {
                createItem(in: .current)
            }
    }
}
```

**Information Density Constraint:**
- Each screen answers ONE primary question
- Secondary info available but not competing
- If you need a legend, simplify the visualization

### Creating Distinctive Apps Within HIG

HIG provides the grammar; your app provides the voice. Ways to stand out:

1. **Custom Color Palettes**: Don't just use systemBlue. Create a distinctive accent that becomes your signature while maintaining semantic colors for system UI.

2. **Typographic Personality**: SF Pro is the base, but weight, tracking, and case variations create character:
   - ALL CAPS with wide tracking → Premium/Luxury
   - Rounded design with medium weight → Friendly/Approachable
   - Tight tracking, bold → Technical/Precise

3. **Meaningful Motion**: Don't animate for animation's sake. Each motion should:
   - Provide feedback (response to touch)
   - Guide attention (what to look at next)
   - Express personality (bouncy = playful, smooth = sophisticated)

4. **Signature Interactions**: One memorable micro-interaction users associate with your app:
   - Pull-to-refresh with custom animation
   - Unique haptic patterns
   - Distinctive transition between screens

### Anti-Patterns: What NOT to Do

❌ **Generic AI Design Symptoms:**
- Using every Liquid Glass effect on every surface
- Identical spacing everywhere (no visual rhythm)
- Stock SF Symbols without customization
- Blue accent color for everything
- No empty states or loading states
- Identical corner radius on all elements
- Timer-based animations (not gesture-responsive)
- Haptics on release instead of press
- All data shown at once (no progressive disclosure)
- Same animation duration everywhere

✅ **Premium App Patterns:**
- Glass effects on 2-3 key surfaces max
- Varying spacing creates rhythm (tight grouping = related, wide = separate)
- Customize SF Symbol colors, weights, and rendering modes
- Choose accent colors that match brand personality
- Design every state: empty, loading, error, success, offline
- Corner radius varies: smaller for nested elements, larger for containers
- Animation velocity matches gesture velocity
- Anticipatory haptics guide users
- 20% data surface, 80% on-demand
- Animation duration matches importance (quick for minor, slower for major)

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

## Live Activities & Dynamic Island

> "Flighty won Apple's Design Award for Live Activities." — This is the premium iOS differentiator.

Live Activities let your app present real-time information on the Lock Screen and Dynamic Island without users opening the app.

### When to Use Live Activities

| Use Case | Good Fit | Bad Fit |
|----------|----------|---------|
| Flight tracking | ✅ Real-time status | |
| Food delivery | ✅ Order progress | |
| Sports scores | ✅ Live game updates | |
| Workout | ✅ Active exercise | |
| Weather alerts | ✅ Time-sensitive | |
| Social media | | ❌ Not time-sensitive |
| News | | ❌ Not urgent |
| Shopping | | ❌ Unless delivery |

### Dynamic Island States

```
┌──────────────────────────────────────────────┐
│                                              │
│   ●──────●      Compact (default)            │
│   Leading Trailing                           │
│                                              │
├──────────────────────────────────────────────┤
│                                              │
│   ┌─────────────────────┐                    │
│   │   Expanded View      │  Long press       │
│   │   More details       │                   │
│   └─────────────────────┘                    │
│                                              │
├──────────────────────────────────────────────┤
│                                              │
│        ●        Minimal (multiple)           │
│                 When several activities      │
│                                              │
└──────────────────────────────────────────────┘
```

### Design Principles (Flighty-style)

1. **Glanceability**: Users spend <1 second looking
2. **Information Hierarchy**: Most critical info visible in compact
3. **Familiar Conventions**: Use industry signage (airport boards, scoreboards)
4. **Color as Data**: Green = good, Red = problem, Yellow = attention
5. **Progress Visualization**: Rings, bars, or percentage—pick one

### SwiftUI Implementation

```swift
import ActivityKit
import WidgetKit

// MARK: - Activity Attributes
struct FlightActivityAttributes: ActivityAttributes {
    // Static data (doesn't change)
    let flightNumber: String
    let departure: String
    let arrival: String
    let airline: String

    // Dynamic data (updates)
    struct ContentState: Codable, Hashable {
        let progress: Double  // 0.0 to 1.0
        let status: FlightStatus
        let gate: String?
        let departureTime: Date
        let arrivalTime: Date
        let delay: Int?  // minutes
    }
}

enum FlightStatus: String, Codable {
    case scheduled, boarding, departed, enRoute, landed, delayed, cancelled
}

// MARK: - Live Activity Widget
struct FlightLiveActivity: Widget {
    var body: some WidgetConfiguration {
        ActivityConfiguration(for: FlightActivityAttributes.self) { context in
            // Lock Screen view
            LockScreenView(context: context)
        } dynamicIsland: { context in
            DynamicIsland {
                // Expanded view (long press)
                DynamicIslandExpandedRegion(.leading) {
                    VStack(alignment: .leading) {
                        Text(context.attributes.departure)
                            .font(.headline)
                        Text(context.state.departureTime, style: .time)
                            .font(.caption)
                    }
                }
                DynamicIslandExpandedRegion(.trailing) {
                    VStack(alignment: .trailing) {
                        Text(context.attributes.arrival)
                            .font(.headline)
                        Text(context.state.arrivalTime, style: .time)
                            .font(.caption)
                    }
                }
                DynamicIslandExpandedRegion(.center) {
                    FlightProgressBar(progress: context.state.progress)
                }
                DynamicIslandExpandedRegion(.bottom) {
                    HStack {
                        StatusBadge(status: context.state.status)
                        Spacer()
                        if let gate = context.state.gate {
                            Text("Gate \(gate)")
                                .font(.caption)
                        }
                    }
                }
            } compactLeading: {
                // Compact leading (always visible)
                Image(systemName: "airplane")
                    .foregroundStyle(statusColor(context.state.status))
            } compactTrailing: {
                // Compact trailing (always visible)
                Text(context.state.arrivalTime, style: .timer)
                    .font(.caption2)
                    .monospacedDigit()
            } minimal: {
                // Minimal (when multiple activities)
                Image(systemName: "airplane.circle.fill")
                    .foregroundStyle(statusColor(context.state.status))
            }
        }
    }

    func statusColor(_ status: FlightStatus) -> Color {
        switch status {
        case .scheduled, .boarding: return .blue
        case .departed, .enRoute: return .green
        case .landed: return .green
        case .delayed: return .orange
        case .cancelled: return .red
        }
    }
}

// MARK: - Lock Screen View
struct LockScreenView: View {
    let context: ActivityViewContext<FlightActivityAttributes>

    var body: some View {
        HStack(spacing: 16) {
            // Departure
            VStack(alignment: .leading) {
                Text(context.attributes.departure)
                    .font(.title2.bold())
                Text(context.state.departureTime, style: .time)
                    .font(.caption)
                    .foregroundStyle(.secondary)
            }

            // Progress
            VStack {
                FlightProgressBar(progress: context.state.progress)
                    .frame(height: 4)
                Image(systemName: "airplane")
                    .font(.caption)
            }
            .frame(maxWidth: .infinity)

            // Arrival
            VStack(alignment: .trailing) {
                Text(context.attributes.arrival)
                    .font(.title2.bold())
                Text(context.state.arrivalTime, style: .time)
                    .font(.caption)
                    .foregroundStyle(.secondary)
            }
        }
        .padding()
        .background(.ultraThinMaterial)
    }
}

// MARK: - Progress Bar (Flighty-style)
struct FlightProgressBar: View {
    let progress: Double

    var body: some View {
        GeometryReader { geo in
            ZStack(alignment: .leading) {
                // Track
                Capsule()
                    .fill(.quaternary)

                // Progress
                Capsule()
                    .fill(.blue.gradient)
                    .frame(width: geo.size.width * progress)

                // Airplane indicator
                Image(systemName: "airplane")
                    .font(.system(size: 10))
                    .offset(x: geo.size.width * progress - 5)
            }
        }
    }
}

// MARK: - Starting/Updating Activity
class FlightActivityManager {
    static let shared = FlightActivityManager()
    private var currentActivity: Activity<FlightActivityAttributes>?

    func startTracking(flight: Flight) async throws {
        let attributes = FlightActivityAttributes(
            flightNumber: flight.number,
            departure: flight.departure,
            arrival: flight.arrival,
            airline: flight.airline
        )

        let initialState = FlightActivityAttributes.ContentState(
            progress: 0,
            status: .scheduled,
            gate: flight.gate,
            departureTime: flight.departureTime,
            arrivalTime: flight.arrivalTime,
            delay: nil
        )

        let content = ActivityContent(state: initialState, staleDate: nil)

        currentActivity = try Activity.request(
            attributes: attributes,
            content: content,
            pushType: .token  // For remote updates
        )
    }

    func updateStatus(_ state: FlightActivityAttributes.ContentState) async {
        let content = ActivityContent(state: state, staleDate: nil)
        await currentActivity?.update(content)
    }

    func endActivity() async {
        await currentActivity?.end(nil, dismissalPolicy: .immediate)
    }
}
```

### React Native Implementation

```tsx
// Note: Live Activities require native Swift code
// Use a bridge module for React Native

// Native Module (Swift)
// FlightActivityModule.swift
import ActivityKit

@objc(FlightActivityModule)
class FlightActivityModule: NSObject {
    @objc
    func startActivity(
        _ flightNumber: String,
        departure: String,
        arrival: String,
        resolver: @escaping RCTPromiseResolveBlock,
        rejecter: @escaping RCTPromiseRejectBlock
    ) {
        Task {
            do {
                // Start activity using native code
                let attributes = FlightActivityAttributes(
                    flightNumber: flightNumber,
                    departure: departure,
                    arrival: arrival,
                    airline: ""
                )
                // ... implementation
                resolver(["success": true])
            } catch {
                rejecter("ERROR", error.localizedDescription, error)
            }
        }
    }
}

// React Native usage
import { NativeModules } from 'react-native';

const { FlightActivityModule } = NativeModules;

const startFlightTracking = async (flight) => {
    try {
        await FlightActivityModule.startActivity(
            flight.number,
            flight.departure,
            flight.arrival
        );
    } catch (error) {
        console.error('Failed to start Live Activity:', error);
    }
};
```

### Flutter Implementation

```dart
// Live Activities require platform channels to native Swift code

// Method Channel setup
import 'package:flutter/services.dart';

class LiveActivityService {
    static const _channel = MethodChannel('com.app/live_activity');

    static Future<bool> startFlightActivity({
        required String flightNumber,
        required String departure,
        required String arrival,
        required DateTime departureTime,
        required DateTime arrivalTime,
    }) async {
        try {
            final result = await _channel.invokeMethod('startFlightActivity', {
                'flightNumber': flightNumber,
                'departure': departure,
                'arrival': arrival,
                'departureTime': departureTime.toIso8601String(),
                'arrivalTime': arrivalTime.toIso8601String(),
            });
            return result == true;
        } catch (e) {
            print('Failed to start Live Activity: $e');
            return false;
        }
    }

    static Future<void> updateActivity({
        required double progress,
        required String status,
        String? gate,
    }) async {
        await _channel.invokeMethod('updateActivity', {
            'progress': progress,
            'status': status,
            'gate': gate,
        });
    }

    static Future<void> endActivity() async {
        await _channel.invokeMethod('endActivity');
    }
}

// Native Swift handler (AppDelegate.swift)
/*
@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
    override func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
    ) -> Bool {
        let controller = window?.rootViewController as! FlutterViewController
        let channel = FlutterMethodChannel(
            name: "com.app/live_activity",
            binaryMessenger: controller.binaryMessenger
        )

        channel.setMethodCallHandler { call, result in
            switch call.method {
            case "startFlightActivity":
                // Handle start
            case "updateActivity":
                // Handle update
            case "endActivity":
                // Handle end
            default:
                result(FlutterMethodNotImplemented)
            }
        }

        return super.application(application, didFinishLaunchingWithOptions: launchOptions)
    }
}
*/
```

### Best Practices

1. **Keep it Simple**: Compact view should be understandable in <1 second
2. **Update Sparingly**: Don't update more than once per minute unless critical
3. **Use Push Updates**: For real-time data, configure push notifications for activities
4. **Respect Battery**: End activities when no longer relevant
5. **Test All States**: Compact, expanded, minimal, and Lock Screen
6. **Accessibility**: Ensure all text is readable with Dynamic Type

---

## Widgets

> "The best widget is one you never have to open the app for."

Widgets extend your app to the Home Screen, Lock Screen, and StandBy mode—providing glanceable information at a glance.

### Widget Families & Sizes

| Family | Size (pt) | Content Capacity | Best For |
|--------|-----------|------------------|----------|
| `systemSmall` | 169×169 | 1-2 data points | Quick status |
| `systemMedium` | 360×169 | 3-4 data points | Moderate detail |
| `systemLarge` | 360×376 | Full content | Rich information |
| `systemExtraLarge` | iPad only | Maximum | Dashboards |
| `accessoryCircular` | Lock Screen | Icon + number | Minimal glance |
| `accessoryRectangular` | Lock Screen | 2-3 lines | Brief status |
| `accessoryInline` | Lock Screen | Single line | Minimal text |

### Design Principles

1. **Glanceability**: User spends <3 seconds looking
2. **Single Purpose**: Each widget answers ONE question
3. **Tap Target**: Entire widget is tappable (deep link)
4. **Timeliness**: Show relevant info for NOW
5. **Personalization**: Support multiple configurations

### Information Density by Size

```
┌─────────────────┐
│  systemSmall    │
│    42°          │  ← One hero metric
│   Sunny         │  ← Supporting context
└─────────────────┘

┌───────────────────────────────────┐
│  systemMedium                     │
│    42°F         Today's High: 48° │
│   Sunny         Rain at 3pm       │
│   ☀️ ☀️ 🌧️ ☀️ ☀️ ☀️              │
└───────────────────────────────────┘

┌───────────────────────────────────┐
│  systemLarge                      │
│    San Francisco                  │
│    42°F  Sunny                    │
│                                   │
│    Hourly: ☀️ ☀️ 🌧️ ☀️ ☀️ ☀️ ☀️ ☀️ │
│            9  10 11 12 1  2  3  4 │
│                                   │
│    Weekly Forecast                │
│    Mon  48°/32°  Sunny            │
│    Tue  52°/35°  Cloudy           │
│    Wed  45°/30°  Rain             │
└───────────────────────────────────┘
```

### SwiftUI Implementation

```swift
import WidgetKit
import SwiftUI

// MARK: - Widget Configuration
struct WeatherWidget: Widget {
    let kind: String = "WeatherWidget"

    var body: some WidgetConfiguration {
        // Static widget (no user configuration)
        StaticConfiguration(kind: kind, provider: WeatherProvider()) { entry in
            WeatherWidgetView(entry: entry)
                .containerBackground(.fill.tertiary, for: .widget)
        }
        .configurationDisplayName("Weather")
        .description("Current weather conditions.")
        .supportedFamilies([
            .systemSmall,
            .systemMedium,
            .systemLarge,
            .accessoryCircular,
            .accessoryRectangular,
            .accessoryInline
        ])
    }
}

// Configurable widget (with user options)
struct ConfigurableWeatherWidget: Widget {
    var body: some WidgetConfiguration {
        AppIntentConfiguration(
            kind: "ConfigurableWeather",
            intent: WeatherIntent.self,
            provider: WeatherProvider()
        ) { entry in
            WeatherWidgetView(entry: entry)
        }
        .configurationDisplayName("Weather")
        .description("Choose your location.")
        .supportedFamilies([.systemSmall, .systemMedium, .systemLarge])
    }
}

// MARK: - Timeline Provider
struct WeatherProvider: TimelineProvider {
    func placeholder(in context: Context) -> WeatherEntry {
        WeatherEntry(date: Date(), temperature: 72, condition: .sunny, location: "San Francisco")
    }

    func getSnapshot(in context: Context, completion: @escaping (WeatherEntry) -> Void) {
        // For widget gallery preview
        completion(placeholder(in: context))
    }

    func getTimeline(in context: Context, completion: @escaping (Timeline<WeatherEntry>) -> Void) {
        Task {
            let weather = await fetchWeather()
            let entry = WeatherEntry(
                date: Date(),
                temperature: weather.temperature,
                condition: weather.condition,
                location: weather.location
            )

            // Update every 30 minutes
            let nextUpdate = Calendar.current.date(byAdding: .minute, value: 30, to: Date())!
            let timeline = Timeline(entries: [entry], policy: .after(nextUpdate))
            completion(timeline)
        }
    }
}

// MARK: - Entry
struct WeatherEntry: TimelineEntry {
    let date: Date
    let temperature: Int
    let condition: WeatherCondition
    let location: String
}

// MARK: - Widget Views (Size-Adaptive)
struct WeatherWidgetView: View {
    @Environment(\.widgetFamily) var family
    let entry: WeatherEntry

    var body: some View {
        switch family {
        case .systemSmall:
            SmallWeatherView(entry: entry)
        case .systemMedium:
            MediumWeatherView(entry: entry)
        case .systemLarge:
            LargeWeatherView(entry: entry)
        case .accessoryCircular:
            CircularWeatherView(entry: entry)
        case .accessoryRectangular:
            RectangularWeatherView(entry: entry)
        case .accessoryInline:
            InlineWeatherView(entry: entry)
        default:
            SmallWeatherView(entry: entry)
        }
    }
}

// Small widget
struct SmallWeatherView: View {
    let entry: WeatherEntry

    var body: some View {
        VStack(alignment: .leading) {
            Text(entry.location)
                .font(.caption)
                .foregroundStyle(.secondary)

            Spacer()

            Text("\(entry.temperature)°")
                .font(.system(size: 48, weight: .medium, design: .rounded))

            Text(entry.condition.description)
                .font(.subheadline)
        }
        .frame(maxWidth: .infinity, alignment: .leading)
        .widgetURL(URL(string: "weather://location/\(entry.location)"))
    }
}

// Lock Screen circular
struct CircularWeatherView: View {
    let entry: WeatherEntry

    var body: some View {
        ZStack {
            AccessoryWidgetBackground()
            VStack(spacing: 2) {
                Image(systemName: entry.condition.icon)
                    .font(.system(size: 14))
                Text("\(entry.temperature)°")
                    .font(.system(size: 16, weight: .semibold))
            }
        }
    }
}

// Lock Screen rectangular
struct RectangularWeatherView: View {
    let entry: WeatherEntry

    var body: some View {
        HStack {
            Image(systemName: entry.condition.icon)
                .font(.title2)
            VStack(alignment: .leading) {
                Text("\(entry.temperature)°")
                    .font(.headline)
                Text(entry.condition.description)
                    .font(.caption)
            }
        }
    }
}

// Lock Screen inline
struct InlineWeatherView: View {
    let entry: WeatherEntry

    var body: some View {
        HStack {
            Image(systemName: entry.condition.icon)
            Text("\(entry.temperature)° \(entry.condition.description)")
        }
    }
}
```

### Interactive Widgets (iOS 17+)

```swift
import AppIntents

// MARK: - Interactive Button
struct ToggleIntent: AppIntent {
    static var title: LocalizedStringResource = "Toggle Task"

    @Parameter(title: "Task ID")
    var taskId: String

    func perform() async throws -> some IntentResult {
        // Perform action
        TaskManager.shared.toggle(taskId: taskId)
        return .result()
    }
}

// Widget with interactive button
struct TaskWidgetView: View {
    let task: TaskEntry

    var body: some View {
        HStack {
            Button(intent: ToggleIntent(taskId: task.id)) {
                Image(systemName: task.isCompleted ? "checkmark.circle.fill" : "circle")
                    .foregroundStyle(task.isCompleted ? .green : .secondary)
            }
            .buttonStyle(.plain)

            Text(task.title)
                .strikethrough(task.isCompleted)
        }
    }
}
```

### StandBy Mode

```swift
// StandBy-optimized widget
struct StandByWeatherView: View {
    @Environment(\.isLuminanceReduced) var isLuminanceReduced
    let entry: WeatherEntry

    var body: some View {
        VStack {
            Text("\(entry.temperature)°")
                .font(.system(size: 120, weight: .thin, design: .rounded))
                .foregroundStyle(isLuminanceReduced ? .red : .primary)

            Image(systemName: entry.condition.icon)
                .font(.system(size: 48))
        }
        // Reduce brightness when Always On Display
        .opacity(isLuminanceReduced ? 0.4 : 1.0)
    }
}
```

### React Native & Flutter

Widgets require native implementation. Use platform channels:

```tsx
// React Native - widgetkit-manager
import WidgetKit from 'react-native-widgetkit';

// Update widget data
WidgetKit.setItem('weatherData', JSON.stringify({
    temperature: 72,
    condition: 'sunny',
    location: 'San Francisco'
}), 'group.com.app.weather');

// Reload widget
WidgetKit.reloadAllTimelines();
```

```dart
// Flutter - home_widget package
import 'package:home_widget/home_widget.dart';

class WidgetService {
    static const appGroupId = 'group.com.app.weather';

    static Future<void> updateWidget({
        required int temperature,
        required String condition,
    }) async {
        await HomeWidget.saveWidgetData('temperature', temperature);
        await HomeWidget.saveWidgetData('condition', condition);
        await HomeWidget.updateWidget(
            name: 'WeatherWidget',
            iOSName: 'WeatherWidget',
        );
    }
}
```

### Widget Best Practices

| Do | Don't |
|----|-------|
| Update on meaningful data change | Update every second |
| Use semantic colors | Hardcode light/dark colors |
| Deep link to relevant content | Deep link to app home |
| Show current/relevant data | Show stale information |
| Design for all sizes separately | Shrink large design for small |
| Support accessibility | Ignore Dynamic Type |
| Use widget-specific backgrounds | Use solid opaque backgrounds |

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

### Haptic Hierarchy System

> "Every vibration must be earned." — Premium haptic design principle

Haptics are not decoration—they're **communication**. Premium apps use a strategic hierarchy where the weight of feedback matches the importance of the action.

#### The Haptic Hierarchy

| Action Type | Haptic Style | Weight | When to Use |
|-------------|--------------|--------|-------------|
| **Discovery** | Selection | Lightest | Scrolling through options, hovering |
| **Selection** | Light Impact | Light | Choosing an item, toggling |
| **Confirmation** | Medium Impact | Medium | Button press, action confirmed |
| **Success** | Notification Success | Medium+ | Task completed, payment processed |
| **Warning** | Notification Warning | Heavy | Approaching limit, irreversible action |
| **Error/Destructive** | Heavy + Error | Heaviest | Delete, failure, critical alert |
| **Payment/Critical** | Custom Pattern | Reassuring | Financial transactions |

#### The Golden Rules

1. **Context Determines Weight**: Same action in different contexts = different haptics
   - Delete in list (swipe) = Medium
   - Delete in detail view = Heavy (more consequential)

2. **Anticipatory Haptics**: Fire on press, not release
   - User knows touch was registered
   - Builds confidence before action completes

3. **Detent Feedback**: Mark meaningful positions
   - Slider reaching min/max
   - Scroll snapping to position
   - Value returning to default

4. **Never Gratuitous**: If removing a haptic doesn't hurt UX, remove it

#### SwiftUI Implementation

```swift
// MARK: - Haptic Manager (Premium Pattern)
class HapticManager {
    static let shared = HapticManager()

    private let lightImpact = UIImpactFeedbackGenerator(style: .light)
    private let mediumImpact = UIImpactFeedbackGenerator(style: .medium)
    private let heavyImpact = UIImpactFeedbackGenerator(style: .heavy)
    private let selection = UISelectionFeedbackGenerator()
    private let notification = UINotificationFeedbackGenerator()

    // Prepare generators for instant response
    func prepare() {
        lightImpact.prepare()
        mediumImpact.prepare()
        selection.prepare()
    }

    // Discovery: scrolling, hovering
    func discovery() {
        selection.selectionChanged()
    }

    // Selection: choosing item
    func select() {
        lightImpact.impactOccurred()
    }

    // Confirmation: action confirmed
    func confirm() {
        mediumImpact.impactOccurred()
    }

    // Success: task completed
    func success() {
        notification.notificationOccurred(.success)
    }

    // Warning: approaching limit
    func warning() {
        notification.notificationOccurred(.warning)
    }

    // Error: something went wrong
    func error() {
        notification.notificationOccurred(.error)
    }

    // Destructive: delete, irreversible
    func destructive() {
        heavyImpact.impactOccurred()
        DispatchQueue.main.asyncAfter(deadline: .now() + 0.1) {
            self.notification.notificationOccurred(.error)
        }
    }

    // Payment: reassuring pattern
    func payment() {
        mediumImpact.impactOccurred()
        DispatchQueue.main.asyncAfter(deadline: .now() + 0.15) {
            self.mediumImpact.impactOccurred(intensity: 0.7)
        }
        DispatchQueue.main.asyncAfter(deadline: .now() + 0.3) {
            self.notification.notificationOccurred(.success)
        }
    }

    // Detent: slider/scroll snapping
    func detent() {
        lightImpact.impactOccurred(intensity: 0.5)
    }
}

// Usage in SwiftUI
Button("Delete") {
    HapticManager.shared.destructive()
    deleteItem()
}

// iOS 17+ Sensory Feedback
Button("Confirm") { }
    .sensoryFeedback(.impact(weight: .medium), trigger: isConfirmed)

// Anticipatory haptic (on press, not release)
struct AnticipatorButton: View {
    var body: some View {
        Text("Press Me")
            .simultaneousGesture(
                DragGesture(minimumDistance: 0)
                    .onChanged { _ in
                        HapticManager.shared.select()
                    }
            )
    }
}
```

#### React Native Implementation

```tsx
import * as Haptics from 'expo-haptics';

// Haptic Manager for React Native
const HapticManager = {
    // Discovery: scrolling
    discovery: () => Haptics.selectionAsync(),

    // Selection: item chosen
    select: () => Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light),

    // Confirmation: action confirmed
    confirm: () => Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium),

    // Success: task completed
    success: () => Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success),

    // Warning
    warning: () => Haptics.notificationAsync(Haptics.NotificationFeedbackType.Warning),

    // Error
    error: () => Haptics.notificationAsync(Haptics.NotificationFeedbackType.Error),

    // Destructive: heavy + error
    destructive: async () => {
        await Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Heavy);
        setTimeout(() => {
            Haptics.notificationAsync(Haptics.NotificationFeedbackType.Error);
        }, 100);
    },

    // Payment: reassuring pattern
    payment: async () => {
        await Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium);
        setTimeout(() => Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light), 150);
        setTimeout(() => Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success), 300);
    },
};

// Anticipatory haptic button
const AnticipatorButton = ({ onPress, children }) => (
    <Pressable
        onPressIn={() => HapticManager.select()}  // On press, not release
        onPress={onPress}
    >
        {children}
    </Pressable>
);
```

#### Flutter Implementation

```dart
import 'package:flutter/services.dart';

// Haptic Manager for Flutter
class HapticManager {
    // Discovery
    static void discovery() => HapticFeedback.selectionClick();

    // Selection
    static void select() => HapticFeedback.lightImpact();

    // Confirmation
    static void confirm() => HapticFeedback.mediumImpact();

    // Success (vibrate pattern)
    static void success() => HapticFeedback.mediumImpact();

    // Warning
    static void warning() => HapticFeedback.heavyImpact();

    // Error
    static void error() => HapticFeedback.heavyImpact();

    // Destructive
    static Future<void> destructive() async {
        HapticFeedback.heavyImpact();
        await Future.delayed(Duration(milliseconds: 100));
        HapticFeedback.heavyImpact();
    }

    // Payment pattern
    static Future<void> payment() async {
        HapticFeedback.mediumImpact();
        await Future.delayed(Duration(milliseconds: 150));
        HapticFeedback.lightImpact();
        await Future.delayed(Duration(milliseconds: 150));
        HapticFeedback.mediumImpact();
    }

    // Detent
    static void detent() => HapticFeedback.selectionClick();
}

// Anticipatory haptic button
class AnticipatorButton extends StatelessWidget {
    final VoidCallback onPressed;
    final Widget child;

    const AnticipatorButton({required this.onPressed, required this.child});

    @override
    Widget build(BuildContext context) {
        return GestureDetector(
            onTapDown: (_) => HapticManager.select(),  // On press
            onTap: onPressed,
            child: child,
        );
    }
}
```

#### Haptic Patterns by Context

| Context | Interaction | Recommended Pattern |
|---------|-------------|---------------------|
| List scrolling | Each item passes | `discovery()` (debounced) |
| Toggle switch | State change | `select()` |
| Button press | Confirmation | `confirm()` on press |
| Form submit | Success/failure | `success()` or `error()` |
| Pull to refresh | Threshold reached | `detent()` |
| Swipe delete | Delete executed | `destructive()` |
| Payment | Transaction complete | `payment()` pattern |
| Slider | Reaches min/max | `detent()` |
| Tab switch | Tab selected | `select()` |
| Long press menu | Menu appears | `confirm()` |

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

> "Animation is not about making things move. It's about making things feel alive."

### The 200-500ms Sweet Spot

Animation duration must be **intentional**:

| Duration | Perception | Use Case |
|----------|------------|----------|
| < 100ms | Instant | Button feedback, selection |
| 100-200ms | Snappy | Tab switches, toggles |
| 200-300ms | Responsive | Sheets, cards, list items |
| 300-500ms | Smooth | Page transitions, reveals |
| > 500ms | Cinematic | Onboarding, celebrations |

**Research finding:** Physics-based animations boost user satisfaction by **24%** compared to linear/eased animations.

### Core Principles
- **Purpose**: Every animation answers "why is this moving?"
- **Responsiveness**: Animation velocity matches gesture velocity
- **Continuity**: Maintain context, never lose the user
- **Interruptibility**: User can always cancel or redirect
- **Subtlety**: 80% of animations should be barely noticed

### Physics-Based Animations

Premium apps feel alive because they use real physics, not timers.

#### UIKit Dynamics (for Complex Physics)

```swift
import UIKit

class PhysicsView: UIView {
    private var animator: UIDynamicAnimator!
    private var gravity: UIGravityBehavior!
    private var collision: UICollisionBehavior!
    private var attachment: UIAttachmentBehavior!

    func setupPhysics(for element: UIView) {
        animator = UIDynamicAnimator(referenceView: self)

        // Gravity - elements fall naturally
        gravity = UIGravityBehavior(items: [element])
        gravity.magnitude = 0.5

        // Collision - bounce off boundaries
        collision = UICollisionBehavior(items: [element])
        collision.translatesReferenceBoundsIntoBoundary = true

        // Attachment - rubber band effect
        attachment = UIAttachmentBehavior(
            item: element,
            attachedToAnchor: element.center
        )
        attachment.frequency = 3.0  // Oscillation speed
        attachment.damping = 0.3    // How quickly it settles
        attachment.length = 0       // Resting distance

        animator.addBehavior(gravity)
        animator.addBehavior(collision)
        animator.addBehavior(attachment)
    }

    // Drag with physics response
    func handleDrag(_ gesture: UIPanGestureRecognizer, element: UIView) {
        let location = gesture.location(in: self)

        switch gesture.state {
        case .began:
            attachment.anchorPoint = location
        case .changed:
            attachment.anchorPoint = location
        case .ended:
            // Element snaps back with physics
            attachment.anchorPoint = element.center
            let velocity = gesture.velocity(in: self)
            let push = UIPushBehavior(items: [element], mode: .instantaneous)
            push.pushDirection = CGVector(dx: velocity.x / 1000, dy: velocity.y / 1000)
            animator.addBehavior(push)
        default:
            break
        }
    }
}
```

### Spring Animations

#### SwiftUI Spring Presets

```swift
// MARK: - Animation Presets (Premium)
extension Animation {
    // Quick feedback (buttons, toggles)
    static let quickResponse = Animation.spring(response: 0.3, dampingFraction: 0.7)

    // Standard interaction (cards, sheets)
    static let standard = Animation.spring(response: 0.4, dampingFraction: 0.75)

    // Bouncy (playful apps)
    static let bouncy = Animation.spring(response: 0.5, dampingFraction: 0.5)

    // Smooth (premium/luxury apps)
    static let smooth = Animation.spring(response: 0.6, dampingFraction: 0.9)

    // Snappy (quick actions)
    static let snappy = Animation.spring(response: 0.25, dampingFraction: 0.8)
}

// Usage
withAnimation(.quickResponse) {
    isPressed = true
}

withAnimation(.smooth) {
    showDetail = true
}
```

#### The Spring Formula Explained

```swift
// response: How long to reach target (0.2 = fast, 0.8 = slow)
// dampingFraction: How much bounce (0.5 = bouncy, 1.0 = no bounce)
// blendDuration: How to blend with other animations

Animation.spring(
    response: 0.4,        // 400ms to settle
    dampingFraction: 0.7, // Slight bounce
    blendDuration: 0.25   // Blend over 250ms
)

// Mass-stiffness-damping model (more control)
Animation.interpolatingSpring(
    mass: 1.0,       // Heavier = slower
    stiffness: 150,  // Higher = snappier
    damping: 15,     // Higher = less bounce
    initialVelocity: 0
)
```

#### React Native

```tsx
import Animated, {
    withSpring,
    withTiming,
    useAnimatedStyle,
    useSharedValue,
    runOnJS,
} from 'react-native-reanimated';

// Spring presets
const SpringPresets = {
    quickResponse: { damping: 20, stiffness: 300 },
    standard: { damping: 15, stiffness: 150 },
    bouncy: { damping: 10, stiffness: 100 },
    smooth: { damping: 20, stiffness: 90 },
    snappy: { damping: 25, stiffness: 400 },
};

// Gesture-responsive animation
const GestureCard = () => {
    const translateX = useSharedValue(0);
    const scale = useSharedValue(1);

    const gesture = Gesture.Pan()
        .onStart(() => {
            scale.value = withSpring(0.98, SpringPresets.quickResponse);
        })
        .onUpdate((e) => {
            // Animation follows finger exactly
            translateX.value = e.translationX;
        })
        .onEnd((e) => {
            // Velocity-aware snap back
            translateX.value = withSpring(0, {
                ...SpringPresets.standard,
                velocity: e.velocityX,  // Preserve gesture momentum
            });
            scale.value = withSpring(1, SpringPresets.quickResponse);
        });

    const animatedStyle = useAnimatedStyle(() => ({
        transform: [
            { translateX: translateX.value },
            { scale: scale.value },
        ],
    }));

    return (
        <GestureDetector gesture={gesture}>
            <Animated.View style={[styles.card, animatedStyle]} />
        </GestureDetector>
    );
};
```

#### Flutter

```dart
// Spring presets
class SpringPresets {
    static final quickResponse = SpringDescription(
        mass: 1, stiffness: 300, damping: 20,
    );
    static final standard = SpringDescription(
        mass: 1, stiffness: 150, damping: 15,
    );
    static final bouncy = SpringDescription(
        mass: 1, stiffness: 100, damping: 10,
    );
    static final smooth = SpringDescription(
        mass: 1, stiffness: 90, damping: 20,
    );
}

// Physics-based animation controller
class PhysicsCard extends StatefulWidget {
    @override
    _PhysicsCardState createState() => _PhysicsCardState();
}

class _PhysicsCardState extends State<PhysicsCard>
    with SingleTickerProviderStateMixin {
    late AnimationController _controller;
    late Animation<Offset> _animation;
    Offset _dragOffset = Offset.zero;

    @override
    void initState() {
        super.initState();
        _controller = AnimationController(vsync: this);
    }

    void _onPanUpdate(DragUpdateDetails details) {
        setState(() {
            _dragOffset += details.delta;
        });
    }

    void _onPanEnd(DragEndDetails details) {
        // Spring back with velocity
        final spring = SpringSimulation(
            SpringPresets.standard,
            _dragOffset.distance,
            0,
            details.velocity.pixelsPerSecond.distance / 1000,
        );

        _controller.animateWith(spring);
    }

    @override
    Widget build(BuildContext context) {
        return GestureDetector(
            onPanUpdate: _onPanUpdate,
            onPanEnd: _onPanEnd,
            child: Transform.translate(
                offset: _dragOffset,
                child: Card(child: /* content */),
            ),
        );
    }
}
```

### Gesture-Responsive Animations

The key to premium feel: **animation velocity matches gesture velocity**.

#### SwiftUI

```swift
struct GestureResponsiveCard: View {
    @State private var offset: CGSize = .zero
    @State private var isDragging = false
    @GestureState private var dragVelocity: CGSize = .zero

    var body: some View {
        RoundedRectangle(cornerRadius: 20)
            .fill(.ultraThinMaterial)
            .frame(width: 300, height: 200)
            .offset(offset)
            .scaleEffect(isDragging ? 0.98 : 1)
            .gesture(
                DragGesture()
                    .updating($dragVelocity) { value, state, _ in
                        state = value.velocity
                    }
                    .onChanged { value in
                        isDragging = true
                        // Direct 1:1 tracking
                        offset = value.translation
                    }
                    .onEnded { value in
                        isDragging = false
                        // Animate with velocity for natural momentum
                        withAnimation(.interpolatingSpring(
                            mass: 1,
                            stiffness: 100,
                            damping: 15,
                            initialVelocity: sqrt(
                                pow(value.velocity.width, 2) +
                                pow(value.velocity.height, 2)
                            ) / 1000
                        )) {
                            offset = .zero
                        }
                    }
            )
            .animation(.quickResponse, value: isDragging)
    }
}
```

### State Transition Patterns

#### Button Press Animation

```swift
// Premium button with scale + haptic
struct PremiumButton: View {
    let title: String
    let action: () -> Void

    @State private var isPressed = false

    var body: some View {
        Text(title)
            .font(.headline)
            .foregroundStyle(.white)
            .padding(.horizontal, 24)
            .padding(.vertical, 14)
            .background(Color.accentColor)
            .clipShape(Capsule())
            .scaleEffect(isPressed ? 0.96 : 1)
            .opacity(isPressed ? 0.9 : 1)
            .animation(.quickResponse, value: isPressed)
            .simultaneousGesture(
                DragGesture(minimumDistance: 0)
                    .onChanged { _ in
                        if !isPressed {
                            isPressed = true
                            HapticManager.shared.select()
                        }
                    }
                    .onEnded { _ in
                        isPressed = false
                        action()
                        HapticManager.shared.confirm()
                    }
            )
    }
}
```

#### Loading → Success Celebration

```swift
struct LoadingSuccessView: View {
    @State private var state: LoadState = .idle

    enum LoadState {
        case idle, loading, success, error
    }

    var body: some View {
        ZStack {
            switch state {
            case .idle:
                Button("Submit") {
                    withAnimation(.standard) { state = .loading }
                    performAction()
                }
            case .loading:
                ProgressView()
                    .transition(.scale.combined(with: .opacity))
            case .success:
                Image(systemName: "checkmark.circle.fill")
                    .font(.system(size: 48))
                    .foregroundStyle(.green)
                    .transition(.scale.combined(with: .opacity))
                    .onAppear {
                        HapticManager.shared.success()
                        // Optional: confetti animation
                    }
            case .error:
                Image(systemName: "xmark.circle.fill")
                    .font(.system(size: 48))
                    .foregroundStyle(.red)
                    .transition(.scale.combined(with: .opacity))
                    .onAppear {
                        HapticManager.shared.error()
                    }
            }
        }
        .animation(.bouncy, value: state)
    }
}
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

// Full custom navigation transition (iOS 18+)
.navigationTransition(.zoom(sourceID: "photo", in: animation))
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

// Custom screen transition
<Stack.Screen
    options={{
        animation: 'slide_from_right',
        animationDuration: 300,
        gestureEnabled: true,
        gestureDirection: 'horizontal',
    }}
/>
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

### Micro-Interaction Patterns

| Interaction | Animation | Duration | Haptic |
|-------------|-----------|----------|--------|
| Button press | Scale 0.96 | 100ms | Light |
| Toggle | Spring position | 200ms | Selection |
| Card expand | Scale + fade | 300ms | Medium |
| Pull refresh | Threshold snap | - | Detent |
| Swipe delete | Slide + fade | 250ms | Medium |
| Success | Scale up + bounce | 400ms | Success |
| Tab switch | Cross-fade | 150ms | Selection |
| Sheet present | Slide up | 350ms | Medium |

### Animation Performance Tips

1. **Prefer transform properties**: `scale`, `rotation`, `translation` are GPU-accelerated
2. **Avoid animating layout**: Width/height changes trigger layout recalculation
3. **Use `drawingGroup()`** in SwiftUI for complex view hierarchies
4. **Reduce overdraw**: Transparent overlapping views hurt performance
5. **Profile with Instruments**: Always test on device, not simulator

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

## Data Visualization

> "Each data point tells a story; visualization shouldn't require a legend."

### Progressive Disclosure Pattern

Show **20%** by default, reveal **80%** on interaction:

```
┌─────────────────────────────────────┐
│ Level 1: Glance (visible)           │
│   "Your heart rate: 72 bpm"         │
├─────────────────────────────────────┤
│ Level 2: Tap (revealed)             │
│   "Range today: 58-98 bpm"          │
│   "Resting avg: 62 bpm"             │
├─────────────────────────────────────┤
│ Level 3: Drill-down (new screen)    │
│   Full history, comparisons,        │
│   export options                    │
└─────────────────────────────────────┘
```

### Information Hierarchy

```swift
// Premium data card structure
struct DataCard: View {
    let primaryValue: String      // Largest, hero position
    let primaryLabel: String
    let secondaryValue: String?   // Supporting context
    let trend: Trend?             // Up/down/stable indicator

    var body: some View {
        VStack(alignment: .leading, spacing: 8) {
            // Primary: The ONE thing user needs
            Text(primaryValue)
                .font(.system(size: 56, weight: .medium, design: .rounded))
                .monospacedDigit()

            Text(primaryLabel)
                .font(.subheadline)
                .foregroundStyle(.secondary)

            // Secondary: Supporting context
            if let secondary = secondaryValue {
                HStack {
                    if let trend = trend {
                        Image(systemName: trend.icon)
                            .foregroundStyle(trend.color)
                    }
                    Text(secondary)
                        .font(.caption)
                        .foregroundStyle(.tertiary)
                }
            }
        }
    }
}
```

### SwiftUI Charts

```swift
import Charts

// MARK: - Line Chart (Trends)
struct HeartRateChart: View {
    let data: [HeartRateReading]

    var body: some View {
        Chart(data) { reading in
            LineMark(
                x: .value("Time", reading.time),
                y: .value("BPM", reading.bpm)
            )
            .interpolationMethod(.catmullRom)  // Smooth curves
            .foregroundStyle(.red.gradient)

            // Area fill for emphasis
            AreaMark(
                x: .value("Time", reading.time),
                y: .value("BPM", reading.bpm)
            )
            .foregroundStyle(.red.opacity(0.1).gradient)
        }
        .chartXAxis {
            AxisMarks(values: .stride(by: .hour, count: 3)) { value in
                AxisValueLabel(format: .dateTime.hour())
            }
        }
        .chartYAxis {
            AxisMarks(position: .leading)
        }
        .chartYScale(domain: 40...120)
        // Interactive selection
        .chartOverlay { proxy in
            GeometryReader { geo in
                Rectangle().fill(.clear).contentShape(Rectangle())
                    .gesture(
                        DragGesture()
                            .onChanged { value in
                                let x = value.location.x - geo[proxy.plotAreaFrame].origin.x
                                if let time: Date = proxy.value(atX: x) {
                                    selectedTime = time
                                }
                            }
                    )
            }
        }
    }
}

// MARK: - Progress Ring
struct ProgressRing: View {
    let progress: Double  // 0.0 to 1.0
    let color: Color
    let lineWidth: CGFloat = 12

    var body: some View {
        ZStack {
            // Background track
            Circle()
                .stroke(color.opacity(0.2), lineWidth: lineWidth)

            // Progress arc
            Circle()
                .trim(from: 0, to: progress)
                .stroke(
                    color.gradient,
                    style: StrokeStyle(lineWidth: lineWidth, lineCap: .round)
                )
                .rotationEffect(.degrees(-90))
                .animation(.spring(response: 0.6), value: progress)
        }
    }
}

// MARK: - Activity Rings (Apple Fitness style)
struct ActivityRings: View {
    let move: Double
    let exercise: Double
    let stand: Double

    var body: some View {
        ZStack {
            ProgressRing(progress: stand, color: .cyan)
                .frame(width: 100, height: 100)
            ProgressRing(progress: exercise, color: .green)
                .frame(width: 130, height: 130)
            ProgressRing(progress: move, color: .red)
                .frame(width: 160, height: 160)
        }
    }
}
```

### Real-Time Data Updates

```swift
// Smooth number transitions
struct AnimatedNumber: View {
    let value: Int

    var body: some View {
        Text("\(value)")
            .font(.system(size: 48, weight: .medium, design: .rounded))
            .monospacedDigit()
            .contentTransition(.numericText())
            .animation(.snappy, value: value)
    }
}

// Status with smooth color transition
struct LiveStatus: View {
    let status: Status

    var body: some View {
        HStack {
            Circle()
                .fill(status.color)
                .frame(width: 8, height: 8)

            Text(status.label)
                .font(.subheadline)
        }
        .animation(.smooth, value: status)
    }
}
```

### Color as Data

| Meaning | Color | Usage |
|---------|-------|-------|
| Success/Good | Green | On-time, goal met, healthy |
| Warning/Attention | Yellow/Orange | Approaching limit, caution |
| Error/Problem | Red | Delayed, over limit, critical |
| Neutral/Info | Blue | Normal state, informational |
| Inactive/Disabled | Gray | Unavailable, historical |

```swift
// Status-aware styling
extension FlightStatus {
    var color: Color {
        switch self {
        case .onTime: return .green
        case .delayed: return .orange
        case .cancelled: return .red
        case .boarding: return .blue
        }
    }
}
```

---

## Empty States & Edge Cases

> "Every state is a first impression for someone."

### Empty State Anatomy

```
┌─────────────────────────────────────┐
│                                     │
│          [Illustration]             │  ← Visual, not just icon
│                                     │
│        No flights yet               │  ← Clear headline
│                                     │
│   Add a flight to start tracking    │  ← Helpful explanation
│   your journey.                     │
│                                     │
│       [ Add Flight ]                │  ← Single CTA
│                                     │
└─────────────────────────────────────┘
```

### SwiftUI Implementation

```swift
struct EmptyStateView: View {
    let icon: String
    let title: String
    let message: String
    let actionTitle: String?
    let action: (() -> Void)?

    var body: some View {
        VStack(spacing: 20) {
            // Illustration or animated icon
            Image(systemName: icon)
                .font(.system(size: 64))
                .foregroundStyle(.secondary)
                .symbolEffect(.pulse.byLayer)

            VStack(spacing: 8) {
                Text(title)
                    .font(.title2.bold())

                Text(message)
                    .font(.body)
                    .foregroundStyle(.secondary)
                    .multilineTextAlignment(.center)
                    .padding(.horizontal, 32)
            }

            if let actionTitle = actionTitle, let action = action {
                Button(actionTitle, action: action)
                    .buttonStyle(.borderedProminent)
            }
        }
        .frame(maxWidth: .infinity, maxHeight: .infinity)
    }
}

// Usage
EmptyStateView(
    icon: "airplane.circle",
    title: "No Flights Yet",
    message: "Add a flight to start tracking your journey.",
    actionTitle: "Add Flight",
    action: { showAddFlight = true }
)
```

### Loading States

**Skeleton screens** beat spinners for perceived performance:

```swift
struct SkeletonCard: View {
    @State private var isAnimating = false

    var body: some View {
        VStack(alignment: .leading, spacing: 12) {
            // Title placeholder
            RoundedRectangle(cornerRadius: 4)
                .fill(.quaternary)
                .frame(width: 120, height: 20)

            // Subtitle placeholder
            RoundedRectangle(cornerRadius: 4)
                .fill(.quaternary)
                .frame(width: 200, height: 16)

            // Content placeholder
            RoundedRectangle(cornerRadius: 4)
                .fill(.quaternary)
                .frame(height: 80)
        }
        .padding()
        .background(.ultraThinMaterial)
        .clipShape(RoundedRectangle(cornerRadius: 16))
        // Shimmer effect
        .overlay {
            LinearGradient(
                colors: [.clear, .white.opacity(0.4), .clear],
                startPoint: .leading,
                endPoint: .trailing
            )
            .offset(x: isAnimating ? 400 : -400)
        }
        .clipShape(RoundedRectangle(cornerRadius: 16))
        .onAppear {
            withAnimation(.linear(duration: 1.5).repeatForever(autoreverses: false)) {
                isAnimating = true
            }
        }
    }
}
```

### Error States

Human language, not error codes:

```swift
struct ErrorView: View {
    let error: AppError
    let retry: () -> Void

    var body: some View {
        VStack(spacing: 16) {
            Image(systemName: "exclamationmark.triangle")
                .font(.system(size: 48))
                .foregroundStyle(.orange)

            Text(error.userFriendlyTitle)
                .font(.headline)

            Text(error.userFriendlyMessage)
                .font(.subheadline)
                .foregroundStyle(.secondary)
                .multilineTextAlignment(.center)

            Button("Try Again", action: retry)
                .buttonStyle(.bordered)
        }
        .padding()
    }
}

// Human-friendly error messages
extension AppError {
    var userFriendlyTitle: String {
        switch self {
        case .networkError: return "Connection Problem"
        case .serverError: return "Something Went Wrong"
        case .notFound: return "Not Found"
        }
    }

    var userFriendlyMessage: String {
        switch self {
        case .networkError:
            return "Please check your internet connection and try again."
        case .serverError:
            return "We're having trouble on our end. Please try again in a moment."
        case .notFound:
            return "We couldn't find what you're looking for."
        }
    }
}
```

### Offline Mode

```swift
struct OfflineBanner: View {
    var body: some View {
        HStack {
            Image(systemName: "wifi.slash")
            Text("You're offline")
                .font(.subheadline)
            Spacer()
            Text("Showing cached data")
                .font(.caption)
                .foregroundStyle(.secondary)
        }
        .padding(.horizontal)
        .padding(.vertical, 8)
        .background(.orange.opacity(0.2))
    }
}
```

### First-Run Experience

```swift
// Progressive onboarding - one concept at a time
struct OnboardingFlow: View {
    @State private var currentPage = 0

    var body: some View {
        TabView(selection: $currentPage) {
            OnboardingPage(
                icon: "airplane.departure",
                title: "Track Your Flights",
                subtitle: "Get real-time updates on gates, delays, and boarding.",
                page: 0
            )

            OnboardingPage(
                icon: "bell.badge",
                title: "Never Miss a Change",
                subtitle: "Instant notifications for everything that matters.",
                page: 1
            )

            OnboardingPage(
                icon: "hand.tap",
                title: "One Tap Access",
                subtitle: "Live Activities keep you updated without opening the app.",
                page: 2,
                showGetStarted: true
            )
        }
        .tabViewStyle(.page)
    }
}
```

---

## Premium Polish Details

> "The details are not the details. They make the design." — Charles Eames

### Shadow System

```swift
extension View {
    // Light shadow - for subtle elevation
    func shadowLight() -> some View {
        self.shadow(color: .black.opacity(0.04), radius: 2, y: 1)
    }

    // Medium shadow - for cards and elevated content
    func shadowMedium() -> some View {
        self.shadow(color: .black.opacity(0.08), radius: 8, y: 4)
    }

    // Heavy shadow - for modals and popovers
    func shadowHeavy() -> some View {
        self.shadow(color: .black.opacity(0.15), radius: 20, y: 10)
    }

    // Colored shadow - matches content color
    func shadowColored(_ color: Color) -> some View {
        self.shadow(color: color.opacity(0.3), radius: 12, y: 6)
    }
}

// Usage
Card()
    .shadowMedium()

Button("Premium")
    .background(.blue)
    .shadowColored(.blue)  // Blue glow effect
```

### Corner Radius Hierarchy

| Element Type | Corner Radius | Example |
|--------------|---------------|---------|
| Outer containers | 20-24pt | Cards, modals |
| Inner containers | 12-16pt | Buttons, input fields |
| Nested elements | 8pt | Tags, badges |
| Tight spaces | 4pt | Small indicators |

```swift
// Never same radius everywhere
struct PremiumCard: View {
    var body: some View {
        VStack {
            // Card: 20pt
            RoundedRectangle(cornerRadius: 20)
                .overlay {
                    VStack {
                        // Button inside: 12pt
                        Button("Action") { }
                            .clipShape(RoundedRectangle(cornerRadius: 12))

                        // Tag inside button: 8pt
                        Text("New")
                            .padding(.horizontal, 8)
                            .background(
                                RoundedRectangle(cornerRadius: 8)
                                    .fill(.blue)
                            )
                    }
                }
        }
    }
}
```

### Gradient Usage

```swift
// Accent gradients (not flat colors)
struct PremiumButton: View {
    var body: some View {
        Text("Continue")
            .padding()
            .background(
                LinearGradient(
                    colors: [.blue, .blue.opacity(0.8)],
                    startPoint: .top,
                    endPoint: .bottom
                )
            )
            .clipShape(Capsule())
    }
}

// Mesh gradient (iOS 18+ premium)
struct PremiumBackground: View {
    var body: some View {
        MeshGradient(
            width: 3,
            height: 3,
            points: [
                [0, 0], [0.5, 0], [1, 0],
                [0, 0.5], [0.5, 0.5], [1, 0.5],
                [0, 1], [0.5, 1], [1, 1]
            ],
            colors: [
                .blue, .purple, .pink,
                .cyan, .mint, .orange,
                .blue, .indigo, .purple
            ]
        )
        .ignoresSafeArea()
    }
}
```

### Layered Glass Effects

```swift
struct LayeredGlassCard: View {
    var body: some View {
        ZStack {
            // Background layer (deepest)
            RoundedRectangle(cornerRadius: 24)
                .fill(.ultraThinMaterial)

            // Middle layer
            RoundedRectangle(cornerRadius: 20)
                .fill(.thinMaterial)
                .padding(4)

            // Content layer (front)
            VStack {
                // Content here
            }
            .padding()
        }
        .shadowMedium()
    }
}
```

### Ambient Animations

Subtle, continuous animations that make the UI feel alive:

```swift
struct AmbientBackground: View {
    @State private var phase: CGFloat = 0

    var body: some View {
        Canvas { context, size in
            // Animated gradient blobs
            let blob1 = Path(ellipseIn: CGRect(
                x: size.width * 0.2 + sin(phase) * 20,
                y: size.height * 0.3 + cos(phase) * 15,
                width: 200, height: 200
            ))

            context.fill(blob1, with: .color(.blue.opacity(0.3)))
        }
        .blur(radius: 60)
        .onAppear {
            withAnimation(.linear(duration: 8).repeatForever(autoreverses: true)) {
                phase = .pi * 2
            }
        }
    }
}
```

---

## Sound Design

> "Sound should confirm, not announce."

### When to Use Sound

| Use Sound | Don't Use Sound |
|-----------|-----------------|
| Payment complete | Every button tap |
| Message sent | Navigation |
| Achievement unlocked | Form validation |
| Critical error | Regular errors |
| Timer complete | List scrolling |

### System Sounds

```swift
import AudioToolbox

enum SystemSound {
    static func playSuccess() {
        AudioServicesPlaySystemSound(1407)  // Payment success
    }

    static func playError() {
        AudioServicesPlaySystemSound(1521)  // Error vibration
    }

    static func playTap() {
        AudioServicesPlaySystemSound(1104)  // Subtle tap
    }
}
```

### Custom Sounds Guidelines

1. **Duration**: < 1 second for feedback
2. **Volume**: Quieter than system sounds
3. **Frequency**: Mid-range (not jarring high or rumbling low)
4. **Consistency**: Same sound for same action
5. **Accessibility**: Never sound-only feedback

```swift
import AVFoundation

class SoundManager {
    static let shared = SoundManager()
    private var audioPlayer: AVAudioPlayer?

    func playSuccess() {
        guard let url = Bundle.main.url(forResource: "success", withExtension: "wav") else { return }

        do {
            audioPlayer = try AVAudioPlayer(contentsOf: url)
            audioPlayer?.volume = 0.5  // Quieter than system
            audioPlayer?.play()
        } catch {
            // Silently fail - never block UI for sound
        }
    }
}
```

---

## Context-Specific Design

Different app categories require different design approaches while staying within HIG.

### Travel Apps (Flighty-style)

```swift
// Airport signage conventions
struct FlightStatusBadge: View {
    let status: FlightStatus

    var body: some View {
        Text(status.rawValue.uppercased())
            .font(.system(size: 12, weight: .bold, design: .monospaced))
            .kerning(1)
            .padding(.horizontal, 8)
            .padding(.vertical, 4)
            .background(status.color)
            .foregroundStyle(.white)
            .clipShape(RoundedRectangle(cornerRadius: 4))
    }
}

// Time-zone aware display
struct TimeDisplay: View {
    let date: Date
    let timeZone: TimeZone

    var body: some View {
        VStack(alignment: .leading) {
            Text(date, format: .dateTime.hour().minute())
                .font(.title2.bold())
                .environment(\.timeZone, timeZone)

            Text(timeZone.abbreviation() ?? "")
                .font(.caption)
                .foregroundStyle(.secondary)
        }
    }
}
```

### Finance Apps

```swift
// Currency with proper formatting
struct CurrencyDisplay: View {
    let amount: Decimal
    let currency: String
    let showSign: Bool

    var body: some View {
        Text(amount, format: .currency(code: currency)
            .sign(strategy: showSign ? .always() : .automatic))
            .font(.system(.title, design: .rounded, weight: .semibold))
            .monospacedDigit()
            .foregroundStyle(amount >= 0 ? .primary : .red)
    }
}

// Trend indicator
struct TrendIndicator: View {
    let percentChange: Double

    var body: some View {
        HStack(spacing: 4) {
            Image(systemName: percentChange >= 0 ? "arrow.up.right" : "arrow.down.right")
            Text("\(abs(percentChange), specifier: "%.2f")%")
        }
        .font(.caption.bold())
        .foregroundStyle(percentChange >= 0 ? .green : .red)
    }
}

// Security indicator
struct SecureFieldIndicator: View {
    var body: some View {
        HStack {
            Image(systemName: "lock.fill")
                .foregroundStyle(.green)
            Text("256-bit encrypted")
                .font(.caption)
                .foregroundStyle(.secondary)
        }
    }
}
```

### Health/Fitness Apps

```swift
// Goal celebration
struct GoalComplete: View {
    @State private var animate = false

    var body: some View {
        VStack {
            Image(systemName: "star.fill")
                .font(.system(size: 64))
                .foregroundStyle(.yellow)
                .scaleEffect(animate ? 1.2 : 0.8)
                .onAppear {
                    withAnimation(.spring(response: 0.4, dampingFraction: 0.5)) {
                        animate = true
                    }
                    HapticManager.shared.success()
                }

            Text("Goal Complete!")
                .font(.title2.bold())

            Text("You've moved 30 minutes today")
                .foregroundStyle(.secondary)
        }
    }
}

// Progress ring with goal
struct GoalRing: View {
    let current: Double
    let goal: Double

    var progress: Double { min(current / goal, 1.0) }

    var body: some View {
        ZStack {
            ProgressRing(progress: progress, color: .green)

            VStack {
                Text("\(Int(current))")
                    .font(.system(size: 32, weight: .bold, design: .rounded))
                    .contentTransition(.numericText())
                Text("/ \(Int(goal)) min")
                    .font(.caption)
                    .foregroundStyle(.secondary)
            }
        }
        .frame(width: 150, height: 150)
    }
}
```

### Productivity Apps (Things 3-style)

```swift
// Quick capture
struct QuickAddField: View {
    @State private var text = ""
    @FocusState private var isFocused: Bool

    var body: some View {
        HStack {
            Image(systemName: "plus.circle.fill")
                .foregroundStyle(.blue)

            TextField("New task", text: $text)
                .focused($isFocused)
                .submitLabel(.done)
                .onSubmit {
                    guard !text.isEmpty else { return }
                    createTask(text)
                    text = ""
                    HapticManager.shared.confirm()
                }
        }
        .padding()
        .background(.ultraThinMaterial)
        .clipShape(RoundedRectangle(cornerRadius: 12))
    }
}

// Completion celebration
struct TaskComplete: View {
    var body: some View {
        Image(systemName: "checkmark.circle.fill")
            .font(.title2)
            .foregroundStyle(.green)
            .symbolEffect(.bounce, value: true)
    }
}

// Drag-to-organize
struct ReorderableList<Item: Identifiable, Content: View>: View {
    @Binding var items: [Item]
    let content: (Item) -> Content

    var body: some View {
        List {
            ForEach(items) { item in
                content(item)
            }
            .onMove { from, to in
                items.move(fromOffsets: from, toOffset: to)
                HapticManager.shared.select()
            }
        }
        .listStyle(.plain)
    }
}
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

### Design Philosophy
- [ ] Every design decision is **intentional**, not default
- [ ] Progressive disclosure (20% visible, 80% on-demand)
- [ ] Physical metaphor grounding for gestures
- [ ] Anticipatory feedback (haptics on press, not release)
- [ ] Delight budget allocated (5-10% for joy moments)
- [ ] One-handed design for primary actions

### Visual Design
- [ ] Follow Clarity, Deference, Depth principles
- [ ] Use Liquid Glass on 2-3 key surfaces (not everywhere)
- [ ] Semantic colors for automatic dark/light mode
- [ ] SF Pro with Dynamic Type scales
- [ ] 8-point grid for spacing
- [ ] Corner radius hierarchy (20pt → 12pt → 8pt)
- [ ] Shadow system (light/medium/heavy)
- [ ] Design for both iPhone and iPad

### Animations & Motion
- [ ] Spring-based animations (not linear)
- [ ] Animation velocity matches gesture velocity
- [ ] Duration in 200-500ms sweet spot
- [ ] Interruptible animations
- [ ] Reduce Motion support with alternatives
- [ ] Physics-based for organic feel

### Haptic Feedback
- [ ] Haptic hierarchy implemented (light → heavy)
- [ ] Context determines haptic weight
- [ ] Anticipatory haptics (on press, not release)
- [ ] Detent feedback for meaningful positions
- [ ] Never gratuitous vibrations

### Components
- [ ] Native components when possible
- [ ] Minimum 44x44pt touch targets
- [ ] Consistent navigation patterns
- [ ] Proper keyboard handling
- [ ] Safe area respect

### States & Edge Cases
- [ ] Empty states with illustration + CTA
- [ ] Loading states (skeleton screens, not spinners)
- [ ] Error states with human language
- [ ] Offline mode indicator
- [ ] First-run/onboarding experience

### Live Activities & Widgets
- [ ] Live Activities for time-sensitive data
- [ ] Widgets for all relevant sizes
- [ ] Lock Screen widgets where appropriate
- [ ] Glanceable in <3 seconds
- [ ] Deep links to relevant content

### Data Visualization
- [ ] Information hierarchy (primary → secondary → tertiary)
- [ ] Color as data (green/yellow/red meanings)
- [ ] Smooth number transitions
- [ ] Interactive charts where appropriate

### Sound Design
- [ ] Sounds for confirmations, not every action
- [ ] Custom sounds quieter than system
- [ ] Never sound-only feedback

### Accessibility
- [ ] VoiceOver labels for all interactive elements
- [ ] Dynamic Type support (all text scales)
- [ ] Color contrast (4.5:1 text, 3:1 UI)
- [ ] Reduce Motion alternatives
- [ ] Increase Contrast support

### Performance
- [ ] Smooth 60fps animations
- [ ] Lazy loading for lists
- [ ] Efficient image handling
- [ ] Prefer transform properties (GPU-accelerated)
- [ ] Minimal layout recalculations

### Testing
- [ ] Test on multiple device sizes
- [ ] Test in Light and Dark modes
- [ ] Test with VoiceOver enabled
- [ ] Test with Dynamic Type at various sizes
- [ ] Test with Reduce Motion enabled
- [ ] Test Live Activities (compact/expanded/minimal)
- [ ] Test widgets (all sizes)
- [ ] Test haptics on device (not simulator)
