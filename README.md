# Awesome macOS Engineering 🍏

A curated directory of advanced macOS engineering techniques, system windowing internals, notch & display geometry, AppKit/SwiftUI bridging, and developer utilities.

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![License: CC0-1.0](https://img.shields.io/badge/License-CC0_1.0-lightgrey.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

---

## Contents

- [Windowing & Panels](#windowing--panels)
- [Notch & Display Metrics](#notch--display-metrics)
- [Menu Bar Architecture](#menu-bar-architecture)
- [Hardware & System Introspection](#hardware--system-introspection)
- [Swift Concurrency on Darwin](#swift-concurrency-on-darwin)
- [Open Source macOS References](#open-source-macos-references)

---

## Windowing & Panels

Engineering non-activating, floating, and overlay windows requires deep AppKit mechanics:

- **Borderless NSPanel**: Create unobtrusive utility overlays using `styleMask: [.borderless, .nonactivatingPanel]`.
- **Window Levels**:
  - `NSWindow.Level.floating` — standard floating tool palette.
  - `NSWindow.Level.mainMenu + 1` — overlays that float on top of the macOS Menu Bar.
  - `NSWindow.Level.screenSaver` — lock-screen visible surfaces.
- **Collection Behaviors**:
  - `.canJoinAllSpaces` — Persists across virtual desktop spaces without reloading.
  - `.fullScreenAuxiliary` — Displays over native macOS full-screen applications.
  - `.stationary` — Excluded from Mission Control window tiling.

---

## Notch & Display Metrics

Modern MacBook Pros (M1/M2/M3/M4) feature camera notches that introduce unique layout constraints:

- **`NSScreen.safeAreaInsets`**: When `safeAreaInsets.top > 0`, the display contains a physical notch.
- **`auxiliaryTopLeftArea` / `auxiliaryTopRightArea`**: The notch width equals:
  $$\text{Notch Width} = \text{Screen Width} - (\text{Left Area Width} + \text{Right Area Width})$$
- **NotchKit**: Lightweight Swift package for inspecting notch dimensions: [`swiftui-notch-kit`](https://github.com/nilkanthdesai76/swiftui-notch-kit).

---

## Menu Bar Architecture

- **`NSStatusItem` vs SwiftUI `MenuBarExtra`**:
  - SwiftUI `MenuBarExtra` is great for simple menus, but lacks custom window levels, mouse tracking outside borders, and fine-grained positioning.
  - Hybrid Approach: Use `NSStatusItem` + custom `NSPanel` hosting SwiftUI `NSHostingView`.
- **Starter Template**: [`menubar-swiftui-template`](https://github.com/nilkanthdesai76/menubar-swiftui-template).

---

## Hardware & System Introspection

- **`IOKit` & `SMC`**: Querying fan speed, CPU temperature, and thermal state without elevated privileges.
- **`CoreMedia` / `AVFoundation`**: Low-latency video capture, virtual webcam inputs, and screen capture (`SCStream`).
- **`MediaRemoteAdapter`**: Bridging Now Playing media state and playback metadata on macOS 15+.

---

## Swift Concurrency on Darwin

- **`@MainActor` with AppKit**: All UI mutations (`NSWindow`, `NSView`, `NSStatusItem`) must remain bound to the MainActor.
- **`Sendable` Types**: Thread-safe encapsulation for inter-actor state transfer.
- **Async Networking**: Type-safe async/await client with retry policies: [`swift-modern-networking`](https://github.com/nilkanthdesai76/swift-modern-networking).

---

## Open Source macOS References

- [Rectangle](https://github.com/rxhanson/Rectangle) — Window management app written in Swift.
- [Ice](https://github.com/jordanbaird/Ice) — Powerful menu bar manager for macOS.
- [MonitorControl](https://github.com/MonitorControl/MonitorControl) — Controls external monitor brightness & volume.
- [Stats](https://github.com/exelban/stats) — macOS system monitor in your menu bar.

---

## Contributing

Contributions, fixes, and recommendations are welcome! Please submit a pull request or open an issue.

## License

Dedicated to the public domain under [Creative Commons Zero (CC0 1.0)](LICENSE).
