# thai_promptpay

> **Status: pre-alpha — under active development. APIs will change until v0.1.0.**
> See [PLAN.md](PLAN.md) for the roadmap.

PromptPay / Thai QR Payment payload generator & decoder for Dart and Flutter.

สร้างและอ่าน QR พร้อมเพย์สำหรับ Flutter/Dart พร้อม widget แสดง QR

## Install
```sh
dart pub add thai_promptpay
```

## Usage (target API)
```dart
import 'package:thai_promptpay/thai_promptpay.dart';

final payload = promptPayPayload(PromptPayTarget.parse('0812345678'), amountSatang: 10000);
```

## Development
```sh
dart pub get
dart format .
dart analyze --fatal-infos
dart test
```

## Contributing
Issues and PRs welcome — see [CONTRIBUTING.md](CONTRIBUTING.md). Thai or English.

## License
MIT © Chawn and contributors
