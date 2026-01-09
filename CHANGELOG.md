# Changelog

All notable changes to iOS Mobile Designer will be documented in this file.

## [3.0.0] - 2026-01-09

### BREAKING: Radical Size Reduction

```
Before: 5,581 lines / 156KB
After:    752 lines /  20KB
Reduction:           86.5%
```

### Removed (Philosophy → Practicality)
- **Design Philosophy** (231 lines) - philosophical essays
- **Brand Personality DNA** (71 lines) - not Apple HIG
- **Color Theory & Palette Generation** (109 lines) - not iOS-specific
- **Anti-Template Techniques** (146 lines) - abstract concepts
- **Emotional Design Framework** (154 lines) - abstract concepts
- **Signature Moments** (152 lines) - nice-to-have
- **Motion Personality** (119 lines) - duplicated Animation section
- **Context-Specific Design** (203 lines) - Flighty-style examples
- **Premium Polish Details** (172 lines) - merged into components
- **Data Visualization** (217 lines) - specialized

### Condensed
- **Animation & Motion**: 643 → 54 lines (92% reduction)
- **Live Activities**: 400 → 30 lines (92% reduction)
- **Widgets**: 350 → 20 lines (94% reduction)
- **UI Components**: 578 → 100 lines (83% reduction)

### Kept (Essential HIG)
- Core Principles (Clarity, Deference, Depth, Consistency)
- Typography System (SF Pro, Dynamic Type)
- Color System (Semantic colors)
- Spacing & Layout (8-point grid, Safe Areas)
- Backgrounds & Materials (with limits)
- UI Components (Navigation, Tabs, Sheets, Buttons, Forms)
- Touch Targets & Gestures
- Accessibility (VoiceOver, Dynamic Type, Contrast, Motion)
- Animation & Motion (Spring presets, Symbol Effects)
- SF Symbols
- Live Activities & Widgets (condensed)
- iOS 18+ Features (Control Center, App Intents, Tinted Icons)
- visionOS (key differences)
- Empty States & Edge Cases
- Screen Sizes Reference
- Best Practices Checklist

### Why This Matters
- **Faster processing**: Claude reads less content per request
- **Focused guidance**: Only practical, actionable information
- **No fluff**: Every line is a usable reference or code snippet
- **Same coverage**: All essential iOS patterns still included

## [2.3.0] - 2026-01-08

### Added
- **iOS 18+ Features Section** - Control Center Widgets, App Intents system, Tinted App Icons, Interactive Widget enhancements
- **visionOS Considerations** - Eye tracking, spatial layout, glass materials, hover effects, typography adjustments
- **Modern Animation APIs** - Phased animations, Keyframe animations, Symbol Effects (iOS 17+), Custom Transitions
- **Creative Archetype** - New brand archetype: Craft, Procreate, Darkroom style

### Improved
- **Diversified App Examples** - Added Arc, Bear, Craft, Copilot, YNAB, Streaks, Linear, Notion, Darkroom
- **Updated SwiftUI APIs** - `.symbolEffect(.bounce.up.byLayer)`, `PhaseAnimator`, `keyframeAnimator`
- **Consolidated Material Warnings** - Removed duplicates, single authoritative section
- **Condensed Verbose Sections** - ~15% reduction in redundant content

### Changed
- **"The Flighty Principle" → "Data as Art Principle"** - More general naming
- **Physical Metaphor Table** - Added Bear, Craft, Arc, Copilot examples
- **Signature Moments Table** - Added 5 new app examples
- **Brand Archetypes Table** - Added Creative archetype, diversified examples

### Best Practices Checklist
- Added iOS 18+ Features section
- Added visionOS testing checklist

### Stats
- New sections: 3 (iOS 18+, visionOS, Modern Animation APIs)
- New app examples: 12+
- Modern API patterns: 20+ new snippets

## [2.2.0] - 2026-01-08

### Critical Fixes
- **Removed `.glassEffect()` API** - This API doesn't exist in standard SwiftUI; replaced with proper material APIs
- **Fixed glass over-emphasis** - Restructured to match Apple HIG: Typography > Color > Spacing > Materials

### Added
- **Backgrounds & Surface Treatments** - New section with solid backgrounds as default, decision tree for when to use materials
- **Performance Budgets** - Apple guidelines: ≤4 compositing layers, ≤40px blur radius (iPhone)
- **Accessibility Fallbacks** - Required code for Reduce Transparency support (SwiftUI, React Native, Flutter)
- **When to Use Materials Table** - Decision framework with ✅/⚠️/❌ recommendations
- **Priority Hierarchy** - nav bar > tab bar > sheets > cards
- **Vestibular Sensitivity** - Parallax limits (≤6px), animation duration limits (≤300ms)
- **What NOT to Do** - Anti-patterns with code examples (nested blurs, no fallbacks)

### Changed
- **Renamed "Liquid Glass" → "Translucent Materials & Visual Depth"** - Reflects supplementary nature
- **Frontmatter updated** - Typography, Color, Spacing now emphasized as primary tools
- **Anti-Patterns section** - Added material-specific warnings
- **Best Practices Checklist** - Added Translucent Materials section with performance/accessibility checks

### Philosophy Shift
- **Apple HIG Alignment** - "Typography, color, and spacing are PRIMARY design tools — translucent effects are supplementary"
- **Accessibility First** - Reduce Transparency fallbacks are REQUIRED, not optional
- **Performance Aware** - Blur effects have real GPU cost; documented constraints

### Stats
- File size: ~4,900 → 5,150+ lines (+5%)
- Fixed: 1 fake API removed
- New sections: 2 (Backgrounds, Materials revision)
- New code patterns: 15+ accessibility-aware snippets

## [2.1.0] - 2026-01-06

### Added
- **Brand Personality DNA System** - 7 archetypes (Luxury, Playful, Professional, Minimalist, Bold, Warm, Technical) with design decision mapping
- **Color Theory & Palette Generation** - 60-30-10 rule, emotion-based palettes, dark mode optimization, brand color extraction
- **Anti-Template Techniques** - "One Unexpected Thing" rule, breaking the grid intentionally, avoiding AI-generated aesthetic
- **Emotional Design Framework** - User journey mapping, emotion-targeted design (confidence, delight, recovery, anticipation)
- **Signature Moments Design** - Creating memorable micro-moments like Things 3 completion celebration
- **Motion Personality System** - 5 motion archetypes (Playful, Professional, Elegant, Snappy, Organic) with spring configs

### Philosophy
- **Unique over Template** - Every app should have distinctive personality, not follow generic patterns
- **Brand DNA First** - Design decisions flow from brand archetype configuration
- **Emotional Journey** - Map user emotions through the app experience

### Stats
- File size: ~4,125 → 4,900+ lines (+19%)
- New sections: 6
- New code patterns: 30+ SwiftUI/React Native/Flutter snippets

## [2.0.0] - 2026-01-06

### Added
- **Live Activities & Dynamic Island** - Flighty-style implementation with compact/expanded/minimal states
- **Widgets Design** - All sizes (small/medium/large), Lock Screen, StandBy, interactive (iOS 17+)
- **Data Visualization** - SwiftUI Charts, progress rings, real-time updates, color as data
- **Empty States & Edge Cases** - Skeleton loading, error states, offline mode, onboarding
- **Premium Polish Details** - Shadow system, corner radius hierarchy, gradients, ambient animations
- **Sound Design** - When to use sound, system sounds, custom sound guidelines
- **Context-Specific Design** - Travel (Flighty-style), Finance, Health/Fitness, Productivity (Things 3-style)
- **Haptic Hierarchy System** - Strategic haptics with HapticManager class

### Enhanced
- **Design Philosophy** - The Flighty Principle, Physical Metaphors, Anticipatory Feedback, Delight Budget, Constraint-Based Design
- **Animation & Motion** - Physics-based animations (UIKit Dynamics), spring presets, gesture-responsive patterns
- **Best Practices Checklist** - Expanded from 21 to 50+ checkpoints

### Stats
- File size: ~1,687 → 4,125 lines (+145%)
- New sections: 7
- Code examples: 50+ new SwiftUI/React Native/Flutter snippets

## [1.0.0] - 2026-01-05

### Initial Release
- Core Apple HIG principles (Clarity, Deference, Depth)
- Liquid Glass (iOS 26) implementation
- Typography system with SF Pro
- Color system with semantic colors
- Spacing & Layout (8-point grid)
- UI Components (Navigation, Tab Bar, Sheets, Buttons, Forms, Lists)
- Touch Targets & Gestures
- Basic Haptic Feedback
- Accessibility (VoiceOver, Dynamic Type, Color Contrast, Reduce Motion)
- Basic Animation & Motion patterns
- SF Symbols usage
- Screen Sizes Reference
- Multi-framework support: SwiftUI, React Native, Flutter
