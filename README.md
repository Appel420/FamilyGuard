# FamilyGuard

**Privacy-first family safety app enforcing end-to-end encryption (E2EE) across devices.**

FamilyGuard is an on-device, privacy-respecting security platform for families. It combines voice-activated emergency controls, a companion screen-capture tool, and a cryptographic fortress protocol — all without cloud processing, persistent listening, or data collection.

---

## Features

- **Voice Command Integrity** — On-device speech recognition with a three-word activation sequence. Triggers emergency actions (kill-switch, total blackout) with strict exact-phrase matching to prevent false positives.
- **Real-Time Audio Analysis** — FFT-based child-voice detection and panic/fear tone filtering via Accelerate framework. A 20 dB spike combined with a child voice profile triggers an instant blackout.
- **Companion Screen Capture** — Lightweight browser-based snapshot tool that encrypts captures locally using the Web Crypto API. Voice-activated via the `remember` keyword.
- **Fortress Protocol** — Cryptographic data sealing using Argon2id password hashing, BLAKE3 checksums, post-quantum lattice key encapsulation, and TZNG compression — entirely on-device.
- **Zero Cloud** — No network calls, no telemetry, no background pings. All cryptographic operations happen locally using the Secure Enclave and on-device frameworks.

---

## Components

| File | Description |
|---|---|
| `FamilyGuard/VoiceCommandIntegrity.swift` | iOS/macOS voice command engine (Swift, Speech + AVFoundation + CryptoKit) |
| `companion.html` | Browser-based encrypted screen-capture companion (HTML/JS, Web Crypto API) |
| `fortress.html` | Reference implementation of the Fortress Protocol (Rust, BLAKE3 + Argon2id) |
| `Guard` | Extended Fortress Protocol documentation and design rationale |

---

## Requirements

### iOS / macOS (VoiceCommandIntegrity)

- iOS 16+ / macOS 13+
- Xcode 15+
- Swift 5.9+
- Frameworks: `Speech`, `AVFoundation`, `Accelerate`, `CryptoKit`
- `NSMicrophoneUsageDescription` and `NSSpeechRecognitionUsageDescription` in `Info.plist`

### Companion (companion.html)

- Any modern browser with `MediaDevices.getDisplayMedia` and `Web Crypto API` support (Chrome 72+, Firefox 66+, Safari 13+)
- No dependencies — fully self-contained HTML

### Fortress Protocol (fortress.html / Rust)

- Rust 1.75+
- Dependencies (add to `Cargo.toml`):
  ```toml
  [dependencies]
  blake3 = "1"
  argon2 = "0.5"
  rand = "0.8"
  ```
  > Note: `qresist` (post-quantum lattice) and `tzng` (compression) are conceptual crates shown for reference. Substitute with `pqcrypto` and `zstd` for production use.

---

## Privacy Guarantees

- No audio is recorded or stored — recognition happens in real-time and is discarded.
- No network requests are made from any component.
- Screen captures in the companion tool are encrypted with a per-session key derived from local state only.
- The Secure Enclave is used for key hashing in the Swift component.

---

## License

[LICENSE](LICENSE)
