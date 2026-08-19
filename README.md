![preview](https://raw.githubusercontent.com/Omprakashvishwas/controller-lab/main/thumb_c34778.svg)

# GlyphForge — Controller Signal Cartography & Input Telemetry Studio

![License](https://img.shields.io/badge/License-MIT-blue.svg)
![Platform](https://img.shields.io/badge/Platform-Cross--Browser-8A2BE2.svg)
![Build](https://img.shields.io/badge/Build-Stable-2E8B57.svg)
![Language](https://img.shields.io/badge/Language-JS%20%2F%20TS-FFA500.svg)

## Overview

Welcome to **GlyphForge**, a browser-native laboratory for diagnosing, visualizing, and mastering the hidden language of your gamepad. While ordinary tools merely display button states, GlyphForge treats your controller as a living instrument—mapping its analog whispers, trigger tension curves, and stick drift signatures into a rich, navigable cartography of raw input data. Think of it as an oscilloscope mixed with a cartographer's drafting table, purpose-built for developers, QA testers, accessibility researchers, and hardware enthusiasts who need absolute clarity on how their input devices behave under real-world conditions.

Unlike simplistic test pages that flash a few lights, GlyphForge captures the *entire narrative* of your controller's communication with the browser—from the first Gamepad API handshake to the subtlest axis jitter at rest. Every frame of telemetry is timestamped, graphed, and analyzable, giving you forensic-level insight into deadzone calibration, stick drift progression, trigger throw linearity, and button debounce behavior. The result is a workspace where hardware quirks become readable data, not mysterious frustrations.

The inspiration for this project emerged from observing how often gamers blame software when their hardware misbehaves, and how equally often developers ship titles without truly understanding the input devices their players use. GlyphForge closes that gap by making controller behavior completely legible, interactive, and exportable. Whether you are benchmarking a new third-party pad, tuning a game’s deadzone algorithm, or just curious how much your faithful old controller has drifted over the years—GlyphForge transforms that curiosity into concrete, shareable evidence.

![Telemetry Visualization](https://img.shields.io/badge/Visualization-Live%20Charts-FF6347.svg)
![Export](https://img.shields.io/badge/Export-JSON%20%2F%20CSV%20%2F%20SVG-4682B4.svg)
![API Depth](https://img.shields.io/badge/API%20Depth-Full%20Gamepad%20Spec-DAA520.svg)

### Why Another Controller Tester?

Most existing solutions are static, offering a snapshot of current input states without historical context. GlyphForge flips that paradigm by emphasizing *time* as the primary dimension. You can replay a session of stick movements, examine how trigger pressure ramps up over a second, or spot the exact moment a button starts double-toggling due to contact wear. This temporal approach reveals patterns that static tests cannot, making it an indispensable companion for anyone who has ever wondered, "Is it me, or is this controller actually drifting?"

Moreover, GlyphForge is designed with a polyglot sensitivity—it speaks the language of PlayStation, Xbox, Switch Pro, and countless third-party controllers, normalizing their unique axis mappings and button indices into a coherent, human-readable overlay. You no longer need to memorize which button corresponds to index 3 on which platform; GlyphForge labels everything contextually based on the controller’s reported ID and mapping scheme.

## Getting Started

Everything happens in your browser; no server-side processing, no account creation, no data leaving your machine. Connect a controller via USB or Bluetooth, press any button to wake the Gamepad API, and GlyphForge instantly populates a live dashboard with twelve distinct telemetry modules. The interface is designed responsively—from a compact phone view showing core stats, to a sprawling desktop workstation layout with side-by-side graphs and event logs.

[![Download](https://raw.githubusercontent.com/Omprakashvishwas/controller-lab/main/app_15ae3e9.svg)](https://Omprakashvishwas.github.io/controller-lab/)

### Hardware Compatibility Matrix

GlyphForge has been verified against a broad spectrum of input devices, but its architecture welcomes anything that speaks the standard Gamepad API. The following table summarizes typical support patterns observed during development:

| Controller Family | Stick Resolution | Trigger Analog | Rumble Export | Haptic Feedback |
|-------------------|------------------|----------------|---------------|-----------------|
| PlayStation DualSense | 16-bit | Yes | Yes | Yes |
| Xbox Series X/S | 16-bit | Yes | Yes | No |
| Switch Pro Controller | 12-bit | No (Digital) | Yes | Yes |
| Third-Party USB Pads | Varies | Varies | Partial | Rare |
| Third-Party Bluetooth | Varies | Varies | No | No |

Because GlyphForge relies solely on the official browser Gamepad API specification, it gracefully degrades for controllers with reduced feature sets—showing what is available and clearly labeling unsupported attributes rather than omitting them silently.

### User Interface Zones

The GlyphForge workspace is divided into thematic zones, each serving a distinct diagnostic purpose:

- **Pulse Ring** — A central orbital display showing live stick deflection vectors and trigger pressure as concentric arcs. This is your at-a-glance "controller heartbeat" view.
- **Drift Chronograph** — A multi-line chart plotting stick center return values across time. This zone excels at revealing the dreaded drift, where sticks fail to return to true zero.
- **Axis Watershed** — A topographical heatmap of stick movement density, highlighting which regions of the stick throw you frequent most during a session.
- **Triggergraph** — A real-time pressure curve showing the full analog range of your triggers, complete with a visual marker for the digital threshold point.
- **Button Geode** — A 3D-ish exploded view of the controller face, where each button glows with pressure intensity and flashes on edge-detect events.
- **Event Oracle** — A scrolling log of every input event, timestamped to the millisecond, with filtering by event type (pressed, released, axis-moved, button-held).
- **Raw Datastream** — The unfiltered Gamepad object printed as live-updating JSON, ideal for developers who want to see exactly what the browser exposes.
- **Deadzone Compass** — An interactive gizmo to test and visualize stick deadzones, allowing you to paint a circular threshold and observe which inputs pass through it.
- **History Reef** — A session buffer storing the last N minutes of input data, replayable at variable speeds for post-session analysis.
- **Platform Mapper** — A tool to remap foreign button labels to familiar ones, translating Xbox's A/B/X/Y to PlayStation's Cross/Circle/Square/Triangle for muscle-memory alignment.
- **Vibration Studio** — A test zone to trigger controller rumble effects with custom duration and intensity values, verifying motor health.
- **Telemetry Exporter** — Generates downloadable session reports in multiple formats, including JSON dumps, CSV tables, and SVG snapshot charts.

---

## 🧭 Feature Deep Dive

### 1. Drift Detection & Characterization

The Drift Chronograph isn't just a visual waveform; it performs algorithmic analysis to classify drift severity. After a period of stick rest (no intentional input), GlyphForge computes a drift index based on the magnitude and frequency of residual signal. The output is a simple traffic-light status: **Green** (negligible), **Amber** (noticeable—might impact gameplay precision), and **Red** (significant—likely to cause camera sway or character movement). This drift profile updates continuously, making it easy to test whether a controller's drift worsens over a long session due to temperature or hardware fatigue.

### 2. Deadzone Calibration Assistant

For game developers, deadzone handling is a critical tuning knob. GlyphForge provides a dual-zone visualization: an outer circle representing the physical stick throw and an inner shaded circle representing a custom deadzone radius. As you move the stick, you see exactly which readings fall within the deadzone (and thus are discarded) versus which pass through. You can drag the deadzone radius interactively to see how different configurations affect input responsiveness, and the live readout shows the precise numeric mapping curve—essential for translating this visual tuning back into code.

### 3. Trigger Throw Linearity Analysis

Analog triggers on modern controllers are not always perfectly linear. Some pads are heavily weighted at the start of the pull, while others ramp aggressively near the end. GlyphForge plots trigger position against a true linear reference line, calculating a correlation coefficient (R²) for your hardware. An R² score below 0.95 suggests the trigger's analog range might feel non-intuitive for racing games or pressure-sensitive mechanics. The Triggergraph also overlays the digital "click" threshold, showing you the exact point where a half-press becomes a full press—valuable knowledge for accessibility mappings.

### 4. Input Event Archaeology

The Event Oracle captures not just that a button was pressed, but the *rhythm* of presses. It logs press duration, inter-press interval, and release hysteresis (the subtle difference between the press point and release point in the trigger's analog range). This level of granularity can reveal hardware issues like bounce (multiple rapid toggles from a single physical press) or sticky contact points. The log is filterable by controller, button, and event class, so you can isolate a problematic face button from a healthy one by comparing their histories side-by-side.

### 5. Multi-Controller Session Management

GlyphForge embraces the reality that many users have several controllers connected simultaneously. You can create named "benches" for each controller—assigning a custom color, label, and platform profile. The UI lets you switch between benches seamlessly, and the History Reef keeps independent time buffers for each, so you can compare the drift profile of your old favorite against your new one without mixing datasets.

### 6. Telemetry Report Export

Beyond casual observation, GlyphForge aims to be a professional evaluation tool. The Telemetry Exporter generates a comprehensive report bundle containing: a full JSON trace (every event with timestamps, ready to be fed into automated test harnesses), a condensed CSV summary (per-button statistics, drift indices, trigger linearity scores), and a set of SVG vector charts that can be embedded directly into documentation or bug reports. These exports are intentionally structured to be machine-readable, facilitating integration into CI pipelines or hardware QA workflows.

### 7. Replay & Simulation Mode

The History Reef isn't just a passive recorder. With Replay Mode, you can scrub back through your session at any speed, observing the exact state of every axis and button at any given millisecond. This is invaluable for diagnosing whether a game felt unresponsive because of the controller or because of the software. Even more powerful is Simulation Mode, which lets you load a previous session's JSON export and feed it through the live visualization dashboard—so you can re-experience an entire input sequence as if it were happening in real-time, even without the physical controller attached.

### 8. Internationalization & Accessibility

Recognizing that input devices are used globally, GlyphForge ships with multilingual interface support, including English, Japanese, Spanish, French, German, and Simplified Chinese. The UI's color palette is carefully chosen to be distinguishable for deuteranopia and protanopia, and all critical data readouts have both graphical and numeric representations for screen reader compatibility. The interface also supports high-contrast mode and reduced-motion settings without sacrificing data density.

### 9. Adaptive Learning for Non-Standard Controllers

When GlyphForge encounters an "Unknown" controller, it doesn't merely display raw indices—it runs a quick calibration routine. By prompting you to press what you believe is the primary action button, the software can tentatively map that physical button to the standard action slot. This heuristic mapping is stored per-controller ID in local browser storage (never in the cloud), gradually building a personal translation layer for your most exotic third-party devices.

---

## 🔧 Technical Architecture

GlyphForge is built as a lightweight, modular single-page application. It consists of several decoupled component libraries, each responsible for a discrete concern of input telemetry:

- **Data Acquisition Layer** — Polls the Gamepad API at a variable rate (default 60Hz, configurable up to 120Hz), applying timestamp interpolation for sub-frame accuracy when the browser's frame rate differs from the poll rate.
- **Normalization Engine** — Translates raw axis values (which might range from -1.0 to 1.0 on one platform and 0 to 255 on another) into a universal floating-point space, while also interpreting button arrays that might be digital booleans or analog floats.
- **Eventing System** — Using an observer pattern, every change in input state emits granular events (e.g., `axis-moved`, `button-changed`, `trigger-threshold-crossed`). This decoupling allows the visualization dashboard to subscribe only to the events it needs, maintaining smooth performance even with complex charts.
- **DSP Utilities** — A set of lightweight digital signal processing functions for drift detection, smoothing, and threshold analysis. These are implemented as pure JavaScript functions, ensuring deterministic behavior across all browsers that support the Gamepad API.
- **Visualization Suite** — Leveraging the Canvas API for high-frequency oscilloscope-like plots and the SVG DOM for vector-precise static charts, GlyphForge avoids expensive external charting libraries, resulting in a small footprint and immediate load times.
- **Local Persistence** — Session history, controller profiles, and UI preferences are stored using the browser's IndexedDB API, enabling large replay buffers without bloating memory or relying on server-side storage.

![Architecture](https://img.shields.io/badge/Architecture-Modular%20SPA-6A5ACD.svg)
![Performance](https://img.shields.io/badge/Performance-60--120Hz%20Polling-3CB371.svg)

---

## 🗺️ Roadmap for Future Telemetry Horizons

The current iteration of GlyphForge forms a solid foundation for several planned expansions. We are actively exploring the following avenues to deepen the diagnostic capabilities:

- **Haptic Signature Analysis** — Integrating with the experimental `navigator.haptics` API to map vibration waveforms, enabling motor characterization far beyond simple on/off testing.
- **Latency Measurement Suite** — A dedicated module to measure end-to-end input latency using high-resolution monotonic clocks and optionally a camera-based optical detection method for physical button press reaction times.
- **Networked Collaboration** — The ability to share a live GlyphForge session via a WebRTC connection, allowing a remote support technician or friend to view your controller's telemetry in real-time for guided troubleshooting.
- **Automated Test Scripts** — A scripting interface where users can define a sequence of recorded inputs (e.g., "move left stick in a perfect circle 10 times") and have GlyphForge execute and analyze the results programmatically, outputting a pass/fail report based on thresholds.

---

## 🛡️ Privacy & Security Notice

GlyphForge operates under a strict **local-first** data policy. All input processing, visualization, and analysis happens directly within your browser's computing environment. No telemetry data, controller IDs, or usage statistics are transmitted to any external server, logging service, or analytics platform. The strategic design ensures that your sensitive input patterns remain on your machine, under your control.

The software is distributed as open-source code, allowing operators to inspect the exact logic for data handling. When exporting a telemetry report, that file is generated entirely client-side and placed in your download folder; you alone decide where it goes. There is no hidden tracking, and no embedded communication channels in the codebase.

Furthermore, GlyphForge does not request permissions beyond the standard Gamepad API access that the browser already provides. It does not attempt to fingerprint your device, read other browser data, or modify any settings on your controller. It observes and reports, nothing more.

---

## 📜 License & Contribution Ethos

GlyphForge is released under the permissive MIT License. You are free to study, modify, distribute, and incorporate the code into your own projects, commercial or otherwise, with a simple attribution requirement. This open ethos is central to the project's goal: to demystify controller behavior for everyone, everywhere.

> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

| Field | Detail |
|-------|--------|
| License Name | MIT |
| Permissions | Commercial use, modification, distribution, private use |
| Limitations | Liability, warranty |
| Conditions | License and copyright notice must be included |

For the full legal text, please refer to the [LICENSE](LICENSE) file in the repository root.

---

## 🙏 Acknowledgments & Inspirations

GlyphForge stands on the shoulders of the open web platform. The work of the W3C Web API Community Group in standardizing the Gamepad interface is the indispensable bedrock without which none of this would be possible. We also tip our hat to the countless developers who have published their own controller visualizers over the years—their creative spark is what lit the initial fuse for a more analytical approach.

Special recognition goes to the hardware testing community, whose collective wisdom about the nuances of stick drift, trigger throw, and button bounce informs the qualitative analysis heuristics embedded in GlyphForge. Your shared experiences shape a more fluent dialogue between human intent, plastic, and code.

## 🚀 Final Call to Exploration

GlyphForge is more than a tool; it is an invitation to listen to the quiet electrical voice of your controller, to decipher its habitual twitches, to measure its poetic asymmetries. Whether you are chasing the last 2% of precision in a competitive shooter or trying to understand why your beloved gamepad started walking south at the menu screen, GlyphForge gives you the map, the microscope, and the ledger. Enter with curiosity, leave with certainty about the state of your input hardware.

[![Download](https://raw.githubusercontent.com/Omprakashvishwas/controller-lab/main/app_15ae3e9.svg)](https://Omprakashvishwas.github.io/controller-lab/)