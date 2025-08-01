# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ACarousel is a SwiftUI library that provides a customizable carousel/slider component for iOS 13.0+, macOS 10.15+, and tvOS 13.0+ applications. The library follows Apple's Swift Package Manager structure and uses reactive programming patterns with Combine framework.

## Development Commands

### Building the Library
```bash
swift build
```

### Running Tests
```bash
swift test
```

### Building Demo Applications
```bash
# Open Xcode project for demo apps
open ACarouselDemo/ACarouselDemo.xcodeproj

# Build iOS demo
xcodebuild -project ACarouselDemo/ACarouselDemo.xcodeproj -scheme "ACarouselDemo iOS" -destination "platform=iOS Simulator,name=iPhone 14" build

# Build macOS demo  
xcodebuild -project ACarouselDemo/ACarouselDemo.xcodeproj -scheme "ACarouselDemo macOS" build
```

## Architecture Overview

### Core Components

**ACarousel** (`Sources/ACarousel/ACarousel.swift`): The main SwiftUI view component that renders the carousel. Uses a generic structure supporting any data type conforming to `RandomAccessCollection` with `Hashable` identifiers.

**ACarouselViewModel** (`Sources/ACarousel/ACarouselViewModel.swift`): The brain of the carousel implementing `ObservableObject`. Manages:
- Item positioning and offset calculations
- Drag gesture handling with drag threshold detection
- Auto-scroll timing and state management
- Wrap-around logic for infinite scrolling
- Scaling animations for active/inactive items

**ACarouselAutoScroll** (`Sources/ACarousel/ACarouselAutoScroll.swift`): An enum defining auto-scroll behavior with `.inactive` and `.active(TimeInterval)` cases.

### Supporting Utilities

**AppLifeCycleModifier** (`Sources/ACarousel/AppLifeCycleModifier.swift`): Cross-platform modifier that monitors app state changes to pause/resume auto-scrolling when the app becomes inactive/active.

**View+ReceiveTimer** (`Sources/ACarousel/View+ReceiveTimer.swift`): Extension providing optional timer subscription functionality for auto-scroll features.

### Key Architectural Patterns

- **MVVM Pattern**: Clear separation between view (`ACarousel`) and view model (`ACarouselViewModel`)
- **Reactive Programming**: Uses Combine publishers for timer events and app lifecycle notifications
- **Generic Design**: Supports any data type through Swift generics and protocol constraints
- **Cross-Platform Compatibility**: Uses conditional compilation (`#if os(macOS)`) for platform-specific code
- **State Management**: Uses `@Published` properties for reactive UI updates and `UserDefaults` for animation state persistence

### Offset and Animation System

The carousel uses a sophisticated offset calculation system:
- `itemActualWidth`: Individual item width plus spacing
- `defaultPadding`: Base padding from headspace and spacing
- `activeIndex`: Currently centered item (supports wrap-around indexing)
- Drag gestures update `dragOffset` with bounds checking
- Animation state is managed through `isAnimatedOffset` flag stored in UserDefaults

### Demo Structure

The `ACarouselDemo/` directory contains separate iOS and macOS demo applications that showcase the library's capabilities using One Piece character images. Both demos share similar `ContentView.swift` implementations but have platform-specific app lifecycle files.

## Package Configuration

The library is configured as a Swift Package with:
- Minimum deployment targets: iOS 13.0, macOS 10.15, tvOS 11.0
- No external dependencies
- Single library product targeting the `ACarousel` module