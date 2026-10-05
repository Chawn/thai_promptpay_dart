# PLAN — thai_promptpay (Dart / Flutter)

Status: **M0 scaffolded** · Owner: @Chawn · Last updated: 2026-10-06

## Goal
Pure-Dart PromptPay / Thai QR Payment payload **generator and parser**, API-compatible
with `promptpay-go`, plus a Flutter widget package that renders the QR with the official
Thai QR Payment look. Target users: Flutter POS, shop, donation and invoicing apps.

## Non-goals
- Payment confirmation / bank APIs / slip verification.
- Scanning (camera) — users combine our `decode()` with `mobile_scanner`; we document how.

## Design
- Core package `thai_promptpay` (pure Dart, zero deps, works on web).
- Flutter package `thai_promptpay_flutter` in `flutter/` (depends on core + `qr_flutter`):
  `PromptPayQr(target: ..., amount: ...)` widget with optional header/logo frame.
- **Check pub.dev for name collisions** (`promptpay`, `promptpay_qr` may exist) before M6.
- Spec and test vectors are shared with `promptpay-go` — copy `testdata/promptpay_vectors.json`
  byte-identical; see that repo's PLAN.md for the tag table.

## API (v0.1.0)
```dart
enum TargetKind { mobile, nationalId, eWallet }
class PromptPayTarget { final TargetKind kind; final String id; factory PromptPayTarget.parse(String s); }

String promptPayPayload(PromptPayTarget target, {int? amountSatang, String? merchantName, String? city, String? reference});
String promptPayPayloadFromAmount(PromptPayTarget target, num amount); // convenience, rounds to satang
String billPaymentPayload({required String billerId, required String ref1, String? ref2, int? amountSatang});

class DecodedPayload { bool isStatic; PromptPayTarget? target; int? amountSatang; BillPayment? bill; Map<String,String> raw; }
DecodedPayload decodePayload(String payload); // throws FormatException on bad CRC/structure
int crc16(List<int> bytes);
```

## Milestones
- [x] **M0 — Scaffold**
- [ ] **M1 — TLV + CRC16** (`crc16(utf8.encode('123456789')) == 0x29B1`).
- [ ] **M2 — Credit transfer** — all shared vectors byte-identical.
- [ ] **M3 — Decoder** + round-trip tests + random-input test (never throws anything but FormatException).
- [ ] **M4 — Bill payment / Tag 62**.
- [ ] **M5 — Flutter widget package** with golden tests.
- [ ] **M6 — Release v0.1.0** on pub.dev (160 pub points), example app GIF in README.

## Definition of done
`dart format`, `dart analyze --fatal-infos`, `dart test`, `dart pub publish --dry-run` clean; PLAN ticked.
